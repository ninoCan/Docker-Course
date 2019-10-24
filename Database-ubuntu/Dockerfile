############################################################
# Dockerfile to build MySQL server container
# Based on the official ubuntu 18.04 image
############################################################

    FROM ubuntu:18.04
    ENV  DEBIAN_FRONTEND noninteractive

    RUN apt-get update && apt-get install -y debconf-utils
    RUN  sudo debconf-set-selections <<< "mysql-server mysql-server/root_password password getenv('MYSQL_HOST_PASSWORD')"
    RUN sudo debconf-set-selections <<< "mysql-server mysql-server/root_password_again password getenv('MYSQL_HOST_PASSWORD')"
    RUN apt-get install -y mysql-server
    RUN sed -i "s/127.0.0.1/0.0.0.0/" /etc/mysql/mysql.conf.d/mysqld.cnf
    RUN mysql -u root -p"getenv('MYSQL_HOST_PASSWORD')" <<EOF\nCREATE DATABASE wordpress;\nGRANT ALL PRIVILEGES ON wordpress.* TO 'dbuser'@'%' IDENTIFIED BY 'dbpass';\nGRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root';\nFLUSH PRIVILEGES;\nEOF
    EXPOSE 3306
    CMD ["mysqld"]
