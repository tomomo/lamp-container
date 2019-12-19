# LAMP-Container
Generate a Docker image for building a LAMP environment.

## Requirement
- docker
- docker-compose

## Usage
1. Generate an image.
    ```bash
    # build image.
    docker-compose build

    # confirm images.
    docker images | grep php-custom-docker

    lamp-container_mysql-55   latest  59455c5fe314  13 seconds ago  205MB
    lamp-container_apache-24  latest  12f6655cf636  16 seconds ago  130MB
    lamp-container_fpm-72     latest  7804cb426f1a  20 seconds ago  338MB
      :
    ```
1. Specify the docker image in `docker-compose.yml` of the project you are developing. See `project_root`.
    ```yml
    # docker-compose.yml setting example
    version: '3.7'
    
    services:
      web:
        image: lamp-container_apache-24
        restart: always
        depends_on:
          - php-fpm
          - db
        ports:
          - "${HTTP_PORT}:80"
          - "${HTTPS_PORT}:443"
        volumes:
          - .:/var/www/html:Z
          # - ./certificate:/usr/local/apache2/certificate:ro
      php-fpm:
        image: lamp-container_php-fpm-74
        restart: always
        volumes:
          - .:/var/www/html:Z
      db:
        image: lamp-container_mysql-55
        restart: always
        volumes:
          - data:/var/lib/mysql
        environment:
          - MYSQL_ROOT_PASSWORD=root
          - MYSQL_DATABASE=${DB_DATABASE}
          - MYSQL_USER=${DB_USERNAME}
          - MYSQL_PASSWORD=${DB_PASSWORD}
      phpmyadmin:
        image: phpmyadmin/phpmyadmin:latest
        restart: always
        depends_on:
          - db
        ports:
          - "${PHPMYADMIN_PORT}:80"
        links:
          - db
        environment:
          - PMA_ARBITRARY=1
          - PMA_HOST=db
          - PMA_USER=root
          - PMA_PASSWORD=root
    volumes:
      data:
        external:
          name: ${DB_VOLUME}
    ```

## References
### PHP support schedule.
See the link for more details.
https://www.php.net/supported-versions.php
