# VIM 快捷命令

| 命令 | 描述 |
| --- | :--- |
|D|删除当前字符右侧字符|
|gg| 文件首|

#### 可视块

例：删除前10行前4列

> "j"纵向选行 "l"横向选列

![vim](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Tools/images/image001.png)

 1.回到文档首

        gg


 2.开启可视块模式 
   
        WIN：ctrl+v MAC：control+v

![ctrl+v](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Tools/images/image002.png)

 3.输入行数"10"，紧接着输入"j"，纵向选择10行

![10,j](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Tools/images/image003.png)

 4.紧接着输入"3",再输入"l" ，当前光标右选3列

![3,l](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Tools/images/image004.png)

 5.最后输入"d"删除

![d](https://github.com/LeoShi2018/LinuxTutorial/blob/master/Tools/images/image005.png)


> 整个步骤需要连贯操作 gg -> ctrl+v -> 10 -> j -> 3 -> l -> d

## END

