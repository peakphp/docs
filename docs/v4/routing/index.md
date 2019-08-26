---
id: routing-overview
title: Routing
sb: sidebar/docs.html
---

# Routing

### Adding routes to your application

Creating HTTP Routes is very easy. and can be done with methods:

 - ```get()```
 - ```post()```
 - ```put()```
 - ```patch()```
 - ```delete()```
 - ```all()```
 
Using one of the methods above will automatically add them to your application stack
 
```php
$app->get(string $path, mixed $stack);
```

