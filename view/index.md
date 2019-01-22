# Peak/View

PSR-7 Response view template engine with macro and helpers

## Installation

```
$ composer require peak/view
```

### Create a Presentation



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

### Render a view

```php
$output = $view->render();
```

### Example of a view template file

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

script example:
```php
<div class="container">
    hello <?php echo $this->name; ?>
</div>
```


# Create a complex Presentation 
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
