---
id: installation
title: Installation
sb: sidebar/docs.html
---

## Installation

##### System Requirements

 - PHP 7.2 or newer
 - a web server with URL rewriting like Apache or NGINX


##### Application Requirements

 - a PSR-7 compatible library
 

### Step 1: Install Composer

Don’t have Composer? It’s easy to install by following the instructions on their [download](https://getcomposer.org/download/) page.

### Step 2: Install Peak

We recommend to install Peak via Composer:

```
$ composer require peak/framework
```

### Step 3: Install a PSR-7 compatible library

In order to process an HTTP Request with a Peak Application, you will need to install a compatible with PSR-7. Here's a quick list:

 - [guzzlehttp/psr7](https://packagist.org/packages/guzzlehttp/psr7)
 - [zendframework/zend-diactoros](https://packagist.org/packages/zendframework/zend-diactoros)
 - [nyholm/psr7](https://packagist.org/packages/nyholm/psr7)
 - etc... (check [here](https://packagist.org/providers/psr/http-message-implementation) for all PSR-7 library implementation)
 
For the purpose of this documentation, we will use zend-diactoros:

```
$ composer require zendframework/zend-diactoros
```

### Step 4: create your application entry point

```php
<?php

use Peak\Backpack\Bedrock\HttpAppBuilder;
use Peak\Http\Response\Emitter;
use Psr\Http\Message\ServerRequestInterface as Request;
use Zend\Diactoros\Response\HtmlResponse;
use Zend\Diactoros\ServerRequestFactory;

require __DIR__ . '/../vendor/autoload.php';

$app = (new HttpAppBuilder())->build();

$app
    ->get('/hello/{name}', function (Request $request) {
        return new HtmlResponse('Hello ' . $request->args->name . '!', 200);
    })
    ->run(ServerRequestFactory::fromGlobals(), new Emitter());
```



 

    
    