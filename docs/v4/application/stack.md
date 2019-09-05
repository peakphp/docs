---
id: application-stack
title: Application Stack
sb: sidebar/docs.html
---

# Application Stack

Because of the nature of middlewares, the process to resolve them is very linear, like a pipeline, where at the end you receive a response. 

A Peak application is composed of a ``Stack`` object containing your middleware(s) and/or handler(s). By doing so, you can create a structure of nested middlewares and handlers instead of a linear chain of middlewares.

In other words, a ``Stack`` is a group of middlewares which can hold a parent ``Stack`` to call if the request have passed through all the middlewares and no response have been returned.

```php
$app->stack([
    new MiddlewareA(),
    new MiddlewareB(),
    new MiddlewareC(),
    $app->createStack([
        new MiddlewareD(),
        new MiddlewareE(),
        new MiddlewareF(),
    ])
    new HandlerA(),
]);
```

If we pass a request through the example above, the ``MiddlewareF`` is executed than, the child stack will call the parent stack to resume the process by calling ``HandlerA``, 


### A Route is also a Stack

```php
$app->get('/hello', [
    new MiddlewareA(),
    new MiddlewareB(),
    $app->createStack([
        new MiddlewareD(),
        new MiddlewareE(),
    ])
    new HandlerA(),
]);
```

### Differences between ``stack()`` and ``createStack()``

Internally, when you call method ``stack()``, a new ``Stack`` object is created with yours middlewares/handlers and it is finally stored inside the main internal application ``Stack``. 

You **cannot** nest method ``stack()`` inside another ``stack()`` or ``stackRoute()``, because the child stack will be store before is parent stack.

The same differences between ``stack()`` and ``createStack()`` applies to ``stackRoute()`` and ``createRoute()``.

You **cannot** nest method ``stackRoute()`` inside a ``stack()`` and ``stackRoute``. The same apply to ``get()``, ``post()``, ``put()``, ``patch()``, ``delete()``, ``all()``

```php
// this won't work
$app->stack( [
    new MiddlewareA(),
    new MiddlewareB(),
    $app->stack([
        new MiddlewareD(),
        new MiddlewareE(),
    ])
    $app->get('/hello', new HandlerA())
    new 404Handler(),
]);
```

The proper way to nested stacks is to use ``createStack()`` and ``createRoute()``:

```php
$app->stack([
    new MiddlewareA(),
    new MiddlewareB(),
    $app->createStack([
        new MiddlewareD(),
        new MiddlewareE(),
    ])
    $app->createRoute('GET', '/hello', new HandlerA())
    new 404Handler(),
]);
```


### Conditional Stack

```php
$app->stackIfTrue(<condition>, [
    new MiddlewareA(),
    new MiddlewareB(),
    new MiddlewareC(),
])
```