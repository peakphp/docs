---
title: Peak/Di - Dependency Injection Container
sb: sidebar/di.html
---

{% include breadcrumb3.html base_url=site.url url_1="" text_1="Home" url_2="packages" text_2="Packages" text_3="Dependency Injection" %}

## Peak\Di
#### PSR-11 Dependency Injection Container

This component allows you to standardize and centralize the way objects are constructed in your application.

## Installation outside framework

```
$ composer require peak/di
```

Basic usage:

- [Using Autowiring](basic-usage#using-autowiring)
- [Reuse a class instance](basic-usage#reuse-a-class-instance)
- [Get a stored class instance](basic-usage#get-a-stored-class-instance)
- [Verify a stored class instance](basic-usage#verify-a-stored-class-instance)
- [Use alias for stored class instance](basic-usage#use-alias-for-stored-class-instance)
- [Call an class instance method](basic-usage#call-an-class-instance-method)

Create objects:

- [How method create() work](create/#how-method-create-work)
- [create() parameters](create/#parameters)

Advanced usage:

- [Definitions (Autowiring disabled)](definitions/#definitions-autowiring-disabled)
- [Definitions type](definitions/#definitions-type)
- [Singleton bind](definitions/#singleton-bind)
- [Factory bind](definitions/#factory-bind)
- [Prototype bind](definitions/#prototype-bind)
- [Array of definition](definitions/#array-of-definition)
