---
id: di-definitions
title: Dependency Injection - Definitions
sb: sidebar/docs.html
---
# Dependency Injection
### Definitions

For small and medium projects, autowiring can do the job correctly, but as your project grow, you may want to have more control over your objects creations.

This can be done by binding definitions to class name with following methods:

- ```bind()```
- ```bindPrototype()```
- ```bindFactory()```

##### Only use definitions

When requesting a object, by default, if no definitions is found for the requested object, autowiring will try to create the object instance. Disable the autowiring to prevent this behavior. This will let the container throw an exception if no definition match the requested object. 

```php
$container->disableAutowiring();
```

### Definitions type

There is many way you can declare definition. Here a list of accepted definition for each one:

- ```bind()``` : callable, classname string, object, array of definition
- ```bindFactory()``` : callable only
- ```bindPrototype()``` : classname string, array of definition

### Singleton bind

The default singleton definition binding is an object that is first created than stored and reused.

```php
$container->disableAutowiring();

$container->bind(Foo::class, new Foo);
$foo = $container->get(Foo::class);
$other_foo = $container->get(Foo::class);
// $foo === $other_foo
```


### Factory bind

A factory definition accept a callable definition that is executed each time.

```php
$container->bindFactory(Finger::class, function (Container $c, $args) {
    /* do some stuff and then return a object */
    return new Finger(
        new A, 
        'factory', 
        'bar'
    );
});

$foo = $container->get(Foo::class);
$foo2 = $container->get(Foo::class);
// $foo !== $foo2
```

### Prototype bind

Prototype accept various definitions and always try to return a new instance of dependencies. ```bindPrototype()``` will ignore stored instance(s) and definition(s) in the container.

```php
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

$chest = $container->get(Chest::class);
// $chest will always contain a new instance of Arm and Arm will 
// always contains a new instance of Hand and so on. 
```


### Array of definition

Array of definition represent a powerful way to describe and group how dependencies can be resolve for a definition. It also support nested definition and passing normal argument(s) value(s).

Supported types supported in an array of definition: callable, classname string, object and array of definition. 

When using class name string, the first item of the array always represent the class to instantiate, other represent constructor argument(s).

```php
class A {
    public function __construct(B $b, C $c, $id) { ... }
}

class B {
    public function __construct(D $d, $name) { ... }
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

$a = $container->get(A::class);

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


```php
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


$db = $container->get(Database::class);

```