---
title: Peak/Di - Dependency Injection Container
sb: sidebar/di.html
---

{% include breadcrumb3.html base_url=site.url url_1="" text_1="Home" url_2="packages" text_2="Packages" text_3="Dependency Injection" %}

## Peak/Di
#### PSR-11 Dependency Injection Container

This component allows you to standardize and centralize the way objects are constructed in your application.

## Installation

```
$ composer require peak/di
```

## Getting started
```php
$foo = $container->get(Foo::class);
```