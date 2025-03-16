---
title: "Mysql Replication"
date: 2023-03-03 00:00:00 +0800
categories: [DevOps]
tags: [Replication]
---  
**MASTER : 172.16.0.211**
**SLAVE : 172.16.0.212**

Step 1: Install MYSql server and client on both the Master and Slave Systems

```
sudo apt-get install mysql-server mysql-client -y
```

Step 2 : Configure Secure Installation
	
```
sudo mysql_secure_installation
```

**MASTER**

Step 1 : navigate to mysql folder

```
cd /etc/mysql/mysql.conf.d
```

Step 2 : open mysql configuration file

```
nano mysqld.cnf
```

1. Comment the bind address
2. Set-up a server-id and uncomment it
3. uncomment log file 

Step 3 : Login to mysql

```
 mysql -u root -p
```

Step 4 : Create a replication user on master and provide access from slave.

```
CREATE USER 'replication_user'@'172.16.0.212' IDENTIFIED WITH mysql_native_password BY '54612e322cee57a5dcd';
```

Step 5: Grant slave status to the user specified 

```
GRANT REPLICATION SLAVE ON *.* TO 'replication_user'@'172.16.0.212';
```

Step 6: Dump all the sql data

```
mysqldump --all-databases --routines --events --apply-replica-statements --delete-source-logs --single-transaction > /tmp/mysqldump.sql
```


**SLAVE**

Step 1: Set replication source as the master server.  

```
CHANGE MASTER TO MASTER_HOST='172.16.0.211', MASTER_USER='replication_user', MASTER_PASSWORD='54612e322cee57a5dcd';  
```

Step 2 : Restore the mysql dump from earlier, ensure there are no errors.  

```
mysql < /tmp/mysqldump.sql  
```

Step 3 : Start replication in the slave. If you are restarting replication, you may first have to stop slave; before starting the slave process.  

```
start slave;  
```
```
show slave status\G
```

*Check the log and status files in case of any errors that are to be rectified*