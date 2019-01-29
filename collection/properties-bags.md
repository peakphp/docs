# Properties Bags

A bag of properties is a simple collection structure with basic tooling for array. It comes in 2 versions: 
- ``PropertiesBag`` 
- ``ImmutablePropertiesBag``

As the name speak for itself, ``ImmutablePropertiesBag`` properties cannot be altered after instance creation and will throw an exception if you try to.

A properties bag implement (Peak\Blueprint\Collection\Dictionary)[https://github.com/peakphp/framework/blob/master/src/Blueprint/Collection/Dictionary.php] and (JsonSerializable)https://php.net/manual/en/class.jsonserializable.php]

```php
$bag = new PropertiesBag([
    'foo' => 'bar',
    'name' => 'jane'
]);
```

#### Set and Get
```php
// set
$bag->set('name', 'j.');
$bar->name = 'j.';
$bag['name'];

// get
$bag->get('name');
$bag->name;
$bag['name'];
```

#### Isset and Unset
```php
// isset
isset($bag->name);
isset($bag['name']);
$bag->has('name');

// unset
unset($bag->name);
unset($bag['name']);
```

#### Loop
```php
foreach ($bag as $property => $value) {
    // ...
}
```

#### Other methods

Bag also support:
 - ``count()``, 
 - ``json_encode()``, 
 - ``serialize()`` and ``unserialize()``, 
 - ``toArray()``, 

