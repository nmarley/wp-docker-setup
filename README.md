# Dockerized WordPress Setup

Tried to build this the proper way, instead of sticking everything into one
container.

### Steps to Install/Run:

1. Extract recent version of WordPress to shared/code:

    ```
    curl -s https://wordpress.org/latest.tar.gz | gzip -d | pax -r -s :^wordpress:shared/code:
    chmod 0777 shared/code
    ```

2. Build php-fpm image w/mysqli:

    ```
    (cd php && docker build -t php:7-fpm-alpine-mysqli .)
    ```

3. Place necessary configuration files in `nginx/`, `mysql/`, `php/`.

  * SSL files go in `nginx/ssl`
  * set proper defaults for `nginx/vars.env` and `mysql/vars.env`
  * any other configuration changes you desire

4. Start all services with docker-compose\*:

    ```
    docker-compose up -d
    ```

* Note: Ubuntu is notoriously slow about updating packages. As of 2017-03-20, the version of docker-compose that ships with Ubuntu 16.10 is really old and won't work with this docker-compose file. You will need to install the updated binary yourself.

### To stop all services:

    docker-compose down

### Post-Install

After installing WordPress via the web interface, you might want to lock down the code directory permissions:

    chmod 0755 shared/code
