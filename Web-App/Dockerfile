############################################################
# Dockerfile to build Linux Apache PHP server with WordPress
# Based on the official ubuntu 18.04 image
############################################################

    FROM ubuntu:18.04
    #import the official ubuntu docker image, version 18.04

    MAINTAINER nino <ninocangialosi@yahoo.it>
    #specify the mainteiner (me=Antonino Cangialosi)

    ENV  DEBIAN_FRONTEND noninteractive
    #skip the configuration questions during the ubuntu installation

    RUN apt-get update && apt-get -y install apache2 wget php php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip php-mysql
    #install the needed packages

####version 1.0 starts here
    WORKDIR /var/www
    #move to the www folder inside var
    RUN wget http://wordpress.org/latest.tar.gz
    #dowload the latest WordPress installation
    RUN tar -xzvf latest.tar.gz -C /var/www/
    #expand the tar.gz package
    RUN mv /var/www/wordpress/wp-config-sample.php /var/www/wordpress/wp-config.php
    #create the configuration file out of the already available Wordpress sample
    RUN sed -i 's/'database_name_here'/getenv("DB_NAME")/' /var/www/wordpress/wp-config.php
    RUN sed -i 's/'username_here'/getenv("DB_USER")/' /var/www/wordpress/wp-config.php
    RUN sed -i 's/'password_here'/getenv("DB_PASSWORD")/' /var/www/wordpress/wp-config.php
    RUN sed -i 's/'localhost'/getenv("DB_HOST")/' wordpress/wp-config.php
    #define the variables to be inputed at the time of container launch for the database configuration
    RUN cd /etc/apache2/sites-enabled/
    RUN sed -i 's/\/var\/www\/html/\/var\/www\/wordpress/' /etc/apache2/sites-enabled/000-default.conf
    #tells apache to load the wordpress page instead of the stardard initial page
    WORKDIR /
    #shift the working directory back to home for future login in with bash
####version 1.0 stops here

    EXPOSE 80
    #open the port 80 for external connection

    CMD ["apache2ctl","-D","FOREGROUND"]
    #run the apache service again
