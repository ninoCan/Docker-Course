############################################################
# Dockerfile to build Linux Apache PHP server with WordPress
# Based on the official php:7.2-apache
############################################################

    FROM php:7.2-apache
    #load the php image with apache2 preinstalled

    MAINTAINER nino <ninocangialosi@yahoo.it>
    #specify the mainteiner of this image (me=Antonino Cangialosi)

    RUN docker-php-ext-install mysqli pdo_mysql
    # add the mysql needed extensions for php

    WORKDIR /var/www/html/
    # change into this directory on the run

    COPY ./index.php /var/www/html/
    #copy the content of an hello php file from the locale to that path

#version 1.0 starts here
    COPY ./latest.tar.gz /var/www/
    #attach the latest (at time of image building) WordPress installation
    WORKDIR /var/www/
    #move the starting directory to /var/www/
    RUN tar -xzvf latest.tar.gz -C /var/www/
    #expand the package
    RUN rm latest.tar.gz
    #remove the taz.gz package from the container. Version 1.1
    RUN mv /var/www/wordpress/wp-config-sample.php /var/www/wordpress/wp-config.php
    #create the WordPress configuration file out of the available sample
    RUN sed -i "s/'database_name_here'/getenv('DB_NAME')/" /var/www/wordpress/wp-config.php
    RUN sed -i "s/'username_here'/getenv('DB_USER')/" /var/www/wordpress/wp-config.php
    RUN sed -i "s/'password_here'/getenv('DB_PASSWORD')/" /var/www/wordpress/wp-config.php
    RUN sed -i "s/'localhost'/getenv('DB_HOST')/" wordpress/wp-config.php
    # define and put in place the variables needed to be inputed at time of the container initialization
    RUN cd /etc/apache2/sites-enabled/
    RUN sed -i 's/\/var\/www\/html/\/var\/www\/wordpress/' /etc/apache2/sites-enabled/000-default.conf
    #tells apache to load the wordpress page instead of the simple php file
    #version 1.0 stops here

    EXPOSE 80
    #opens port 80 for external comunication

    CMD ["apache2-foreground"]
    #run the final apache2 config
