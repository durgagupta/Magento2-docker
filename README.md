# Magento2-docker

Getting started with docker-compose is a few steps process:

In this project, we are using:

> Operating system: Ubuntu 16.04

> Web Server: Apache2

> Database Server: Mysql-server-5.7

> PHP version: PHP-7.1


To begin with, please install docker and docker-compose on your system. 

Then follow the following steps:

1). Clone or download this repository as 
2) Set mysql root credentials and name of the database to be created . Go to ~/magento2-docker-compose/docker-compose.yml and change mysql root password in database_server in:

> mysql_password=

> mysql_database=

3). Download Magento 2 version you wish to dockerize and upload it in directory magento2 in parallel docker-compose.yml.

> Go to https://magento.com/tech-resources/download? .

4). Build the docker image.

> docker-compose build

6). Check the built image as:

> docker images

7). Run the containers from built image as:

> docker-compose up -d

8). Check the running docker containers by command:

> docker-compose ps

> docker ps

Now, your server setup is all ready, now hit your domain name or IP to install Magento 2.

# Few common Docker command



