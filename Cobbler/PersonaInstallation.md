# Personalized installation

## 1. Custom network profile to install system

> You must know the server mac address

<pre>
IP:10.1.0.110
Hostname:linux-node1.leo.shi
Netmask:255.255.255.0
Gateway:10.1.0.2
DNS:10.1.0.2
</pre>


<pre>
cobbler system add --name=linux-node1.leo.shi \
--mac=00:0c:29:ca:cd:b4 --profile=Centos7.5-x86_64 \
--ip-address=10.1.0.110 --subnet=255.255.255.0 \
--gateway=10.1.0.2 --interface=eth0 --static=1 \
--hostname=linux-node1.jikai.com --name-servers="10.1.0.2" \
--kickstart=/var/lib/cobbler/kickstarts/CentOS7.5-x86_64.ks \
--kopts='net.ifnames=0 biosdevname=0'
</pre>

    cobbler system list
    ----------
    linux-node1.leo.shi

![Boot Menu](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Cobbler/images/images011.png)

![Boot Menu](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Cobbler/images/images012.png)


## END