##### < [packages](/index#packages)
### Peak/Http


Http stack represent a pipeline of middlewares designed to handle an http request and/or return a reponse. 
To work properly, a stack will need a HandlerResolver with a container which may have PSR-11 container to resolve middlewares.

```php
use Peak\Di\Container;
use Peak\Http\Request\HandlerResolver;
use Peak\Http\Stack;

$container = new Container();
$handlerResolver = new HandlerResolver($container);
$stack = new Stack([
    MiddlewareA::class,
], $handlerResolver);

```

Any PSR-15 middleware can be added to a stack. 
A stack itself can be added to a parent stack.

```php
$stack = new Stack([
    MiddlewareA::class,
    new Stack([
        MiddlewareA1::class,
        MiddlewareA2::class,
        new Stack([
            MiddlewareAA1::class,
            MiddlewareAA2::class,
        ]),
    ]),
    MiddlewareB::class,
], $handlerResolver);

```

It also support simple closure but behind the scene, closure are bridged to a valid Psr\Http\Server\MiddlewareInterface

```php
$stack = new Stack([
    MiddlewareA::class,
    function(ServerRequestInterface $req, RequestHandlerInterface $next) {
        // do something with the request and call next
        return $next->handle($req);
    },
    function() {
        return new TextResponse('Ok', 200);
    },
], $handlerResolver);

```

# Resolve stack with a request

Resolve a http request compatible PSR-7 ``ServerRequestInterface`` with the method ``handle()``

```php
$response = $stack->handle($request);
```