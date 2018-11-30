# PXE Menu Security

#### 1.Increase the password of the installation security setting menu

Use openssl create a random password

    openssl passwd -1 -salt $(openssl rand 15 -base64) 'password'
    -------------
    $1$TZnHBKw5$2OsBBc4C8lRRy5P1cXZBZ0

#### 2. Modify /etc/cobbler/pxe/pxedefault.template

>Add line 4
>>MENU MASTER PASSWD $1$TZnHBKw5$2OsBBc4C8lRRy5P1cXZBZ0

<pre>
vi /etc/cobbler/pxe/pxedefault.template
-------------
      1 DEFAULT menu
      2 PROMPT 0
      3 MENU TITLE Cobbler | http://cobbler.github.io/
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

#### 3. Modify /etc/cobbler/pxe/pxeprofile.template

>Add line 2
>>MENU PASSWD

<pre>
vi /etc/cobbler/pxe/pxeprofile.template
  1 LABEL $profile_name
  2         MENU PASSWD
  3         kernel $kernel_path
  4         $menu_label
  5         $append_line
  6         ipappend 2
</pre>

#### 4. Don't forget Cobbler Sync

![PXE Password](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Cobbler/images/images005.png)