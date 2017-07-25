## Using Autowiring

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
In example above, a new ``Bar`` instance is created automatically each time ```Foo``` is created. This mechanism rely on ```Reflection``` to resolve objects dependencies. This is the default behavior of Peak Di.


### Reuse a class instance

By default, method create() will always look for stored instance of Bar before creating a new one.
You can store an class instance in the container with ```add()```.

```PHP
$bar = new Bar();
$bar->name = "John Bar";

$container->add($bar);

$foo1 = $container->create(Foo::class);
$foo2 = $container->create(Foo::class);
```

In example above, ```$foo1``` and ```$foo2``` will have the same instance of ```Bar```.

```PHP
echo $foo1->bar->name; //output: John Bar
echo $foo2->bar->name; //output: John Bar
```

### Get a stored class instance

You can get a stored class instance by using ```get()```.

```PHP
$container->add(new Monolog\Logger);
$logger = $container->get(Monolog\Logger::class);
```

### Use alias for stored class instance


```PHP
// when adding (shorter)
$container->add(new Monolog\Handler\StreamHandler(), 'LogStream');
$stream = $container->get('LogStream');

// or with addAlias()
$container->addAlias(Monolog\Handler\StreamHandler::class, 'LogStream');
$container->add(new Monolog\Handler\StreamHandler);
$stream = $container->get('LogStream');
```

### Call an class instance method

You can also resolve dependencies of a object method with```call()```. It work like method ```create()``` except for the first parameter which must be an array containing the object instance and the method string name. 

```PHP
class Foo {
    public function add(Bar $bar, $alias) {
        return $bar;
    }
}

$foo = new Foo;
$bar = $container->call([
    $foo,
    'add'
]);
```