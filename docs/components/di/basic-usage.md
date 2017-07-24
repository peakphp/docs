## Autowiring

No configuration needed. Just type-hint your constructor parameters and the container can guess which dependencies to inject.

```PHP
class Bar {}
class Foo {
    public $bar;
    public function __construct(Bar $bar, $id = null, $alias = null) {
        $this->bar = $bar;
    }
}

$container = new Container;
$foo = $container->create(Foo::class);
```
In example above, a new ``Bar`` instance is created automatically each time ```Foo``` is created. This mechanism rely on ```Reflection``` to resolve objects dependencies.


### Reuse a class instance by storing it in the container with ```add()```
By default, method create() will always look for stored instance of Bar before creating a new one.

```PHP
$bar = new Bar();
$bar->name = "John Bar";

$container->add($bar);

$foo1 = $container->create(Foo::class);
$foo2 = $container->create(Foo::class);
```

In example above, ``$foo1`` and ``$foo2`` will have the same instance of ``Bar``.

```PHP
echo $foo1->bar->name; //output: John Bar
echo $foo2->bar->name; //output: John Bar
```

### Get a stored object instance with ```get()```

```PHP
$container->add(new Monolog\Logger);
$logger = $container->get(Monolog\Logger::class);
```

### Use alias for class name

```PHP
$container->add(new Monolog\Handler\StreamHandler(), 'LogStream');
$stream = $container->get('LogStream');
```

### Call an object method with ```call()```
You can also resolve dependencies of a object method with```call()```. It work like method ```create()``` except for the first parameter which must be an array containing the object instance and the method string name. 

```PHP
class Events {
    public function method(Bar $bar, $alias = null) {
        return $bar;
    }
}

$events = new Events;
$bar = $container->call([
    $events,
    'method
]);
```