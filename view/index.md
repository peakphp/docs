# Peak/View

Fast view template engine with macro and helpers.

## Installation

```
$ composer require peak/view
```

### Create a view

A view need 2 things:

 - Presentation
 - Data
 
```php

$presentation = new Presentation([
    '/layout1.php' => ['/view1.php']
]);
$data = [
    'title' => 'FooTheBar'
    'name' => 'JohnDoe'
];
$view = new View($data, $presentation);
```

### Example of view templates

layout example:
```php
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title><?php echo $this->title; ?></title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
<body>
    <main>
        <?php echo $this->layoutContent; ?>
    </main>
</body>
</html>
```

script example (represented by ```$this->layoutContent``` in your layout):
```php
<div class="container">
    hello <?php echo $this->name; ?>
</div>
```



### Render a view

```php
$output = $view->render();
```

### Add a macro
Macro are closure that will be bind to your view class instance. They have access to all class properties/methods so they must be used carefully. Macro are ideal for small task. 

```php
$view->addMacro('formatedName', function() {
    return ucfirst($this->name);
});
```

and in your template view:
```php
...
<h1>Hello <?php echo $this->formatedName(); ?></h1>
```

### Add an helper
An helper is a standalone object instance. You map a method from your helper to the view. In contrary of macro, helper do not have access to view properties/methods directly and tend to be more maintainable and secure than macro. Helper are ideal for advanced task and can benefit from dependencies injection.

Example of helper class
```php
class TextUtil
{
    public function escape($text)
    {
        return filter_var($text, FILTER_SANITIZE_SPECIAL_CHARS);
    }
}
```

Before you can use it, you'll need to set your view helpers:
```php
$view->setHelpers([
    'espace' => new TextUtil(),
]);
```

and finally, you'll be able to use your helper the same way you use macros
```php
...
<h1>Hello <?php echo $this->escape($this->name); ?></h1>
```


Tips: You can also add multiple methods from a single instance:
```php
$textUtil = new TextUtil();
$view->setHelpers([
    'espace' => $textUtil,
    'myMethod2' => $textUtil,
]);
```

### Create a complex Presentation 
```php
$presentation = new Presentation([
    '/layout1.php' => [
        '/view1.php',
        '/layout2,php' => [
            '/view2.php',
            '/view3.php',
        ]
    ]
]);
```
