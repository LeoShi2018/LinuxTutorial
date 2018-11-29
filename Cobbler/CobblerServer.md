# Cobbler Server Environment and Deploy
> V1.0

## 1.Config the fast repo mirror

Install Aliyun mirror

<pre>
rpm -ivh https://mirrors.aliyun.com/epel/epel-release-latest-7.noarch.rpm
rpm -ivh https://mirrors.aliyun.com/repo/Centos-7.repo
</pre>


## 2.Update CentOS7.5

<pre>
yum -y update
</pre>

##3.Install Package

<pre>
yum install -y httpd dhcp tftp cobbler cobbler-web pykickstart xinetd
</pre>

##4.Disable SElinux

<pre>
setenforce 0 && sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' \
/etc/sysconfig/selinux
</pre>

##5.Open Ports

<pre>
firewall-cmd --add-port=80/tcp --permanent
firewall-cmd --add-port=443/tcp --permanent
firewall-cmd --add-service=dhcp --permanent
firewall-cmd --add-port=69/tcp --permanent
firewall-cmd --add-port=69/udp --permanent
firewall-cmd --add-port=4011/udp --permanent
firewall-cmd --reload
</pre>

##6.Start apache cobbler Xinetd

<pre>
systemctl start httpd cobblerd xinetd rsyncd && \
systemctl enable httpd cobblerd xinetd rsyncd
</pre>

![Active](../images/images001.png)

##7.Cobbler check

<pre>
[root@cobbler ~]# cobbler check
The following are potential configuration items that you may want to fix:

1 : The 'server' field in /etc/cobbler/settings must be set to something other than localhost, or kickstarting features will not work.  This should be a resolvable hostname or IP for the boot server as reachable by all machines that will use it.
2 : For PXE to be functional, the 'next_server' field in /etc/cobbler/settings must be set to something other than 127.0.0.1, and should match the IP of the boot server on the PXE network.
3 : change 'disable' to 'no' in /etc/xinetd.d/tftp
4 : Some network boot-loaders are missing from /var/lib/cobbler/loaders, you may run 'cobbler get-loaders' to download them, or, if you only want to handle x86/x86_64 netbooting, you may ensure that you have installed a *recent* version of the syslinux package installed and can ignore this message entirely.  Files in this directory, should you want to support all architectures, should include pxelinux.0, menu.c32, elilo.efi, and yaboot. The 'cobbler get-loaders' command is the easiest way to resolve these requirements.
5 : enable and start rsyncd.service with systemctl
6 : debmirror package is not installed, it will be required to manage debian deployments and repositories
7 : The default password used by the sample templates for newly installed machines (default_password_crypted in /etc/cobbler/settings) is still set to 'cobbler' and should be changed, try: "openssl passwd -1 -salt 'random-phrase-here' 'your-password-here'" to generate new one
8 : fencing tools were not found, and are required to use the (optional) power management features. install cman or fence-agents to use them

Restart cobblerd and then run 'cobbler sync' to apply changes.
</pre>

####One by one processing problem
1.Edit /etc/cobbler/settings
<pre>
sed -i 's%server: 127.0.0.1%server: 10.1.0.161%g' /etc/cobbler/settings && \
sed -i 's%^server: 127.0.0.1%server: 10.1.0.161%g' /etc/cobbler/settings
</pre>


2.Edit /etc/cobbler/settings
<pre>
sed -i 's%^next_server: 127.0.0.1%next_server: 10.1.0.161%g' /etc/cobbler/settings
</pre>


3.Edit /etc/xinetd.d/tftp
<pre>
tftp_disable_conf=`grep -n disable /etc/xinetd.d/tftp|awk -F':' '{print $1}'`
sed -i '/disable/d' /etc/xinetd.d/tftp
sed -i "$tftp_disable_conf i\ disable = no" /etc/xinetd.d/tftp
</pre>


4.Run cobbler get-loaders

    cobbler get-loaders

<pre>task started: 2018-11-30_063912_get_loaders
task started (id=Download Bootloader Content, time=Fri Nov 30 06:39:12 2018)
downloading https://cobbler.github.io/loaders/README to /var/lib/cobbler/loaders/README
downloading https://cobbler.github.io/loaders/COPYING.elilo to /var/lib/cobbler/loaders/COPYING.elilo
downloading https://cobbler.github.io/loaders/COPYING.yaboot to /var/lib/cobbler/loaders/COPYING.yaboot
downloading https://cobbler.github.io/loaders/COPYING.syslinux to /var/lib/cobbler/loaders/COPYING.syslinux
downloading https://cobbler.github.io/loaders/elilo-3.8-ia64.efi to /var/lib/cobbler/loaders/elilo-ia64.efi
downloading https://cobbler.github.io/loaders/yaboot-1.3.17 to /var/lib/cobbler/loaders/yaboot
downloading https://cobbler.github.io/loaders/pxelinux.0-3.86 to /var/lib/cobbler/loaders/pxelinux.0
downloading https://cobbler.github.io/loaders/menu.c32-3.86 to /var/lib/cobbler/loaders/menu.c32
downloading https://cobbler.github.io/loaders/grub-0.97-x86.efi to /var/lib/cobbler/loaders/grub-x86.efi
downloading https://cobbler.github.io/loaders/grub-0.97-x86_64.efi to /var/lib/cobbler/loaders/grub-x86_64.efi
*** TASK COMPLETE ***
</pre>

5.Don't configer it


6.Set the root password is 'password'

<pre>
sed -i "s%^default_password_crypted.*%default_password_crypted: \
\"$(openssl passwd -1 -salt $(openssl rand 15 -base64) 'password')\"%g" \
/etc/cobbler/settings
</pre>

<pre>
[root@cobbler ~]# cat /etc/cobbler/settings | grep default_password  
default_password_crypted: "$1$o4Xpg+kr$WnqCduSuanQBK6Fu3CxNk0"
</pre>

##8.Restart cobbler service and recheck it.
<pre>
systemctl restart cobblerd
cobbler check

The following are potential configuration items that you may want to fix:

1 : debmirror package is not installed, it will be required to manage debian deployments and repositories
2 : fencing tools were not found, and are required to use the (optional) power management features. install cman or fence-agents to use them

Restart cobblerd and then run 'cobbler sync' to apply changes.
</pre>

These two error ignore

##9.Modify dhcp.template

<pre>
sed -i 's%manage_dhcp: 0%manage_dhcp: 1%g' /etc/cobbler/settings
dhcp_conf=`grep -n "subnet 192" /etc/cobbler/dhcp.template|awk -F':' '{print $1}'`
sed -i '/192.168/d' /etc/cobbler/dhcp.template
sed -i '/255.255.255.0/d' /etc/cobbler/dhcp.template
sed -i "21 i\subnet 10.1.0.0 netmask 255.255.255.0 { " /etc/cobbler/dhcp.template
sed -i "22 i\ option routers 10.1.0.2; " /etc/cobbler/dhcp.template
sed -i "23 i\ option domain-name-servers 114.114.114.114; " /etc/cobbler/dhcp.template
sed -i "24 i\ option subnet-mask 255.255.255.0; " /etc/cobbler/dhcp.template
sed -i "25 i\ range 10.1.0.100 10.1.0.200; " /etc/cobbler/dhcp.template
</pre>

##10.cobbler sync

This command must be executed after each modification

<pre>
systemctl restart cobblerd
cobbler sync

task started: 2018-11-30_065106_sync
task started (id=Sync, time=Fri Nov 30 06:51:06 2018)
running pre-sync triggers
cleaning trees
removing: /var/lib/tftpboot/grub/images
copying bootloaders
trying hardlink /var/lib/cobbler/loaders/pxelinux.0 -> /var/lib/tftpboot/pxelinux.0
trying hardlink /var/lib/cobbler/loaders/menu.c32 -> /var/lib/tftpboot/menu.c32
trying hardlink /var/lib/cobbler/loaders/yaboot -> /var/lib/tftpboot/yaboot
trying hardlink /usr/share/syslinux/memdisk -> /var/lib/tftpboot/memdisk
trying hardlink /var/lib/cobbler/loaders/grub-x86.efi -> /var/lib/tftpboot/grub/grub-x86.efi
trying hardlink /var/lib/cobbler/loaders/grub-x86_64.efi -> /var/lib/tftpboot/grub/grub-x86_64.efi
copying distros to tftpboot
copying images
generating PXE configuration files
generating PXE menu structure
rendering TFTPD files
generating /etc/xinetd.d/tftp
cleaning link caches
running post-sync triggers
running python triggers from /var/lib/cobbler/triggers/sync/post/*
running python trigger cobbler.modules.sync_post_restart_services
running shell triggers from /var/lib/cobbler/triggers/sync/post/*
running python triggers from /var/lib/cobbler/triggers/change/*
running python trigger cobbler.modules.scm_track
running shell triggers from /var/lib/cobbler/triggers/change/*
*** TASK COMPLETE ***
</pre>

##11.Mount your ISO

CentOS-7-x86_64-Minimal-1804.iso

    mount /dev/cdrom /mnt

##12.Import the images
<pre>
cobbler import --arch=x86_64 --path=/mnt --name=CentOS7.5

task started: 2018-11-30_065920_import
task started (id=Media import, time=Fri Nov 30 06:59:20 2018)
Found a candidate signature: breed=redhat, version=rhel6
Found a candidate signature: breed=redhat, version=rhel7
Found a matching signature: breed=redhat, version=rhel7
Adding distros from path /var/www/cobbler/ks_mirror/CentOS7.5-x86_64:
creating new distro: CentOS7.5-x86_64
trying symlink: /var/www/cobbler/ks_mirror/CentOS7.5-x86_64 -> /var/www/cobbler/links/CentOS7.5-x86_64
creating new profile: CentOS7.5-x86_64
associating repos
checking for rsync repo(s)
checking for rhn repo(s)
checking for yum repo(s)
starting descent into /var/www/cobbler/ks_mirror/CentOS7.5-x86_64 for CentOS7.5-x86_64
processing repo at : /var/www/cobbler/ks_mirror/CentOS7.5-x86_64
need to process repo/comps: /var/www/cobbler/ks_mirror/CentOS7.5-x86_64
looking for /var/www/cobbler/ks_mirror/CentOS7.5-x86_64/repodata/*comps*.xml
Keeping repodata as-is :/var/www/cobbler/ks_mirror/CentOS7.5-x86_64/repodata
*** TASK COMPLETE ***
</pre>
This is the path to the image.
> /var/www/cobbler/ks_mirror/CentOS7.5-x86_64

##13.Modify the kickstart file
<pre>
cat <<'EOF' > /var/lib/cobbler/kickstarts/CentOS7.5-x86_64.ks
#Kickstart Configurator for cobbler by LeoShi
#https://github.com/LeoShi2018
#platform=x86, AMD64, or Intel EM64T
#version=v201811
# System language
lang en_US.UTF-8
# Keyboard layouts
keyboard 'us'
# System timezone
timezone Asia/Shanghai
#Root password
rootpw --iscrypted $default_password_crypted
# Use text install
text
# Install OS instead of upgrade
install
# Use network installation
url --url=$tree
# System bootloader configuration
bootloader --location=mbr
# Firewall configuration
firewall --enabled --ssh
# System authorization information
auth  --useshadow  --passalgo=sha512


firstboot --disable
# SELinux configuration
selinux --disabled

# Clear the Master Boot Record
zerombr
# Partition clearing information
clearpart --all --initlabel
part pv.252 --fstype="lvmpv" --ondisk=sda --grow
part /boot --fstype="xfs" --ondisk=sda --size=200
volgroup centos --pesize=4096 pv.252
logvol /home  --fstype="xfs" --size=5120 --name=home --vgname=centos
logvol swap  --fstype="swap" --size=1024 --name=swap --vgname=centos
logvol /  --fstype="xfs" --size=1 --grow --name=root --vgname=centos

# Allow anaconda to partition the system as needed
#autopart

$yum_repo_stanza
# Network information
$SNIPPET('network_config')
# Reboot after installation
reboot

%pre
$SNIPPET('log_ks_pre')
$SNIPPET('kickstart_start')
#$SNIPPET('pre_install_network_config')
# Enable installation monitoring
$SNIPPET('pre_anamon')
%end

%packages
@^minimal
%end


%post --nochroot
$SNIPPET('log_ks_post_nochroot')
%end

%post
$SNIPPET('log_ks_post')
# Start yum configuration
$yum_config_stanza
# End yum configuration
$SNIPPET('post_install_kernel_options')
$SNIPPET('post_install_network_config')
$SNIPPET('func_register_if_enabled')
$SNIPPET('puppet_register_if_enabled')
$SNIPPET('download_config_files')
$SNIPPET('koan_environment')
$SNIPPET('redhat_register')
$SNIPPET('cobbler_register')
# Enable post-install boot notification
$SNIPPET('post_anamon')
# Start final steps
$SNIPPET('kickstart_done')
# End final steps
%end
EOF
</pre>

This is the path to  default kickstart
>  /var/lib/cobbler/kickstarts/

##14.Create cobbler profile
<pre>
cobbler profile edit --name=CentOS7.5-x86_64 --kickstart=/var/lib/cobbler/kickstarts/CentOS7.5-x86_64.ks
</pre>

##15.Modify Network device name to eth0
    cobbler profile edit --name=CentOS7.5-x86_64 --kopts='net.ifnames=0 biosdevname=0'
    
####Don't forget cobbler sync
    cobbler sync

##16.Check your config
<pre>
cobbler profile report CentOS7.5-x86_64
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

END