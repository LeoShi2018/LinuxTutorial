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