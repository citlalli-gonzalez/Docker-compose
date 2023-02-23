




version: "3"
networks:
    myproyecto-wordpress-6.1.1-net:
        driver: bridge

services:
    mysql:
        image: mysql:5.7
        container_name: myproyecto-wordpress-6.1.1-mysql
        tty: true
        ports:
            - "4208:3306"
        volumes:
            - "./var/libclea/mysql/:/var/lib/mysql"
        environment:
            MYSQL_ROOT_PASSWORD: 1234
            MYSQL_DATABASE: wordpress
            MYSQL_USER: citlalli
            MYSQL_PASSWORD: 1234
        networks:
            - myproyecto-wordpress-6.1.1-net

    server:
        image: wordpress:latest
        container_name: myproyecto-wordpress-6.1.1
        ports:
            - "4282:80"
        volumes:
            - "./var/www/html/:/var/www/html"
        environment:
            WORDPRESS_DB_USER: citlalli
            WORDPRESS_DB_PASSWORD: 1234
            WORDPRESS_DB_NAME: wordpress
            WORDPRESS_DB_HOST: myproyecto-wordpress-6.1.1-mysql
        depends_on:
            - mysql
        networks:
            - myproyecto-wordpress-6.1.1-net

    phpmyadmin:
        image: phpmyadmin/phpmyadmin
        container_name: myproyecto-phpmyadmin
        ports:
            - "4283:80"
        environment:
            PMA_HOST: myproyecto-wordpress-6.1.1-mysql
            MYSQL_ROOT_PASSWORD: 1234
        depends_on:
            - mysql
        networks:
            - myproyecto-wordpress-6.1.1-net

