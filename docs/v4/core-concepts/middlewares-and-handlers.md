---
id: middlewares-handlers
title: Middlewares and Handlers / Core concepts
sb: sidebar/docs.html
---

# Middlewares and Handlers


Middlewares and handlers are essentials parts of Peak Applications stack. Their job is to simply receive a request, do stuff and call the next middleware or return a response whenever possible. They are also fully compatible with [PSR-7](https://www.php-fig.org/psr/psr-7/) and [PSR-15](https://www.php-fig.org/psr/psr-15/)


### Middleware

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
        // do something ...
        // and finish by calling the next middleware
        return $handler->handle($request);
        // or 
        return new Response('Hello mountains!');
    }
}
```


### Handlers

The main difference with handlers is that you must return a response. You cannot pass the request to the next middleware.

```php
use Psr\Http\Server\RequestHandlerInterface as Handler;
use Psr\Http\Message\ServerRequestInterface as Request;
use Psr\Http\Message\ResponseInterface as Response;

class HandlerA implements Handler
{
    /**
     * Handle the request and return a response.
     */
    public function handle(Request $request): Response
    {
        // ...
        return new Response('Hello mountains!');
    }
}  
```

### Closure middleware
You can also write middlewares and handlers with closure. Underneath, they are wrapped in `Peak\Http\Middleware\CallableMiddleware` which is *PSR-15* compliant. 

One of the downside of using closure is that they are less reusable outside your application. Also, it is generally not a good idea to use closure for complex middleware, but for small task and prototyping, they comes really handy.

```php
$myMiddleware = function ($request, $handler) {
    // ... do stuff
    return $handler->handle($request);
}

$myHandler = function ($request) {
    // ... do stuff
    return new Response('...');
}
```