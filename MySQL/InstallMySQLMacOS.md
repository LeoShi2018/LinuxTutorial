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
### 6. Initialization MySQL

````
$ mysql_secure_installation
----------
    
    Securing the MySQL server deployment.
    
    Connecting to MySQL using a blank password.
    
    VALIDATE PASSWORD COMPONENT can be used to test passwords
    and improve security. It checks the strength of password
    and allows the users to set only those passwords which are
    secure enough. Would you like to setup VALIDATE PASSWORD component?
    
    # Change your root password,Input "yes"
    Press y|Y for Yes, any other key for No: yes
    
    There are three levels of password validation policy:
    
    LOW    Length >= 8
    MEDIUM Length >= 8, numeric, mixed case, and special characters
    STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file
    
    
    Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 0
    Please set the password for root here.

    New password: 
    
    Re-enter new password: 
    
    Estimated strength of the password: 50 
    Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : yes
    By default, a MySQL installation has an anonymous user,
    allowing anyone to log into MySQL without having to have
    a user account created for them. This is intended only for
    testing, and to make the installation go a bit smoother.
    You should remove them before moving into a production
    environment.
    
    Remove anonymous users? (Press y|Y for Yes, any other key for No) : yes
    Success.
    
    
    Normally, root should only be allowed to connect from
    'localhost'. This ensures that someone cannot guess at
    the root password from the network.

    Disallow root login remotely? (Press y|Y for Yes, any other key for No) : yes
    Success.
    
    By default, MySQL comes with a database named 'test' that
    anyone can access. This is also intended only for testing,
    and should be removed before moving into a production
    environment.
    
    
    Remove test database and access to it? (Press y|Y for Yes, any other key for No) : yes
     - Dropping test database...
    Success.
    
     - Removing privileges on test database...
    Success.
    
    Reloading the privilege tables will ensure that all changes
    made so far will take effect immediately.
    Reload privilege tables now? (Press y|Y for Yes, any other key for No) : yes
    Success.
    
    All done! 
````

### 7. Login MySQL use root user

````
$ mysql -uroot -p
----------
    Enter password: 
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 17
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




