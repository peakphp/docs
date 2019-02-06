---
title: Peak/Collection - Typed Structures
sb: sidebar/collection.html
---

{% include_relative _header.html %}

# Typed Structure

Typed structure is an easy to enforce types over an array or class while waiting for PHP 7.4 typed properties.

Here an example:

```php
use Peak\Collection\Structure\AbstractStructure;

class User extends AbstractStructure
{
    public function getStructure(): array
    {
        return [
            'id' => $this->integer(),
            'email' => $this->string(),
            'description' => $this->string()->null(),
            'banned' => $this->boolean()->default(true),
            'createdAt' => $this->object(\DateTime::class),
            'misc' => $this->any(),
        ];
    }
}

$user = new User([
    'email' => 'foobar@gmail.com',
    'description' => null,
    'banned' => false,
    'createdAt' => new \DateTime(),
]);

// or
$user = new User();
$user->email = 'foobar@gmail.com';
$user->descrition = '...';
```

All unspecified properties at the object creation and defined in your structure will be set to null or their default, if specified.

#### List of types

 - ``integer()``
 - ``float()``
 - ``boolean()``
 - ``string()``
 - ``array()``
 - ``object([ string $className = 'object' ])``
 - ``resource()``
 - ``null()``
 - ``any()``
 
Call typing can be done with ``object($className)``.

Use ``any()`` only when your not sure of the data you will store on a property.

You can combine multiple types to a single property by chaining them:

```php
'bar' => $this->string()->array()->null()
````
 
#### Setting or updating a property

Structure implements __get() and __set(). Setting an unknown property or the wrong type to a defined property will throw an ``Exception``. You may use ``isset()`` to check for a property existence.

```php
$user->id = 134; // ok
$user->id = 'test'; // will throw an exception
````

#### Immutable structure
Use class ``AbstractImmutableStructure`` to create a read-only structure. You will be able to set properties only on object creation. Trying to modify a property after the object creation will throw an ``Exception``.


#### Looping
Structure implement ``IteratorAggregate``.

```php
foreach ($user as $prop => $val) {
    // ..
}
```

#### Other method

```
$userArray = $user-toArray();
```