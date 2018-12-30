# 关闭IPv6

### 1. CentOS 7

> 插入值"ipv6.disable=1"

<pre>
# cat /etc/default/grub
---------- 
    GRUB_TIMEOUT=5
    GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
    GRUB_DEFAULT=saved
    GRUB_DISABLE_SUBMENU=true
    GRUB_TERMINAL_OUTPUT="console"
--- GRUB_CMDLINE_LINUX="rd.lvm.lv=centos/root rd.lvm.lv=centos/swap net.ifnames=0 biosdevname=0 rhgb quiet"
+++ GRUB_CMDLINE_LINUX="ipv6.disable=1 rd.lvm.lv=centos/root rd.lvm.lv=centos/swap net.ifnames=0 biosdevname=0 rhgb quiet"
    GRUB_DISABLE_RECOVERY="true"

# grub2-mkconfig -o /boot/grub2/grub.cfg
----------
    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.10.0-862.14.4.el7.x86_64
    Found initrd image: /boot/initramfs-3.10.0-862.14.4.el7.x86_64.img
    Found linux image: /boot/vmlinuz-3.10.0-862.11.6.el7.x86_64
    Found initrd image: /boot/initramfs-3.10.0-862.11.6.el7.x86_64.img
    Found linux image: /boot/vmlinuz-3.10.0-862.el7.x86_64
    Found initrd image: /boot/initramfs-3.10.0-862.el7.x86_64.img
    Found linux image: /boot/vmlinuz-0-rescue-2405f3748e924b1b99fa6a85dc49ae5e
    Found initrd image: /boot/initramfs-0-rescue-2405f3748e924b1b99fa6a85dc49ae5e.img
    done
</pre>


### END

