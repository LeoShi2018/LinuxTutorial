# Zabbix 4.0 LTS Deploy
> Ver1.0

## 1. System info

- CentOS 7.5 x86_64 1804
- VM 2Core/2G/100G



## 2.Config the fast repo mirror

#### Install Aliyun mirror

> Bakckup local repo files 

<pre>
/etc/yum.repos.d/
for var in `ls`; do mv -f "$var" `echo "$var" |sed 's/repo$/repo.bak/'`; done
</pre>

> Zabbix 4.0 repo
<pre>
cat <<'EOF' > /etc/yum.repos.d/zabbix.repo
[root@localhost yum.repos.d]# cat zabbix.repo 
[zabbix]
name=Zabbix Official Repository - $basearch
baseurl=http://mirrors.aliyun.com/zabbix/zabbix/4.0/rhel/7/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-ZABBIX-A14FE591

[zabbix-non-supported]
name=Zabbix Official Repository non-supported - $basearch 
baseurl=http://mirrors.aliyun.com/zabbix/non-supported/rhel/7/$basearch/
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-ZABBIX
gpgcheck=1
EOF
curl -o /etc/pki/rpm-gpg/RPM-GPG-KEY-ZABBIX https://mirrors.aliyun.com/zabbix/RPM-GPG-KEY-ZABBIX
curl -o /etc/pki/rpm-gpg/RPM-GPG-KEY-ZABBIX-A14FE591 https://mirrors.aliyun.com/zabbix/RPM-GPG-KEY-ZABBIX-A14FE591
</pre>

> CentOS 7.5 repo

<pre>
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
</pre>

> EPEL 7

<pre>
curl -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
</pre>

<pre>
yum clean all && yum repolist
----------
    Loaded plugins: fastestmirror
    Cleaning repos: base epel extras updates zabbix zabbix-non-supported
    Cleaning up everything
    Maybe you want: rm -rf /var/cache/yum, to also free up space taken by orphaned data from disabled or removed repos
    Cleaning up list of fastest mirrors
    Loaded plugins: fastestmirror
    Determining fastest mirrors
     * base: mirrors.aliyun.com
     * extras: mirrors.aliyun.com
     * updates: mirrors.aliyun.com
    base                                                                               | 3.6 kB  00:00:00     
    epel                                                                               | 3.2 kB  00:00:00     
    extras                                                                             | 3.4 kB  00:00:00     
    updates                                                                            | 3.4 kB  00:00:00     
    zabbix                                                                             | 2.9 kB  00:00:00     
    zabbix-non-supported                                                               |  951 B  00:00:00     
    (1/8): base/7/x86_64/group_gz                                                      | 166 kB  00:00:00     
    (2/8): epel/x86_64/group_gz                                                        |  88 kB  00:00:00     
    (3/8): epel/x86_64/updateinfo                                                      | 929 kB  00:00:00     
    (4/8): zabbix/x86_64/primary_db                                                    |  26 kB  00:00:00     
    (5/8): extras/7/x86_64/primary_db                                                  | 205 kB  00:00:01     
    (6/8): epel/x86_64/primary                                                         | 3.6 MB  00:00:01     
    (7/8): updates/7/x86_64/primary_db                                                 | 6.0 MB  00:00:01     
    (8/8): base/7/x86_64/primary_db                                                    | 5.9 MB  00:00:02     
    zabbix-non-supported/x86_64/primary                                                | 1.6 kB  00:00:00     
    epel                                                                                          12716/12716
    zabbix-non-supported                                                                                  4/4
    repo id                                repo name                                                    status
    base/7/x86_64                          CentOS-7 - Base - mirrors.aliyun.com                          9,911
    epel/x86_64                            Extra Packages for Enterprise Linux 7 - x86_64               12,716
    extras/7/x86_64                        CentOS-7 - Extras - mirrors.aliyun.com                          434
    updates/7/x86_64                       CentOS-7 - Updates - mirrors.aliyun.com                       1,614
    zabbix/x86_64                          Zabbix Official Repository - x86_64                              41
    zabbix-non-supported/x86_64            Zabbix Official Repository non-supported - x86_64                 4
    repolist: 24,720
</pre>

## 2. Update system

    yum -y update

## 3. Disable SElinux

<pre>
setenforce 0 && sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
</pre>


## 4.Open Ports

- Zabbix Web use port 80
- Zabbix Server use port 10051 

<pre>
firewall-cmd --add-port=80/tcp --permanent
firewall-cmd --add-port=10051/tcp --permanent
firewall-cmd --reload
</pre>

## 5.Install Package
    yum install -y zabbix-server-mysql zabbix-web-mysql zabbix-agent mariadb mariadb-server net-tools

## 6.Change Zabbix time zone
    sed -i 's%# php_value date.timezone Europe/Riga%php_value date.timezone Asia/Shanghai%g' /etc/httpd/conf.d/zabbix.conf
    
<pre>
cat /etc/httpd/conf.d/zabbix.conf | grep timezone
----------
    php_value date.timezone Asia/Shanghai
</pre>

## 7.Start mariadb
    systemctl start mariadb && systemctl enable mariadb

## 8.Config mariadb

<pre>
mysql -u root
----------
    MariaDB [(none)]>create database zabbix character set utf8 collate utf8_bin;
    MariaDB [(none)]>grant all privileges on zabbix.* to zabbix@localhost identified by 'password';
    MariaDB [(none)]>quit;
</pre>

## 9.Set zabbix db
<pre>
zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -ppassword zabbix
</pre>

## 10.Config zabbix server conf file
<pre>
sed -i 's%# DBPassword=%DBPassword=password%g' /etc/zabbix/zabbix_server.conf

cat /etc/zabbix/zabbix_server.conf | grep DBPass
----------
    ### Option: DBPassword
    DBPassword=password
</pre>

## 11.Start zabbix server and apache

<pre>
systemctl start zabbix-server httpd && systemctl enable zabbix-server httpd

systemctl status zabbix-server httpd | grep active
----------
    Active: active (running) since Sun 2018-12-02 08:41:47 CST; 15s ago
    Active: active (running) since Sun 2018-12-02 08:41:47 CST; 15s ago

</pre>

## 12.GUI setup

    http://10.1.0.161/zabbix/setup.php

![Zabbix web](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Zabbix/images/image001.png)

> Next Step

![Zabbix web](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Zabbix/images/image002.png)

> Input your db password. This demo "password"

![Zabbix web](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Zabbix/images/image003.png)

> Custom your server name

![Zabbix web](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Zabbix/images/image004.png)

> Check your config and Next

![Zabbix web](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Zabbix/images/image005.png)

> Finish

![Zabbix web](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Zabbix/images/image006.png)

- DefaultName: Admin
- DefaultPassword: zabbix

![Zabbix web](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Zabbix/images/image007.png)

![Zabbix web](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Zabbix/images/image008.png)

## END
