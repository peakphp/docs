---
id: home
title: Home
layout: default
---

<h1 class="h1-logo text-center">{{ site.small_logo }} Peak 
<small><a href="{{ site.latest_release_url }}">v{{ site.latest_release }}</a></small>
</h1>

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

<div class="text-center">

<h4>Community</h4>

<h5>Slack</h5>
Join us there at <a href="https://peakframework.slack.com">peakframework.slack.com</a><br>
<a class="pull-right btn btn-sm btn-warning" href="https://join.slack.com/t/peakframework/shared_invite/enQtNzQ3NDQyNTg2Mjk0LTJhMjg1YmFhMjgyYTczYWRlMGYxZDkxNGVjNTcxYTlhYTdjZTUzMzRmYTY5NTIzYzgzZWVlYjQyMzBiMmE1OWY">Join Slack Channel</a>

</div>
        
