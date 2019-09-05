---
id: application-overview
title: Application overview
sb: sidebar/docs.html
---

# Application

### Manually create HTTP application 

To create a new HTTP Application instance, you need at least 3 things:

 - a **Container** compatible [PSR-11](https://www.php-fig.org/psr/psr-11/)
 - a **[Kernel](https://github.com/peakphp/framework/blob/master/src/Blueprint/Bedrock/Kernel.php)** 
 - a **[Resource resolvers](https://github.com/peakphp/framework/blob/master/src/Blueprint/Common/ResourceResolver.php)** for your stack middleware(s) and handler(s)

Also, you can optionally set a **[Dictionary](https://github.com/peakphp/framework/blob/master/src/Blueprint/Collection/Dictionary.php)** for your application properties

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

### Creating an application using HttpAppBuilder()

This is the easiest and fastest way to create an application. 

Be advise though by default, ``HttpAppBuilder`` will assume dependencies for you if you don't specify anything. 

```php
use Peak\Backpack\Bedrock\HttpAppBuilder;

$httpAppBuilder = new HttpAppBuilder();
$app = $httpAppBuilder
    ->setProps([
        'version' => '1.0',
        'name' => 'app'
    ])
    ->build();
```



##### Configure environment with ``setEnv()``

By default, environment is set to ``prod``. To change that: 

```php
$httpAppBuilder->setEnv('dev');
```

##### Change the Kernel Class ``setKernelClass()``

By default, class ``Peak\\Bedrock\\Kernel`` is used. To change that: 

```php
$httpAppBuilder->setKernelClass(MyAppKernel::class);
```

##### Change the container

By default, class ``Peak\Di\Container`` is used. To change that

```php
$httpappBuilder->setKernelClass(MyAppKernel::class);
```