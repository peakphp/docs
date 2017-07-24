### Definitions (Autowiring disabled)

For small and medium projects, autowiring can do the job correctly, but as your project grow, you may want to have more control over your objects creations.

This can be done with methods:

- ```bind()```
- ```bindPrototype()```
- ```bindFactory()```

To use definitions with ```create()```, you need to disable autowiring which is enabled by default:


```PHP
$container->disableAutowiring();
```

 

### Singleton definition with ```bind()```
The default singleton definition binding is an object that is first created than stored and reused.

```PHP

$container->disableAutowiring();

$container->bind(Foo::class, new Foo);
$foo = $container->create(Foo::class);

$foo->bar = 'foo';

$other_foo = $container->create(Foo::class);
// $foo === $other_foo
```

### Factory definition with ```bindFactory()```
A factory definition accept a callable definition that is executed each time.

```PHP

$container->bindFactory(Finger::class, function (Container $c, $args) {
    /* do some stuff and then return a object */
    return new Finger(
        new A, 
        'factory', 
        'bar'
    );
});

// create foo successfully
$foo = $container->create(Foo::class);
$foo2 = $container->create(Foo::class);
// $foo !== $foo2
```

### Prototype definition with ```bindFactory()```
Prototype accept various definitions and always try to return a new instance of dependencies. ```bindPrototype()``` will ignore stored instance(s) and definition(s) in the container.

```PHP

class Hand {}

class Arm {
    public function __construct(Hand $hand) {
        // ...
    }
}

class Chest {
    public function __construct(Arm $arm, $argv = null) {
        // ...
    }
}

$container->bindPrototype(Chest::class, [
    Chest::class,
    Arm::class => [
        Arm::class,
        Hand::class,
    ],
    'bar',
]);

$chest = $container->create(Chest::class);
// $chest will always contain a new instance of Arm and Arm will 
// always contains a new instance of Hand and so on. 
```

### Definitions type
There is many way you can declare definition. Here a list of accepted definition for each one:

- ```bind()``` : callable, classname string, object, array of definition
- ```bindFactory()``` : callable only
- ```bindPrototype()``` : classname string, array of definition