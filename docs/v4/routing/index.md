---
id: routing-overview
title: Routing
sb: sidebar/docs.html
---

# Routing

### Adding routes to your application

Creating HTTP Routes is very easy and can be done with preset route methods. Using one of the methods below will automatically add the route to your application stack:

 - ```get()```
 - ```post()```
 - ```put()```
 - ```patch()```
 - ```delete()```
 - ```all()```
 
Usage:

``$app->[method](string $path, mixed $stack);``

```php
$app->get('/hello', function() {
    return new TextResponse('Hello!');
});
```

Use ``all()`` if you want to respond to a route path while ignoring the request method.

```php
$app->all('/hello', function() {
    return new TextResponse('Hello!');
});
```

### Using stackRoute()

Alternatively, you can create and stack new routes with ``stackRoute()``. You have to pass the request method as argument along with the path and the stack.

Usage:

``$app->stackRoute(?string $method, string $path, mixed $stack);``

```php
$app->stackRoute('GET', '/hello', function() {
    return new TextResponse('Hello!');
});
```

Note: Passing ``null`` as ``$method`` argument will simply create a route that ignore the request method.

