#Dockerfile Mysql
#Dockerfile Mysql
FROM mysql

RUN apt-get update
RUN apt-get install mysql-server
RUN mysql_secure_installation

ENV DB_USER mohsen
ENV DB_PASSWORD 123qwe#@!
ENV DB_NAME tradeApi
ENV VOLUME_HOME "/var/lib/mysql"

EXPOSE 3306 

RUN cp /etc/mysql/my.cnf /usr/share/mysql/my-default.cnf
RUN /usr/bin/mysqld  && sleep 5 && \
     mysql -uroot -e "CREATE DATABASE ${DB_NAME}" && \
     mysql -uroot -e "CREATE USER '${DB_USER}'@'%' IDENTIFIED BY '${DB_PASSWORD}'" && \
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
