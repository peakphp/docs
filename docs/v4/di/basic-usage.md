---
id: di-basic-usage
title: Dependency Injection Basic Usage
sb: sidebar/docs.html
---
# Dependency Injection

### Basic usage

##### Using Autowiring

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
 1. Check for stored instance
 2. Check for definition
 3. Try autowiring
 4. Throw exception
 
You can disable the autowiring to prevent step ``3`` and let the container throw an exception if no definition match the requested object. 

```php
$container->disableAutowiring();
```

### Store a class instance with ``set()``

You can store an class instance in the container with ```set()```.

```php
$container->set(new ClimbingSession());

$foo1 = $container->get(ClimbingSession::class);
$foo2 = $container->get(ClimbingSession::class);
```

In example above, ```$foo1``` and ```$foo2``` will have the same instance of ```Bar```.

```php
echo $foo1->bar->name; //output: John Bar
echo $foo2->bar->name; //output: John Bar
```

### Verify a stored class instance with ``has()``

You can check if the container has particular stored class instance. 

```php
if ($container->has(Monolog\Logger::class)) {
    // ...
}
```

### Delete a stored class instance with ``delete()``


```php
$container->delete(Logger::class);
```

### Use aliases for class instance


```php
// when setting (shorter)
$container->set(new Monolog\Handler\StreamHandler(), 'LogStream');
$stream = $container->get('LogStream');

// or with addAlias()
$container->addAlias(Monolog\Handler\StreamHandler::class, 'LogStream');
// ...
$container->set(new Monolog\Handler\StreamHandler);
$stream = $container->get('LogStream');
```

### Resolve a class method dependencies with ``call()``

You can also resolve dependencies of a object method with```call()```.

```php
class Foo {
    public function method(Bar $bar, $arg) {
        // ...
        return 'ok';
    }
}

$foo = new Foo();
$response = $container->call([ $foo, 'method' ], 123);
echo $response; // output 'ok'
```