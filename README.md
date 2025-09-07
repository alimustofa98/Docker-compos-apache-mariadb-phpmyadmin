version: '3.9'

services:
  web:
    image: php:8.2-apache
    container_name: apache-php
    volumes:
      - ./app:/var/www/html
    ports:
      - "8080:80"
    depends_on:
      - db
    build:
      context: .
      dockerfile: Dockerfile

  db:
    image: mariadb:10.11
    container_name: mariadb
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: mydb
      MYSQL_USER: user
      MYSQL_PASSWORD: userpass
    volumes:
      - db_data:/var/lib/mysql
    ports:
      - "3306:3306"

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: phpmyadmin
    restart: always
    depends_on:
      - db
    environment:
      PMA_HOST: db
      PMA_USER: root
      PMA_PASSWORD: root
    ports:
      - "8081:80"

volumes:
  db_data:
