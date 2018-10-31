# Dockerfile Mysql

FROM Uubuntu:16.04

RUN apt-get update \
 && DEBIAN_FRONTEND=noninteractive apt-get install -y mysql-server \
 && sed -i "s/127.0.0.1/0.0.0.0/g" /etc/mysql/mysql.conf.d/mysqld.cnf \
 && mkdir /var/run/mysqld \
 && chown -R mysql:mysql /var/run/mysqld

ENV DB_USER mohsen
ENV DB_PASSWORD 123qwe#@!
ENV DB_NAME tradeApi
ENV VOLUME_HOME "/var/lib/mysql"



RUN
     mysql -uroot -e "CREATE DATABASE ${DB_NAME}" && \
     mysql -uroot -e "CREATE USER '${DB_USER}'@'localhost' IDENTIFIED BY '${DB_PASSWORD}'" && \
     mysql -uroot -e "GRANT ALL PRIVILEGES ON ${DB_NAME}.users_table TO '${DB_USER}'@'localhost'" &&\
     mysql -uroot -e "FLUSH PRIVILEGES" &&\
     mysql -u${DB_USER} -e "use tradeApi" &&\
     mysql -u${DB_USER} -e "create table tradeApi.users_table ( UUID char(36) not null primary key,
     UserName varchar(35) null,
     EmailAddress varchar(255) null,
     PasswordHash varchar(64) null,
     Verified  tinyint(1) default '0' null,
     constraint users_table_UserName_uindex   unique (UserName) )" &&\
     mysqladmin -uroot shutdown
     
     
VOLUME ["/var/lib/mysql"]     
EXPOSE 3306
CMD ["mysqld_safe"]
