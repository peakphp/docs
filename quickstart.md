---
sb: sidebar/quickstart.html
id: quickstart
title: Quick Start
---

# Start an application from scratch
This guide assume that your have composer installed and an environment which can run php through a web browser.

### Install via composer

```
$ composer require peak/framework
```

### Create an application

To create an new application instance, you need at least 3 things:

 - a **Container** PSR-11 (*Psr\Container\ContainerInterface*)
 - a **Kernel** (*Peak\Blueprint\Bedrock\Kernel*) 
 - a **Resource resolvers** (*Peak\Blueprint\Common\ResourceResolver*)
 - (optional) A **Dictionary** for properties (*Peak\Blueprint\Collection\Dictionary*)

```php
use Peak\Bedrock\Application\Application;
use Peak\Bedrock\Kernel;
use Peak\Collection\PropertiesBag;
use Peak\Di\Container;
use Peak\Http\Request\HandlerResolver;

$container = new Container();
$app = new Application(
    new Kernel('prod', $container),
    new HandlerResolver($container),
    new PropertiesBag([ // optional
        'version' => '1.0',
        'name' => 'app'
    ])
);
```

### Add route

```php
$app
    ->get('/', function() {
        return new TextResponse('Climb!');
    })
    ->stack(function() {
        return new TextResponse('Not Found', 404);
    });
```

### Process a server request and output a response

To handle an `Application` server request, you need a compatible *PSR-7* library. 

Popular ones:
 - [guzzlehttp/psr7](https://packagist.org/packages/guzzlehttp/psr7)
 - [zendframework/zend-diactoros](https://packagist.org/packages/zendframework/zend-diactoros)

Example with Zend/Diactoros:
```php
    try {
        $request = ServerRequestFactory::fromGlobals();
        $app->run($request, new Emitter());
    } catch(Exception $e) {
        // do something
    }
```

### Handle the response

You may want to do additionnal processing over the response before sending out to the client. It possible to do so by using ``handler()`` method instead of ``run()``

```php
$response = $app->handle($request);

// ... do something with the response

$emitter = new Emitter();
$emitter->emit($response);
```

### How it works ?

The application is essentially a stack of resources acting as request middlewares and handlers. 
When a *PSR-7* request is send to the `Application`, the main stack is resolved and executed. 

There is two outcomes possible when the main stack is resolved:
1. It stop when a *PSR-7* response is returned.
2. It throw an exception because no response have been returned.

<img src="https://raw.githubusercontent.com/peakphp/docs/master/_pencils/request_response_flow.png" alt="Peak">

### Quick start Summary

```php
<?php
    require __DIR__ . '/vendor/autoload.php';
    
    use Peak\Bedrock\Application\Application;
    use Peak\Bedrock\Kernel;
    use Peak\Collection\PropertiesBag;
    use Peak\Di\Container;
    use Peak\Http\Request\HandlerResolver;
    use Peak\Http\Response\Emitter;
    use Zend\Diactoros\Response\TextResponse;
    use Zend\Diactoros\ServerRequestFactory;

    $container = new Container();
    $app = new Application(
        new Kernel('prod', $container),
        new HandlerResolver($container),
        new PropertiesBag([ // optional
            'version' => '1.0',
            'name' => 'app'
        ])
    );
    $app
        ->get('/', function() {
            return new TextResponse('Climb!');
        })
        ->stack(function() {
            return new TextResponse('Not Found', 404);
        });

    try {
        $request = ServerRequestFactory::fromGlobals();
        $app->run($request, new Emitter());
    } catch(Exception $e) {
        echo $e->getMessage();
    }
```
    




