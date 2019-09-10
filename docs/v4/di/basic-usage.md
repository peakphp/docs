---
id: di-basic-usage
title: Dependency Injection - Basic Usage
sb: sidebar/docs.html
---
# Dependency Injection

### Basic usage

### Store a class instance with ``set()``

You can store an class instance in the container with ```set()```.

```php
$container->set(new ClimbingSession());

$class1 = $container->get(ClimbingSession::class);
$class2 = $container->get(ClimbingSession::class);
```

In example above, ```$class1``` and ```$class2``` will be the same instance of ClimbingSession.

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