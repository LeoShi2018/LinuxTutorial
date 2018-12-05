# Sed

## 1. 匹配字符上一行、下一行添加字符

> sed -i '/匹配内容/i#上一行添加' 文件

<pre>
# cat a.txt 
----------
    1
    2
    3
    4
    5
    6
    7
    8
    9
</pre>

<pre>
# sed -i '/2/iUp' a.txt 
# cat a.txt
----------          
    1
    Up
    2
    3
    ......
</pre>

> sed -i '/匹配内容/a#下一行添加' 文件

<pre>
# sed -i '/3/aDown' a.txt       
# cat a.txt 
---------
    1
    Up
    2
    3
    Down
    4
    5
    ......
</pre>

为了便于识别 

> sed -i '/2/iUp' a.txt 等同于 sed -i '/2/i\Up' a.txt

> sed -i '/3/aDown' a.txt 等同于 sed -i '/3/a\Down' a.txt

## END