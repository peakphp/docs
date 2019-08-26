---
id: routing-groups
title: Routing groups
sb: sidebar/docs.html
---

# Group

### Grouping routes

Repetitive routes prefixes can be reorganized into groups. Grouping routes will  generally improve performances of routing resolution. They also better encapsulate repetitive middleware(s) over certain routes group.

Example without group: 

```php
$app
    ->get('/users/action/edit', [new MiddlewareA(), new HandlerA()])
    ->get('/users/action/update', [new MiddlewareA(), new HandlerA()])
    ->get('/users/action/cancel', [new MiddlewareA(), new HandlerA()])
    ->get('/users//mypath1', new HandlerA())
    ->get('/users//mypath2', new HandlerA());

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
                    ->get('/cancel', new HandlerB());
            })
            ->get('/mypath1', new HandlerA())
            ->get('/mypath2', new HandlerA());
    });
```

As you can see, with group, we don't have to repeat ```MiddlewareA``` for ```/users/action/...``` routes.