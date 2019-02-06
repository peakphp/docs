---
id: peak-app-project
title: Peak Application Project
---

{% include breadcrumb2.html base_url=site.url url_1="" text_1="Home" text_2="Peak Application Project" %}

# Peak Application Project

<i class="fab fa-github"></i> [https://github.com/peakphp/peak](https://github.com/peakphp/peak)

This is the default opinionated implementation of the peak framework that comes with session, configuration , routing, debugbar, phpunit, folder structure and use [zendframework/zend-diactoros](https://packagist.org/packages/zendframework/zend-diactoros) as PSR-7 library.

### Docker support

This project also comes with a Docker configurations to help you start fast. What's included :

- PHP 7.3
- Nginx latest
- MYSQL 8
- Redis latest
- And a bunch of tools like composer and database migration

### Install via composer

```
$ composer create-project peak/peak -s dev
```
{% capture alert_content %}
Peak in steal in beta, so you need to specify ``-s dev``
{% endcapture %}
{% include alert.html type="warning" content=alert_content %}

##### How to use it ?

- Copy ``.env.dist`` to ``.env``
- Install vendor with ``$ docker-compose run composer-install``
- Start the web server with ``$ docker-compose up -d web``
- Visit ``http://localhost:8080``

Don't want to use docker? Just remove:
 - `Dockerfile`
 - `DockerfileShell`
 - `docker-compose.yml`
 - `/docker` folder
 