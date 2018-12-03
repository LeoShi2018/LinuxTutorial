# Error: rpmdb open failed

<pre>
# yum install -y lrzsz
----------
    error: rpmdb: BDB0113 Thread/process 3790/140203213080384 failed: BDB1507 Thread died in Berkeley DB library
    error: db5 error(-30973) from dbenv->failchk: BDB0087 DB_RUNRECOVERY: Fatal error, run database recovery
    error: cannot open Packages index using db5 -  (-30973)
    error: cannot open Packages database in /var/lib/rpm
    CRITICAL:yum.main:
    
    Error: rpmdb open failed

# yum clean all
----------
    error: rpmdb: BDB0113 Thread/process 3790/140203213080384 failed: BDB1507 Thread died in Berkeley DB library
    error: db5 error(-30973) from dbenv->failchk: BDB0087 DB_RUNRECOVERY: Fatal error, run database recovery
    error: cannot open Packages index using db5 -  (-30973)
    error: cannot open Packages database in /var/lib/rpm
    CRITICAL:yum.main:
    
    Error: rpmdb open failed
</pre>

## Solution

<pre>
# cd /var/lib/rpm
# ll
----------
    total 101968
    -rw-r--r--. 1 root root  3047424 Aug  8 13:39 Basenames
    -rw-r--r--. 1 root root    20480 Aug  8 13:38 Conflictname
    -rw-r--r--. 1 root root   270336 Dec  3 09:10 __db.001
    -rw-r--r--. 1 root root    81920 Dec  3 09:10 __db.002
    -rw-r--r--. 1 root root  1318912 Dec  3 09:10 __db.003
    -rw-r--r--. 1 root root  1355776 Aug  8 13:39 Dirnames
    -rw-r--r--. 1 root root    16384 Aug  8 13:39 Group
    -rw-r--r--. 1 root root    12288 Aug  8 13:39 Installtid
    -rw-r--r--. 1 root root    24576 Aug  8 13:39 Name
    -rw-r--r--. 1 root root    16384 Jul 13 16:24 Obsoletename
    -rw-r--r--. 1 root root 95866880 Aug  8 13:39 Packages
    -rw-r--r--. 1 root root  2277376 Aug  8 13:39 Providename
    -rw-r--r--. 1 root root   151552 Aug  8 13:39 Requirename
    -rw-r--r--. 1 root root    49152 Aug  8 13:39 Sha1header
    -rw-r--r--. 1 root root    40960 Aug  8 13:39 Sigmd5
    -rw-r--r--. 1 root root     8192 Jul 13 16:19 Triggername
# rm __db.* -rf
# rpm --rebuilddb
# ll
----------
    total 76380
    -rw-r--r--. 1 root root  2613248 Dec  3 09:18 Basenames
    -rw-r--r--. 1 root root    20480 Dec  3 09:18 Conflictname
    -rw-r--r--. 1 root root  1318912 Dec  3 09:18 Dirnames
    -rw-r--r--. 1 root root     8192 Dec  3 09:18 Group
    -rw-r--r--. 1 root root    12288 Dec  3 09:18 Installtid
    -rw-r--r--. 1 root root    24576 Dec  3 09:18 Name
    -rw-r--r--. 1 root root    16384 Dec  3 09:18 Obsoletename
    -rw-r--r--. 1 root root 70184960 Dec  3 09:18 Packages
    -rw-r--r--. 1 root root  2179072 Dec  3 09:18 Providename
    -rw-r--r--. 1 root root   155648 Dec  3 09:18 Requirename
    -rw-r--r--. 1 root root    40960 Dec  3 09:18 Sha1header
    -rw-r--r--. 1 root root    32768 Dec  3 09:18 Sigmd5
    -rw-r--r--. 1 root root     8192 Dec  3 09:18 Triggername
# yum clean all
----------
    Loaded plugins: fastestmirror
    Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
    Cleaning repos: base elrepo epel extras google-cloud-compute google-cloud-sdk mongodb-org-3.2 updates
    Cleaning up everything
    Maybe you want: rm -rf /var/cache/yum, to also free up space taken by orphaned data from disabled or removed repos
    Cleaning up list of fastest mirrors
</pre>

# END