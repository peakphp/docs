# Middlewares and Handlers

Middlewares and handlers are essentials parts of your Applications stack. They job is to simply received a request, do stuff and in case of middlewares, call the next middlewares and handlers return a response whenever possible. They are also fully compatible on [PSR-15: HTTP Server Request Handlers](https://www.php-fig.org/psr/psr-15/).

### Here a middleware example: 

```php
use Psr\Http\Server\MiddlewareInterface;
use Psr\Http\Server\RequestHandlerInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Message\ResponseInterface;

class middlewareA implements MiddlewareInterface
{
    /**
     * Process an incoming server request and return a response, optionally delegating
     * response creation to a handler.
     */
    public function process(ServerRequestInterface $request, RequestHandlerInterface $handler): ResponseInterface 
    {
        // ...
        
        return $handler->handle($request);
    }
}
```

### Here an handler example: 

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
### Using closure
You can also write middlewares and handlers with closure. Underneath, they will be wrapped on `CallableMiddleware` which is *PSR-15* compliant. The only downside is that closure are less reusable outside your application. Also, it is generally not a good idea to use closure for complex middleware, but for small task and prototyping, they comes really handy.

```php
$myMiddleware = function ($request, $handler) {
    return $handler->handle($request);
}

$myHandler = function ($request) {
     return return new Response('...');
 }
```