# Charset
> 数据库中出现乱码

## 1. Show charset

<pre>
mysql> \s
    --------------
    mysql  Ver 14.14 Distrib 5.6.42, for Linux (x86_64) using  EditLine wrapper
    
    Connection id:          542
    Current database:
    Current user:           root@localhost
    SSL:                    Not in use
    Current pager:          stdout
    Using outfile:          ''
    Using delimiter:        ;
    Server version:         5.6.42 MySQL Community Server (GPL)
    Protocol version:       10
    Connection:             Localhost via UNIX socket
    Server characterset:    latin1
    Db     characterset:    latin1
    Client characterset:    utf8
    Conn.  characterset:    utf8
    UNIX socket:            /var/lib/mysql/mysql.sock
    Uptime:                 21 hours 32 min 29 sec
    
    Threads: 2  Questions: 4560  Slow queries: 0  Opens: 45  Flush tables: 1  Open tables: 38  Queries per second avg: 0.058
    --------------

mysql> show variables like '%char%';
    +--------------------------+----------------------------+
    | Variable_name            | Value                      |
    +--------------------------+----------------------------+
    | character_set_client     | utf8                       |
    | character_set_connection | utf8                       |
    | character_set_database   | latin1                     |
    | character_set_filesystem | binary                     |
    | character_set_results    | utf8                       |
    | character_set_server     | latin1                     |
    | character_set_system     | utf8                       |
    | character_sets_dir       | /usr/share/mysql/charsets/ |
    +--------------------------+----------------------------+
    8 rows in set (0.00 sec)
</pre>

## 2. Change charset latin1 to utf8

<pre>
sed -i '/^\[mysqld\]/a\character-set-server=utf8' /etc/my.cnf
cat <<'EOF' >> /etc/my.cnf
[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
EOF
systemctl restart mysqld
</pre>

## END