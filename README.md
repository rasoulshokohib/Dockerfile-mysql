# Dockerfile Mysql


FROM ubuntu:16.04

RUN apt-get update \
 && DEBIAN_FRONTEND=noninteractive apt-get install -y mysql-server \
 && sed -i "s/127.0.0.1/0.0.0.0/g" /etc/mysql/mysql.conf.d/mysqld.cnf

ENV DB_USER mohsen
ENV DB_PASSWORD 123qwe#@!
ENV DB_NAME tradeApi
ENV VOLUME_HOME "/var/lib/mysql"

# Create Database
RUN mkdir /usr/sql
RUN chmod 744 /usr/sql
RUN /etc/init.d/mysql start && \
        mysql -uroot  -e "CREATE DATABASE ${DB_NAME}"

VOLUME ["/var/lib/mysql"]     
EXPOSE 3306
CMD ["mysqld_safe"]
