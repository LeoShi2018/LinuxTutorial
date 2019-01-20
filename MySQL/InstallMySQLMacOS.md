# How to install MySQL Server on your MacBook with brew tools.

### 1. Environment

My computer is MacBook.
My MacOS version is 10.14.2.
> MacOS:10.14.2

````
$ brew --version
----------
    Homebrew 1.9.2
    Homebrew/homebrew-core (git revision bdef; last commit 2019-01-15)
````
### 2. Use brew tools search mysql package.

````
$ brew search | grep mysql
----------    
    automysqlbackup
    mysql
    mysql++
    mysql-client
    mysql-cluster
    mysql-connector-c
    mysql-connector-c++
    mysql-sandbox
    mysql-search-replace
    mysql-utilities
    mysql@5.5
    mysql@5.6
    mysql@5.7
    mysqltuner
````

### 3. Install

Install mysql database with brew tools.

````
$ brew install mysql
----------
    ==> Downloading https://homebrew.bintray.com/bottles/mysql-8.0.13.mojave.bottle.tar.gz
    ######################################################################## 100.0%
    ==> Pouring mysql-8.0.13.mojave.bottle.tar.gz
    ==> /usr/local/Cellar/mysql/8.0.13/bin/mysqld --initialize-insecure --user=leoshi --basedir=/usr/local/Cellar/
    ==> Caveats
    We've installed your MySQL database without a root password. To secure it run:
        mysql_secure_installation
    
    MySQL is configured to only allow connections from localhost by default
    
    To connect run:
        mysql -uroot
    
    To have launchd start mysql now and restart at login:
      brew services start mysql
    Or, if you don't want/need a background service you can just run:
      mysql.server start
    ==> Summary
    /usr/local/Cellar/mysql/8.0.13: 267 files, 236.6MB

````

### 4. Start Service

````
# 1. Backgroud Service
$ brew services start mysql
----------
    ==> Tapping homebrew/services
    Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-services'...
    remote: Enumerating objects: 17, done.
    remote: Counting objects: 100% (17/17), done.
    remote: Compressing objects: 100% (14/14), done.
    remote: Total 17 (delta 0), reused 12 (delta 0), pack-reused 0
    Unpacking objects: 100% (17/17), done.
    Tapped 1 command (50 files, 62.2KB).
    ==> Successfully started `mysql` (label: homebrew.mxcl.mysql)

# 2. Just run
$ mysql.server start
----------
    Starting MySQL
    .. SUCCESS! 

````

### 5. Stop Service

````
# 1. Use brew tools
$ brew services stop mysql 
----------
    Stopping `mysql`... (might take a while)
    ==> Successfully stopped `mysql` (label: homebrew.mxcl.mysql)

# 2. Just stop
$ mysql.server stop
----------
    Shutting down MySQL
    ... SUCCESS! 

````
### 6. Login MySQL use root user

````
$ mysql -uroot
----------
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 9
    Server version: 8.0.13 Homebrew
    
    Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

    mysql> 
````
When you see the "mysql>" prompts, the login is successful.

## END




