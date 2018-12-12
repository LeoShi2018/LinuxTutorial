# Windows Agent
## 1. Download windows agnet
> If your system is x64 download

    https://www.zabbix.com/downloads/4.0.0/zabbix_agents-4.0.0-win-amd64.zip

## 2. Unzip

    c:\zabbix
    
## 3. Config file path

    c:\zabbix\conf\zabbix_agentd.win.conf
    
## 4. Edit config file

<pre>
62-<    # EnableRemoteCommands=0
62->    EnableRemoteCommands=1
86-<    Server=127.0.0.1
86->    Server=10.1.0.171  #Zabbix Server IP address
127-<   ServerActive=127.0.0.1
127->   ServerActive=10.1.0.171 #Zabbix Server IP address
138-<   Hostname=Windows host
138->   Hostname=client  #Hostname must matching your serverhost
</pre>       

![WindowsAgent](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Zabbix/images/image009.png)

## 5. Use administrator cmd

![Windowsadmincmd](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Zabbix/images/image010.png)

## 6. Install Zabbix Agent Service

<pre>
> zabbix_agentd.exe -i -c c:\zabbix\conf\zabbix_agentd.win.conf
----------
    zabbix_agentd.exe [2460]: service [Zabbix Agent] installed successfully
    zabbix_agentd.exe [2460]: event source [Zabbix Agent] installed successfully
</pre>

![InstallService](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Zabbix/images/image011.png)


## 7. Start Zabbix Agent Service

<pre>
> zabbix_agentd.exe -s -c c:\zabbix\conf\zabbix_agentd.win.conf
----------
    zabbix_agentd.exe [2380]: service [Zabbix Agent] started successfully
</pre>

![ServiceStart](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Zabbix/images/image012.png)

## 8. Agent log
<pre>
c:\zabbix\zabbix_agentd.log
----------
    740:20181212:144130.622 Zabbix Agent stopped. Zabbix 4.0.0 (revision 85308).
    260:20181212:144801.972 Starting Zabbix Agent [client]. Zabbix 4.0.0 (revision 85308).
    260:20181212:144801.972 **** Enabled features ****
    260:20181212:144801.972 IPv6 support:          YES
    260:20181212:144801.972 TLS support:            NO
    260:20181212:144801.972 **************************
    260:20181212:144801.972 using configuration file: c:\zabbix\conf\zabbix_agentd.win.conf
    260:20181212:144801.972 agent #0 started [main process]
    1356:20181212:144801.972 agent #2 started [listener #1]
    2664:20181212:144801.972 agent #1 started [collector]
    2128:20181212:144801.987 agent #4 started [listener #3]
    3344:20181212:144801.987 agent #3 started [listener #2]
    2452:20181212:144801.987 agent #5 started [active checks #1]
</pre>

## 9. Stop ZabbixAgent

<pre>
> zabbix_agentd.exe -x
----------
    zabbix_agentd.exe [1516]: service [Zabbix Agent] stopped successfully
</pre>

## 10. Uninstall ZabbixAgent

<pre>
> zabbix_agentd.exe -d
----------
    zabbix_agentd.exe [292]: service [Zabbix Agent] uninstalled successfully
    zabbix_agentd.exe [292]: event source [Zabbix Agent] uninstalled successfully
</pre>

## END