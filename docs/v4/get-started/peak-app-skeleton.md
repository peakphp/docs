---
id: peak-app-skeleton
title: Peak Application Skeleton project
sb: sidebar/docs.html
---

# Peak Application Skeleton

In a hurry? Want to start writing & running your app without the hassle of preparing your project structure? This application skeleton project can help you to start even faster.

### What is it?
This application skeleton is the default "opinionated" implementation of the peak framework that comes with session, configuration, database & migration, phpunit, folder structure and use [zendframework/zend-diactoros](https://packagist.org/packages/zendframework/zend-diactoros) as PSR-7 library.

<i class="fab fa-github"></i> [https://github.com/peakphp/peak](https://github.com/peakphp/peak)

### Docker support

This project comes with a Docker configurations to help you start fast. What's included :

- PHP 7.3
- Nginx latest
- MYSQL 8
- Redis latest
- Composer install & update
- Database migration
- Shell access

### Install via composer

```
$ composer create-project peak/peak [you-project-name]
```

### How to use it ?

- Copy ``.env.dist`` to ``.env``
- Install vendor with ``$ docker-compose run composer-install``
- Start the web server with ``$ docker-compose up app``
- Visit ``http://localhost:8080`` and voilà!

Others docker commands:

- ```$ docker-compose run migration``` for database migration
- ``$ docker-compose run shell`` for entering in the container with shell bash
- ``$ docker-compose run climber`` for using the CLI console application
- ``$ docker-compose up adminer`` for running database web manager Adminer

Don't want to use docker? Just remove:
 - `Dockerfile`
 - `DockerfileShell`
 - `docker-compose.yml`
 - `/docker` folder
 
 