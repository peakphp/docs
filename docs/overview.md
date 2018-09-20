# Overview

### How to start

To create an new application instance, you need at least 3 things:

 - a **Kernel** (*Peak\Blueprint\Bedrock\Kernel*) 
 - a **Container** PSR-11 (*Psr\Container\ContainerInterface*)
 - a **Resource resolvers** (*Peak\Blueprint\Common\ResourceResolver*)
 - (optional) A **Dictionary** for properties (*Peak\Blueprint\Collection\Dictionary*)

```php
$app = new Application(
    new Kernel('prod', new Container()),
    new HandlerResolver(),
    new PropertiesBag([ // optional
        'version' => '1.0', 
        'name' => 'app'
    ]) 
)
```

### How it works ?

The application is essentially a stack of resources acting as request middleware and handler. 
When a *PSR-7* request is send to the `Application`, the main stack is resolved and executed. 

There is two outcomes possible when the main stack is resolved:
1. It stop when a *PSR-7* response is returned.
2. It throw an exception because no response have been returned.

<img src="https://raw.githubusercontent.com/peakphp/docs/master/pencils/request_response_flow.png" alt="Peak">

### How to build your stack

Adding stuff to your `Application` stack in done with method `stack()` and `set()`.
They work both the same way except that `stack()` append stuff and  `set()` overwrite what was previously added to the stack.

```php
$app->stack(MiddlewareA::class);
$app->stack([
    MiddlewareB::class, 
    MiddlewareC::class
]);
```
If we were processing request here, middlewares A, B and C would have been executed.

### How to process a server request 

To handle an `Application` server request, you need a compatible *PSR-7* library. 

By default, Peak come with Zend/Diactoros, but you can use many others.

Example with Zend/Diactoros:
```php
$request = ServerRequestFactory::fromGlobals();

try {
    $response = $app->handle($request);
    // ...
} catch(Exception $e) {
    // ...
}
```

### How to output the response

The easiest way to output a *PSR-7* response is to use `Peak\Bedrock\Http\Response\Emitter`.

```php
$response = $app->handle($request);

// ... do something with the response

$emitter = new Emitter();
$emitter->emit($response);
```

Or use Application::run()

```php
$app->run($request, new Emitter());
```

### Complete Application example from A to Z

Now that you know how an `Application` works, lets try a more real usage. 
We will use `ApplicationFactory` to create our app.

```php
use Peak\Di\Container; // PSR-11
use Peak\Bedrock\Application\Application; // PSR-15
use Peak\Bedrock\Http\Response\Emitter;
use Zend\Diactoros\ServerRequestFactory; // PSR-7

try {

    $app = new Application(
        new Kernel('prod', new Container()),
        new HandlerResolver(),
        new PropertiesBag([ // optional
            'version' => '1.0', 
            'name' => 'app'
        ]) 
    )
    
    $app->all('/', [
        HomePageHandler::class
    ]);
    
    $app->get('/user/([a-zA-Z0-9]+)', UserProfileHandler::class);
    
    $app->post('/userForm/([a-zA-Z0-9]+)', [
        AuthenticationMiddleware::class,
        CSRFTokenMiddleware::class,
        UserFormHandler::class,
    ]);
    
    $app->stack(PageNotFoundHandler::class);

    // create request from globals
    $request = ServerRequestFactory::fromGlobals();
    
    // handle request and emit app stack response
    $app->run($request, new Emitter());
    
} catch(Exception $e) {
    // overwrite app stack with error middleware
    $app->set(new DevExceptionHandler($e))
        ->run($request ?? ServerRequestFactory::fromGlobals(), new Emitter());
}
```
    




