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

### Definitions type

There is many way you can declare definition. Here a list of accepted definition for each one:

- ```bind()``` : callable, classname string, object, array of definition
- ```bindFactory()``` : callable only
- ```bindPrototype()``` : classname string, array of definition

### Singleton

The default singleton definition binding is an object that is first created than stored and reused.

```PHP
$container->disableAutowiring();

$container->bind(Foo::class, new Foo);
$foo = $container->create(Foo::class);

$foo->bar = 'foo';

$other_foo = $container->create(Foo::class);
// $foo === $other_foo
```

### Factory

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

### Prototype

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

### How Array of definition work

Array of definition represent a powerfull way to describe and group how dependencies can be resolve for a definition. It also support nested definition.

Inside the array of definition, supported type are: callable, classname string, object and array of definition. 

The first item of the array always represent the class to instantiate, other represent constructor argument(s).

```PHP
class A {
    public function __construct(B $b, C $c, $id) {
        // ...
    }
}

class B {
    public function __construct(D $d, $name) {
        // ...
    }
}

class C {}
class D {}

$container->bind(A::class, [
    A::class, // represent the class to instantiate
    B::class => [ // nested definition for $b
        B::class, // represent the class to instantiate
        D::class, // $d
        'JohnDoe' // $name
    ],
    C::class, // $c
    123  // $id
]);

$a = $container->create(A::class);

// what php equivalent look like (without stored instance(s))
$a = new A(
    new B(
        new D,
        'johndoe'
    ),
    new C,
    123
);
```

In the example above, the main difference between ```bind()``` and php vanilla is that ```bind()``` will look for stored instance or definition to resolve dependency before creating a new instance. To reproduce the same behavior with php vanilla, bind() should be replaced by ```bindPrototype()```.  

A more concrete example would be something like:


```PHP
class Database {
    public function __construct(DatabaseConfiguration $config) {
        // ...
    }
}

// bind a singleton for database configuration
$container->bind(DatabaseConfiguration::class, function(ContainerInterface $c) {
    return new DatabaseConfiguration(
        'locahost', 
        'dbexample', 
        'root', 
        'root'
    );
});

$container->bind(Database::class, [
    // represent the class to instantiate
    Database::class, 
    // will resolve DatabaseConfiguration::definition
    // and return the same instance of DatabaseConfiguration each time
    DatabaseConfiguration::class  
    
]);


$db = $container->create(Database::class);

```