---
id: routing-groups
title: Routing groups
sb: sidebar/docs.html
---

# Grouping routes

Repetitive routes prefixes can be reorganized into groups. Grouping routes will  generally improve performances of routing resolution. They also better encapsulate repetitive middleware(s) over certain routes group.

Example without group: 

```php
$app
    ->get('/users/action/edit', [new MiddlewareA(), new HandlerA()])
    ->get('/users/action/update', [new MiddlewareA(), new HandlerA()])
    ->get('/users/action/upgrade', [new MiddlewareA(), new HandlerA()])
    ->get('/users/profile', new HandlerA())
    ->get('/users/settings', new HandlerA());

```

Now with groups, we can express the same stack above more efficiently:

```php
$app
    ->group('/users', function() use ($app) {
        $app
            ->group('/action', function() use ($app) {
                $app
                    ->stack(new MiddlewareA())
                    ->get('/edit', new HandlerB())
                    ->get('/update', new HandlerB())
                    ->get('/upgrade', new HandlerB());
            })
            ->get('/profile', new HandlerA())
            ->get('/settings', new HandlerA());
    });
```

As you can see, with group, we don't have to repeat ```MiddlewareA``` for ```/users/action/...``` routes.