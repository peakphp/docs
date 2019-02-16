---
title: Peak/Bedrock - Cli Application
sb: sidebar/bedrock.html
---

{% include_relative _header.html %}

```php
namespace {

    use Peak\Bedrock\Cli\Application;
    use Peak\Bedrock\Kernel;
    use Peak\Collection\PropertiesBag;
    use Peak\Di\Container;

    require '../vendor/autoload.php';

    $kernel = new Kernel('dev', new Container());
    $app = new Application($kernel, new PropertiesBag([
        'name' => 'My Cli Application',
        'version' => '1.0',
    ]));

    try {
        $app->add([
            MyCommand::class
        ])
        $app->run();
    } catch (\Exception $e) {
        echo $e->getMessage();
    }
}
```