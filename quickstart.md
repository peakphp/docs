---
sb: sidebar/quickstart.html
id: quickstart
title: Quick Start
---

{% include breadcrumb2.html base_url=site.url url_1="" text_1="Home" text_2="Quick Start" %}

# Start an application from scratch
This guide assume that your have composer installed and an environment which can run php through a web browser.

{% capture alert_content %}
Before you go on, you might want try <a href="{{ site.url }}peak-app-project">peak project</a> to even start faster.
{% endcapture %}
{% include alert.html content=alert_content %}

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
use Peak\Bedrock\Http\Application;
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

We will use [Zend Diactoros](https://packagist.org/packages/zendframework/zend-diactoros) for the example:

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

### Quick start Summary

```php
<?php
    require __DIR__ . '/vendor/autoload.php';
    
    use Peak\Bedrock\Http\Application;
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
    




