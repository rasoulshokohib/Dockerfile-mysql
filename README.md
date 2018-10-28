# mysql_container
Dockerfile Mysql
#!/bin/bash

#ubuntu softwer update.
apt-get update;
echo "ubuntu softwer update";
apt-get install mysql-server;
echo "install Mysql successfully";
#Run the MySQL Secure Installation wizard
mysql_secure_installation

service mysql restart
echo "conf Mysql successfully"
# systemctl status mysql.service:
UP=$(pgrep mysql | wc -l);
if [ "$UP" -ne 1 ];
then
        echo "MySQL is down.";
        sudo service mysql start

else
        echo "All is well.";
fi
	echo "Please enter root user MySQL password!"
	read rootpasswd
	echo "Please enter the NAME of the new MYSQL database! (example: database1)"
	read dbname
	echo "Please enter the MYSQL database CHARACTER SET! (example: latin1, utf8, ...)"
	read charset
	echo "Creating new MYSQL database..."
	mysql -uroot -p${rootpasswd} -e "CREATE DATABASE ${dbname} /*\!40100 DEFAULT CHARACTER SET ${charset} */;"
	echo "Database successfully created!"
	echo "Showing existing databases..."
	mysql -uroot -p${rootpasswd} -e "show databases;"
	echo ""
	echo "Please enter the NAME of the new MYSQL database user! (example: user1)"
	read username
	echo "Please enter the PASSWORD for the new MYSQL database user!"
	read userpass
	echo "Creating new user..."
        mysql -uroot -p${rootpasswd} -e "use ${dbname}"
        echo "use ${dbname}"
        echo ""
	mysql -uroot -p${rootpasswd} -e "CREATE USER ${username}@localhost IDENTIFIED BY '${userpass}'"
	echo "User successfully created!"
	echo ""

	echo "Granting ALL privileges on ${dbname} to ${username}!"
	mysql -uroot -p${rootpasswd} -e "GRANT ALL PRIVILEGES ON ${dbname}.users_table TO '${username}'@'localhost'"
	
        mysql -uroot -p${rootpasswd} -e "FLUSH PRIVILEGES;"
	echo "You're good now"
        mysql -u${username} -p${userpass} -e "use tradeApi;"
echo "change database tradeApi"
        mysql -u${username} -p${userpass} -e "create table tradeApi.users_table ( UUID char(36) not null primary key,
UserName varchar(35) null,
EmailAddress varchar(255) null,
PasswordHash varchar(64) null,
Verified  tinyint(1) default '0' null,
constraint users_table_UserName_uindex   unique (UserName) );"
echo "create table successfully"
	exit
fi


