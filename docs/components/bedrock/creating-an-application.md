# Creating an http application

To create an application object, you need 3 things, a container PSR-11, a kernel with and an handlers resolvers.

```
$container = new Container(); // PSR-11
$kernel = new Kernel('prod', $container);
$app = new Application(
    $kernel,
    new HandlerResolver(),
    '2.0.0', // optional, default is 1.0
)
```


### How it works ?

The application is essentially a stack of resources acting as request middleware and handler. 
When a PSR-7 request is send to the Application, the main stack is resolved and executed. 

There is two outcomes possible when the main stack is resolved:
 
1. It stop when a PSR-7 response is returned.
2. It throw an exception because no response have been returned.

### How to build your stack

Adding stuff to your Application stack in done with method `add()` and `set()`.
They work both the same way except that `add()` append stuff the stack and  `set()` overwrite what was previously added to the stack.

``` 
$app->add(MiddlewareA::class);
$app->add([
    MiddlewareB::class, 
    MiddlewareC::class
]);
```
If we were processing request here, middlewares A, B and C would have been executed.

### How to handler a server request 

To handle an Application server request, you need a compatible PSR-7 library. By default, Peak come with Zend/Diactoros, but you can use many others.

Example with Zend/Diactoros
```
$request = ServerRequestFactory::fromGlobals();

try {
    $response = $app->handle($request);
    // ...
} catch(Exception $e) {
    // ...
}
```


    




