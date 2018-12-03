# Cat常用命令

## 查看文件任意几行的数据
|命令|解释|
|---|---|
|grep -C 5 "^http"| ^符合文件开头为http上下5行内容|



<pre>
# cat /etc/nginx/nginx.conf | grep -C 5 "^http"  
----------
    
    events {
        worker_connections 1024;
    }
    
    http {
        log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent" "$http_x_forwarded_for"';
    
        access_log  /var/log/nginx/access.log  main;
</pre>

|命令|解释|
|---|---|
|grep -B 5 "^http"| ^符合文件开头为http上5行内容|

<pre>
cat /etc/nginx/nginx.conf | grep -B 5 "^http" 
----------
    
    events {
        worker_connections 1024;
    }
    
    http {
    


</pre>

|命令|解释|
|---|---|
|grep -A 5 "^http"| ^符合文件开头为http下5行内容|

<pre>
# cat /etc/nginx/nginx.conf | grep -A 5 "^http"
----------     
    http {
        log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent" "$http_x_forwarded_for"';
    
        access_log  /var/log/nginx/access.log  main;
</pre>

## END