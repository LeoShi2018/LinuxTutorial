# Cobbler Web

#### 1. Yum install Cobbler web

    yum -y cobbler-web


#### 2. Access address link

> https://\<cobbler server ip address>/cobbler_web

    https://10.1.0.161/cobbler_web/

    Username:cobbler
    
    Password:cobbler

![Cobbler web](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Cobbler/images/images007.png)


![Cobbler web](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Cobbler/images/images008.png)

#### 3. Change the cobbler password

    htdigest /etc/cobbler/users.digest "Cobbler" cobbler  


#### 4. Add a new user

    htdigest /etc/cobbler/users.digest "Cobbler" your_newname  
    

#### END