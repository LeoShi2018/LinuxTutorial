# Koan reinstall System

## 1. Yum install koan package

### This operation must be at client not cobbler server
> You must have epel repo

    rpm -ivh https://mirrors.aliyun.com/epel/epel-release-latest-7.noarch.rpm

    yum install -y koan

<pre>
koan --server=10.1.0.161 --list=profiles
----------
- looking for Cobbler at http://10.1.0.161:80/cobbler_api
Centos7.5-x86_64
</pre>
<pre>
koan --replace-self --server=10.1.0.161 --profile=CentOS7.5-x86_64
----------
- looking for Cobbler at http://10.1.0.161:80/cobbler_api
- reading URL: http://10.1.0.161/cblr/svc/op/ks/profile/Centos7.5-x86_64
install_tree: http://10.1.0.161/cblr/links/CentOS7.5-x86_64
downloading initrd initrd.img to /boot/initrd.img_koan
url=http://10.1.0.161/cobbler/images/CentOS7.5-x86_64/initrd.img
- reading URL: http://10.1.0.161/cobbler/images/CentOS7.5-x86_64/initrd.img
downloading kernel vmlinuz to /boot/vmlinuz_koan
url=http://10.1.0.161/cobbler/images/CentOS7.5-x86_64/vmlinuz
- reading URL: http://10.1.0.161/cobbler/images/CentOS7.5-x86_64/vmlinuz
- ['/sbin/grubby', '--add-kernel', '/boot/vmlinuz_koan', '--initrd', '/boot/initrd.img_koan', '--args', '"ksdevice=link lang= text net.ifnames=0 ks=http://10.1.0.161/cblr/svc/op/ks/profile/Centos7.5-x86_64 biosdevname=0 kssendmac "', '--copy-default', '--make-default', '--title=kick1543554473']
- ['/sbin/grubby', '--update-kernel', '/boot/vmlinuz_koan', '--remove-args=root']
- reboot to apply changes
</pre>

    init 6

![Boot Menu](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Cobbler/images/images010.png)


## END