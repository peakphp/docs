---
title: Peak/Collection - Dot Notation Collection
sb: sidebar/collection.html
---

{% include_relative _header.html %}

# DotNotation Collection

A DotNotation Collection extends [Collection]({{ site.url }}collection/collection) and implements [Peak\Blueprint\Collection\Dictionary](https://github.com/peakphp/framework/blob/master/src/Blueprint/Collection/Dictionary.php) and give you 4 new methods to manage your collection easily:
 - ``get()``
 - ``set()``
 - ``add()``
 - ``has()``
 
 The Dot ``.`` can be used to access elements in your collection. Imagine you have a collection like this:
 
 ```php
 $coll = new DotNotationCollection([
    'customer' => [
        'name' => 'Jane'
        'age'  => 26,
        'address' => [
            'street' => 'FooBar Boulevard'
            'city' => 'New Bar City',
            'state' => 'ZZ',
            'zip' => '1ab2c3',
            'country' => 'Republica of Banana',
        ]
        'hobbies' => [
            'kayak', 'skiing', 'surfing'
        ]
    ]
 ]);
 ```
 
To access to the customer street with a regular ``Collection``, you have to do something like this:

```php
$street = $coll['customer']['address']['street'];
// or
$street = $coll->customer['address']['street']; 
```
 
With ``DotNotationCollection``, you can simply access to street with ``get()`` with dots instead of array brackets. By default, if a property doesn't exists, ``null`` is returned. You can change the default by passing a second argument to ``get()`` with the default value you want 
```php
$street = $coll->get('customer.address.street');
// with a default value
$street = $coll->get('customer.address.street', 'unknown');
```

You can check if a property exists with ``has()``:
```php
if ($coll->has('customer.address.street')) {
    // ...
}
```

You can change a property value(s) with ``set()``:
```php
$coll->set('customer.address.street', '123 Abc Blvd');
```

#### Dealing with numeric index

Accessing to a element of array is straightforward:
```php
$hobby = $coll->get('customer.hobbies.1'); // skiing
```
