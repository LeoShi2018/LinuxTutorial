# Customize PXE menu TITLE

#### 1.Modify /etc/cobbler/pxe/pxedefault.template

> Modify line 3
>> Maximum 56 characters

<pre>
vi /etc/cobbler/pxe/pxedefault.template
  1 DEFAULT menu
  2 PROMPT 0
-<3 MENU TITLE Cobbler | http://cobbler.github.io/
->3 MENU TITLE LeoShi Auto Install System | https://github.com/LeoShi2018
  4 MENU MASTER PASSWD $1$TZnHBKw5$2OsBBc4C8lRRy5P1cXZBZ0
  5 TIMEOUT 200
  6 TOTALTIMEOUT 6000
  7 ONTIMEOUT $pxe_timeout_profile
  8 
  9 LABEL local
 10         MENU LABEL (local)
 11         MENU DEFAULT
 12         LOCALBOOT -1
 13 
 14 $pxe_menu_items
 15 
 16 MENU end
</pre>

#### 2. cobbler sync

![PXE Menu](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Cobbler/images/images006.png)


#### 3. Pxe config file

> /var/lib/tftpboot/pxelinux.cfg/default

<pre>
cat /var/lib/tftpboot/pxelinux.cfg/default
----------
      1 DEFAULT menu
      2 PROMPT 0
      3 MENU TITLE LeoShi Auto Install System | https://github.com/LeoShi2018
      4 MENU MASTER PASSWD $1$TZnHBKw5$2OsBBc4C8lRRy5P1cXZBZ0
      5 TIMEOUT 200
      6 TOTALTIMEOUT 6000
      7 ONTIMEOUT local
      8 
      9 LABEL local
     10         MENU LABEL (local)
     11         MENU DEFAULT
     12         LOCALBOOT -1
     13 
     14 LABEL Centos7.5-x86_64
     15         MENU PASSWD
     16         kernel /images/CentOS7.5-x86_64/vmlinuz
     17         MENU LABEL Centos7.5-x86_64
     18         append initrd=/images/CentOS7.5-x86_64/initrd.img ksdevice=bootif lang=  text net.ifnames=0 biosdevname=0 kssen
        dmac  ks=http://10.1.0.161/cblr/svc/op/ks/profile/Centos7.5-x86_64
     19         ipappend 2
     20 
->   21 LABEL Andy SU
     22 
     23 MENU end
</pre>

![PXE Menu](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Cobbler/images/images009.png)

## END