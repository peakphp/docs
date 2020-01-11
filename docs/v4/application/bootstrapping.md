---
id: application-bootstrapping
title: Bootstrapping
sb: sidebar/docs.html
---

# Application Bootstrapping

Application bootstrapping consist into creating process(es) that implement ``Bootable`` interface. It can be done by using method ``bootstrap()``. 

```php
$app->boostrap([
    InitDatabase::class,
    StartSession::class,
]);
```

```php
// exemple of a bootable process
class InitDatabase implements Bootable
{
    public function boot()
    {
        // do something
    }
}
```
