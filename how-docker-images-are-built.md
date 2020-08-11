# How docker images are built
You can see that for the services `web` and `app` they have a `build` attribute.

This shows `docker-compose` that if an image does not exist already, how to build one.

```
  app:
    build:
      context: ./docker
      dockerfile: app.docker
``` 

The app service is build from the `app.docker` file, within the `docker/` directory.

## Dockerfiles

Below is `docker/app.docker`. It describes how to build the image for the `app` service. 
```
FROM php:7.4-fpm-alpine

RUN apk update && apk add mysql-client \
        freetype \
        libpng \
        libjpeg-turbo \
        freetype-dev \
        libpng-dev \
        libjpeg-turbo-dev \
        $PHPIZE_DEPS \
     && docker-php-ext-configure gd \
        --with-freetype \
        --with-jpeg \
    && docker-php-ext-install pdo_mysql mysqli gd exif

ARG WITH_XDEBUG=false
RUN if [ $WITH_XDEBUG = "true" ] ; then \
        pecl install xdebug; \
        docker-php-ext-enable xdebug; \
        echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
    fi;

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /var/www
```

It takes an existing image, `php:7.4-fpm-alpine` (you can find more on https://hub.docker.com/), and runs some commands. 

This particular example uses Alpine Linux (which is a very lightweight linux distribution), and installs some dependencies
for PHP, such as pdo_mysql, php-mysqli, php-gd and php-exif. 

It also installs some packages from a variable, `$PHPIZE_DEPS`. These are all of the packages required to run the `phpize`
command required to install certain PHP extensions such as XDebug.

There is also conditional section towards the bottom of the file. If the `WITH_XDEBUG` argument is defined as true, 
install, configure and enable xdebug package.

Finally, it defines the workdir as `/var/www`  