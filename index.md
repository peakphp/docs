---
id: home
title: Home
layout: default
---

<h1 class="h1-logo text-center">{{ site.small_logo }} Peak <small><a href="{{ site.latest_release_url }}">v{{ site.latest_release }}</a></small></h1>

<h5 class="text-center">Fast, unopinionated and minimalist framework for PHP 7.</h5>
<div class="text-center">Build maintainable and powerful web applications and APIs!</div>
<br>

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

<br><br>
