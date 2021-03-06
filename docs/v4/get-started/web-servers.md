---
id: web-servers
title: Web servers
sb: sidebar/docs.html
---

## Web Servers

All HTTP requests must be forwarded to a entry point (usually ```index.php```).

### Apache configuration

Ensure your ```.htaccess``` and ```index.php``` files are in the same public-accessible directory. The .htaccess file should contain this code:

```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^ index.php [QSA,L]
```

Make sure to enable Apache’s mod_rewrite module and your virtual host is configured with the AllowOverride option so that the .htaccess rewrite rules can be used.


### Nginx configuration

This is an example Nginx virtual host configuration for the domain example.com. It listens for inbound HTTP connections on port 80. It assumes a PHP-FPM server is running on port 9000. You should update the server_name, error_log, access_log, and root directives with your own values. The root directive is the path to your application’s public document root directory; your Peak app’s index.php front-controller file should be in this directory.

```
server {
    listen 80;
    server_name example.com;
    index index.php;
    error_log /path/to/example.error.log;
    access_log /path/to/example.access.log;
    root /path/to/public;

    location / {
        try_files $uri /index.php$is_args$args;
    }

    location ~ \.php {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param SCRIPT_NAME $fastcgi_script_name;
        fastcgi_index index.php;
        fastcgi_pass 127.0.0.1:9000;
    }
}
```









 

    
    