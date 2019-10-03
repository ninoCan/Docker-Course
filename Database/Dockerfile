############################################################
# Dockerfile to build a MySQL database server
# Based on Ubuntu
############################################################
    FROM mysql/mysql-server:5.7
    #load the base image

    MAINTAINER nino <ninocangialosi@yahoo.it>

####UNNECESSARY/DEPRECATED LINES from testing phase
    # ENV MYSQL_ROOT_PASSWORD=root
    # ENV MYSQL_USER=dbuser
    # ENV MYSQL_PASSWORD=dbpass
    # ENV MYSQL_DATABASE=wordpress
    # RUN sed -i 's/127.0.0.1/0.0.0.0/' /etc/mysql/mysql.conf.d/mysqld.cnf
    WORKDIR /etc/my.cnf.d/
    COPY mysqld.cnf .
    #attach the mysql.cnf file
    WORKDIR /

    EXPOSE    3306
    #open the stardard port for external connection
    CMD ["mysqld"]
    #reintialize the mysql server
