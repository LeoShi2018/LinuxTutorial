# RHEL6 VM Template

> You can use this method for CentOS6.x OracleLinux6.x

### 1. Delete the ssh host files

<pre>
# rm -rf /etc/ssh/ssh_host_*
</pre>

### 2. Change hostname to "loaclhost.loacldomain"

<pre>
# sed -i '/^HOSTNAME/d' /etc/sysconfig/network
# sed -i '$a\HOSTNAME=loaclhost.loacldomain' /etc/sysconfig/network
</pre> 

### 3. Delete the network device special information

<pre>

# sed -i '/^UUID/d' /etc/sysconfig/network-scripts/ifcfg-eth0
# sed -i '/^HWADDR/d' /etc/sysconfig/network-scripts/ifcfg-eth0
</pre>

### 4. Clear log

<pre>
# cat /dev/null > /var/log/message
</pre>

### 5. Clear history
<pre>
# history -c
# cat /dev/null > /root/.bash_history
</pre>

### 6. Encapsulation system

<pre>
# /usr/sbin/sys-unconfig
----------
    Your VM is poweroff now. This system is your template.
</pre>


## END