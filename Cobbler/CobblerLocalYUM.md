# Configure Local YUM on Cobbler

## 1. Add repo

<pre>
cobbler repo add --name=CentOS7.5-openstack-rocky \
--mirror=https://mirrors.aliyun.com/centos/7.5.1804/cloud/x86_64/openstack-rocky/ \
--arch=x86_64 --breed=yum
</pre>

    cobbler repo list
    ----------
    CentOS7.5-openstack-rocky

## 2. Sync YUM to Local

> Mirror Path:
>> /var/www/cobbler/repo_mirror


<pre>
cobbler reposync 
----------
...
received on stderr: 
running: createrepo  -c cache -s sha /var/www/cobbler/repo_mirror/CentOS7.5-openstack-rocky
received on stdout: Spawning worker 0 with 711 pkgs
Spawning worker 1 with 710 pkgs
Workers Finished
Saving Primary metadata
Saving file lists metadata
Saving other metadata
Generating sqlite DBs
Sqlite DBs complete

received on stderr: 
running: chown -R root:apache /var/www/cobbler/repo_mirror/CentOS7.5-openstack-rocky
received on stdout: 
received on stderr: 
running: chmod -R 755 /var/www/cobbler/repo_mirror/CentOS7.5-openstack-rocky
received on stdout: 
received on stderr: 
*** TASK COMPLETE ***
</pre>

## 3. Add this repo to cobbler profile

> This "CentOS7.5-x86_64" profile not have repo 

<pre>
cobbler profile report
----------
Name                           : CentOS7.5-x86_64
TFTP Boot Files                : {}
Comment                        : 
DHCP Tag                       : default
Distribution                   : CentOS7.5-x86_64
Enable gPXE?                   : 0
Enable PXE Menu?               : 1
Fetchable Files                : {}
Kernel Options                 : {'biosdevname': '0', 'net.ifnames': '0'}
Kernel Options (Post Install)  : {}
Kickstart                      : /var/lib/cobbler/kickstarts/CentOS7.5-x86_64.ks
Kickstart Metadata             : {}
Management Classes             : []
Management Parameters          : <<inherit>>
Name Servers                   : []
Name Servers Search Path       : []
Owners                         : ['admin']
Parent Profile                 : 
Internal proxy                 : 
Red Hat Management Key         : <<inherit>>
Red Hat Management Server      : <<inherit>>
Repos                          : []
Server Override                : <<inherit>>
Template Files                 : {}
Virt Auto Boot                 : 1
Virt Bridge                    : xenbr0
Virt CPUs                      : 1
Virt Disk Driver Type          : raw
Virt File Size(GB)             : 5
Virt Path                      : 
Virt RAM (MB)                  : 512
Virt Type                      : kvm
</pre>

> Add "CentOS7.5-openstack-rocky" repo to "CentOS7.5-x86_64" profile

<pre>
cobbler profile edit --name=Centos7.5-x86_64 --repos="CentOS7.5-openstack-rocky"
----------
[root@cobbler ~]#cobbler profile report | grep Repos
Repos                          : ['CentOS7.5-openstack-rocky']
</pre>

## 4. New Server repo

> You can use local yum repo

<pre>
cat /etc/yum.repos.d/cobbler-config.repo 
----------
[core-0]
name=core-0
baseurl=http://10.1.0.161/cobbler/ks_mirror/CentOS7.5-x86_64
enabled=1
gpgcheck=0
priority=1


[CentOS7.5-openstack-rocky]
name=CentOS7.5-openstack-rocky
baseurl=http://10.1.0.161/cobbler/repo_mirror/CentOS7.5-openstack-rocky
enabled=1
priority=99
gpgcheck=0

</pre>

## END