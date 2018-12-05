# Change Root Password

## 1. Stop mysqld.service

    # systemctl stop mysqld

## 2. Edit /etc/my.cnf

    # sed -i '/^\[mysqld\]/a\skip-grant-tables' /etc/my.cnf

## 3. Restart mysqld.service

    # systemctl restart mysqld

## 4. Change password
<pre>
# mysql -u root
----------
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 2
    Server version: 5.6.42 MySQL Community Server (GPL)
    
    Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql> USE mysql ; 
    Reading table information for completion of table and column names
    You can turn off this feature to get a quicker startup with -A
    
    Database changed
mysql> UPDATE user SET Password = password ( 'password' ) WHERE User = 'root' ;              
----------
    #'password' is new password
    Query OK, 1 row affected (0.01 sec)
    Rows matched: 1  Changed: 1  Warnings: 0

mysql> flush privileges ; 
----------
    Query OK, 0 rows affected (0.00 sec)

mysql> exit
----------
    Bye
</pre>

## 5. Edit back /etc/my.cnf

    # sed -i '/skip-grant-tables/d' /etc/my.cnf

## 6. Restart mysqld.service

    # systemctl restart mysqld
    
## END