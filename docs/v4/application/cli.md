---
id: application-cli
title: Cli Application
sb: sidebar/docs.html
---

# Cli Application

You can create CLI application with ``Peak\Bedrock\Cli\Application``. 

Internally, it use [``symfony/console``](https://symfony.com/doc/current/components/console.html) and ``symfony/process``. 

The advantages of this wrapper over direct implementation with ``symfony/console`` is that it give you PSR-11 support, bootstrapping and configuration.

``cli.php`` example: 
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

Run commands with:
```bash
$ php cli.php [command]
```

You can access to symfony console instance with method ``console()``

```php
$app->console()->setDefaultCommand('...');
```

Visit the official [Symfony Console Documentation](https://symfony.com/doc/current/components/console.html) for more infos on commands and others helpers.