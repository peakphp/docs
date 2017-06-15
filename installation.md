# Framework requirements

Peak 2.x requires:

- PHP 5.6.x at least OR PHP 7.x and +
- PDO PHP Extension (http://php.net/manual/en/pdo.installation.php)
- Mbstring PHP Extension (http://php.net/manual/en/mbstring.installation.php)
- IntlChar (http://php.net/manual/en/intl.installation.php)

# Installation

##### If it's your first time, you may want to check this guide here.

> Peak Framework use [Composer](https://getcomposer.org/), a dependency manager for PHP. 

Install only the framework:
```
$ composer require peakphp/framework
```

Install the default application :
```
$ composer create-project peakphp/peak --prefer-dist
```