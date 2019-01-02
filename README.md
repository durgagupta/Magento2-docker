# Magento2-docker

Getting started with docker-compose is a few steps process:

In this project, we are using:

> Operating system: Ubuntu 16.04

> Web Server: Apache2

> Database Server: Mysql-server-5.7

> PHP version: PHP-7.1


To begin with, please install docker and docker-compose on your system. 

Flow following referance to install Docker
> Mac: https://docs.docker.com/docker-for-mac/install/

> Ubuntu : https://docs.docker.com/install/linux/docker-ce/ubuntu/

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

1). View the details on the container while your web server is running (with docker container ls or docker ps)
2). Stop and remove containers and images with the following commands. Use the “all” flag (--all or -a) to view stopped containers.

Stop container
> docker stop < container name >

Delete Contaner
> docker rm  097b4b431690

List of Contaner with used image
> docker ps -a

Delete All image
> docker images -a | awk '{print $3}' | xargs docker rmi

Delete Image
> docker rmi Image Image

Login to docker Image
> docker run -it < image name >

> docker exec -it < container name > 

EG
> docker exec -it apache2 bash

Close all Docker
> docker-compose down
 


# Docker PHPMYADMIN DOC
> https://hub.docker.com/r/phpmyadmin/phpmyadmin

#Using Supervisor with Docker to manage processes
Referance Url
> https://blog.trifork.com/2014/03/11/using-supervisor-with-docker-to-manage-processes-supporting-image-inheritance/




