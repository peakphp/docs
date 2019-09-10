---
id: di-autowiring
title: Dependency Injection - Autowiring
sb: sidebar/docs.html
---
# Dependency Injection

### Autowiring

No configuration needed. Just type-hint your constructor parameters and the container can guess which dependencies to inject.

```php
class Bar {}
class Foo {
    public $bar;
    public function __construct(Bar $bar, $id = null, $alias = null) {
        $this->bar = $bar;
    }
}

$container = new Container;
$foo = $container->get(Foo::class);
```
In example above, a new ``Bar`` instance is created automatically each time ```Foo``` is created. This mechanism rely on ```Reflection``` to resolve objects dependencies. This is the default behavior of Peak Di.

Autowiring will try to create the object instance if no stored instance or matching definition found.

You can get a stored class instance by using ```get()```. If no instance can be found, it will check definitions and finally use autowiring in last resort.

Under the hood, the flow of ``get()`` is:
 1. Check for stored instance that match class or interface
 2. Check for definition
 3. Try to resolve class with autowiring
 4. Throw exception
 
You can disable the autowiring to prevent step ``3`` and let the container throw an exception if no definition match the requested object. 

```php
$container->disableAutowiring();
```

### Limitation of autowiring

Autowiring cannot always resolve dependencies like interfaces without help. 

Consider this example:

```php
interface Widget {}
class Toolbar implements Widget {}
class WidgetDecorator
{
    public function __construct(Widget $myWidget) { ... }
}

$widgetDecorator = $container->get(WidgetDecorator::class);
```

Trying to resolve ``WidgetDecorator`` will throw an ``Exception`` because ``Widget`` is an interface. There is 2 way to resolve this:

1. The first way is to create a ``Toolbar`` instance and store it in your container before you try to resolve ``WidgetDecorator``:
```php
$toolbar = new Toolbar();
$container->set($toolbar);
$widgetDecorator = $container->get(WidgetDecorator::class);
// $toolbar instance will be used to create $widgetDecorator 
```

2. The second way is to tell the container which class should be used when encountering ``Widget`` interface dependency:
```php
$container->bind(Widget::class, Toolbar::class);
$widgetDecorator = $container->get(WidgetDecorator::class);
```

The 2 solutions above achieve the same result but there are subtilities that are good to know. Learn more about definitions [here](/docs/v4/di/definitions).