---
title: Middlewares and Handlers - Core concepts
sb: core-concepts
---

# Middlewares and Handlers

Middlewares and handlers are essentials parts of Peak Applications stack. Their job is to simply receive a request, do stuff and call the next middleware or return a response whenever possible. They are also fully compatible with [PSR-7](https://www.php-fig.org/psr/psr-7/) and [PSR-15](https://www.php-fig.org/psr/psr-15/)

<img src="https://raw.githubusercontent.com/peakphp/docs/master/_pencils/request_response_flow.png" alt="Peak">


### Middleware example: 


```php
use Psr\Http\Server\MiddlewareInterface;
use Psr\Http\Server\RequestHandlerInterface as Handler;
use Psr\Http\Message\ServerRequestInterface as Request;
use Psr\Http\Message\ResponseInterface as Response;

class ClimbMiddleware implements MiddlewareInterface
{
    /**
     * Process an incoming server request and return a response, optionally delegating
     * response creation to a handler.
     */
    public function process(Request $request, Handler $handler): Response 
    {
        // ...
        return $handler->handle($request);
        // or 
        return new Response('Hello mountains!');
    }
}
```

Note: Middleware can also return a Response when appropriate, and when it happens, the chain of middlewares is stopped.

### Handler example: 

The main difference with handlers is that you must return a response. You cannot pass the request to the next middleware.

```php
use Psr\Http\Server\RequestHandlerInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Message\ResponseInterface;

class HandlerA implements RequestHandlerInterface
{
    /**
     * Handle the request and return a response.
     */
    public function handle(ServerRequestInterface $request): ResponseInterface
    {
        // ...
        return new Response('Hello mountains!');
    }
}  
```

### Closure middleware
You can also write middlewares and handlers with closure. Underneath, they will be wrapped in `CallableMiddleware` which is *PSR-15* compliant. The only downside is that closure are less reusable outside your application. Also, it is generally not a good idea to use closure for complex middleware, but for small task and prototyping, they comes really handy.

```php
$myMiddleware = function ($request, $handler) {
    // ...
    return $handler->handle($request);
}

$myHandler = function ($request) {
    // ...
    return new Response('...');
}
```