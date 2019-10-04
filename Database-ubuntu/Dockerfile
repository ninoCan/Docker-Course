############################################################
# Dockerfile to build MySQL server container
# Based on the official ubuntu 18.04 image
############################################################

    FROM ubuntu:18.04
    ENV  DEBIAN_FRONTEND noninteractive

    RUN apt-get update && apt-get install mysql-server
    #RUN mysql_secure_installation
    RUN mysql -u root < 'CREATE DATABASE '
    EXPOSE 3306
    CMD ["mysqld"]
