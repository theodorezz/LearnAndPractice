## 序

这是我的第一篇技术博文，目的在于总结归纳自己所学，以求模块化、系统化地将知识留存在大脑，也可与好学之人一同交流进步。
 
测试工程师的技能版图，已从手工测试转向要求左移的代码能力，右移的运维能力，上要求产品思维，下要求广阔知识面，Shell脚本的掌握理所当然成为了必备技能。 

求职寒冬从2019年延续到2020年，企业缩编的同时提高门槛，即使像玩笑话所说“面试造飞机”，我们所做的只有不断学习，提升自己。

本系列博文不是教程，亦不是大全，只是学习笔记。

	

> 	Unix哲学：一条命令做一件事。 
> 	Shell脚本：命令的组合。



![Shell思维导图](https://img-blog.csdnimg.cn/20200425130544346.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dpbmRzZWV5b3U=,size_16,color_FFFFFF,t_70)




一、Shell脚本
1.多个命令按序执行，以分号隔开
```bash
cd ;ls ;more
```

2.BASH
在Centos7中默认的Shell为BASH，可用echo $SHELL显示，本文以BASH作为Shell环境。

3.声明（sha-bang）

> * 若将脚本写入文件，需在第一行添加声明

```bash
#!/bin/bash
```
4.脚本创建、执行方式
```bash
vim filename.sh
chmod u+x filename.sh

bash ./filename.sh   #可以不赋权直接执行
./filename.sh   #需可执行权限
source ./filename.sh   #执行时对当前环境造成影响，不创建子进程，属于外部命令
. filename.sh
```


二、管道符与重定向
1.管道符
进程通信方式之一：“|”

2.重定向

 1. 输入重定向：`read var < /path/to/a/file`
 2. 输出重定向：`echo 123 > /path/to/a/file   #输出的文件会被先清空再存储`
 3. 错误重定向：`wrongcmd 123 > filename.txt   #将报错输出`
 4. 全部重定向：`allcmd & > file.txt`
 5. 组合重定向，并输出为文件
```bash
vim filename.sh

#!/bin/bash
cat > /path/to/a/filename.txt << EOF   (以EOF结尾）
echo “hello world”
EOF

Bash filename.sh
cat filename.txt（输出为echo “hello world”）
```


三、变量

> * 变量赋值左右不能有空格

1.将命令执行结果赋值给变量

```bash
cmd1=$(ls -l /etc)
cmd2=‘ls /root'
```
2.变量引用

```bash
${变量名}或$变量名
```

3.变量引用范围
> * 当前Shell，若想当前终端生效则执行source filename.sh
> * export filename 子进程得到父进程的值

4.环境变量

 1. PATH：命令搜索路径（在当前终端生效）
 2. 预定义变量：

```bash
 echo $?   #上一条命令是否执行成功
 echo $$   #显示当前Shell进程PID
 echo $0   #查看当前脚本文件名
 echo $1   #传递给当前Shell的第一个参数
```


四、测试与判断

> * test命令，或使用中括号[ ]

```bash

 1. string1 = string2   #有空格
 2. test -f /etc/password   #文件是否存在
 3. [-d /etc/]   #目录是否存在
 4. [ 5 -gt 4 ]   #大于比较

```


五、if语句
1.if-then

```bash
[root@localhost~]#if [ $UID = 0 ]
>then
>  echo “ root user”
>fi
    root user
```

2.if-then-else

```bash
#!/bin/bash
#if else demo

If [ $USER = root ] ;then
    echo " user root “
    echo $UID
else
    echo " other root “
    echo $UID
fi
```

3.if嵌套

```bash
#!/bin/bash
#if then if then fi fi demo


if [ $UID = 0] ; then
    echo “ please run”
    if [ -x /tmp/10.sh ] ; then   #若文件可执行则执行
            /tmp/10.sh
    fi
else
    echo "swtich user root”
fi
```


六、函数
1.自定义函数

```bash
[root@localhost~]function cdls() {

>cd /var
>ls
>}
[root@localhost]cdls
显示/var目录，ls命令的结果
定义时也可省略function字段
```

> *在脚本中定义函数检查PID

```bash
vim filename.sh
#!/bin/bash
#functions

checkpid() {
    
    local i   #声明i只在本函数有效

    for i in $* ; do   #$*表示所有向脚本传递的参数
        [ -d “/proc/$i” ] && return 0    #类似于if-then。创建进程时会在/proc/目录下创建文件
    done

    return i
执行脚本时需source ./filename.sh，否则会在子进程执行该脚本）
```

2.系统自建函数

```bash
在/etc/init.d/functions中
```


七、脚本资源控制

```bash
fork炸弹
.(){.|.&};.
```

八、正则表达式与文本搜索
1.元字符

```bash
grep pass. . . .$ /root/anaconda-ks.cfg
四个点代表四个字符，$表示后面无其他内容

grep pass.* /root/anaconda-ks.cfg
*表示任意字符

[ ]匹配方括号中任意字符

^表示以某字符串开头
grep ^# abc.txt（以#开头）

转义字符生效（匹配点）
grep "\.” asd.txt
```

2.find

```bash
 1. 查找文件
find /etc -name password

find /etc -name pass*

find /etc -regex .*wd  （任意字符，以wd结尾）
 
 2. 查找文件类型

find /etc -type f -name password

 3. 查找文件并删除
find *txt -exec rm -v {} \;
#   -exec删除后无提示
#   -v进度提示
#   \转义字符
```

九、sed替换命令

> * 将文件以行为单位读取到内存（模式空间）
> * 使用sed的每个脚本对该行进行操作
> * 处理完成后输出该行

```bash
sed 's/old/new' filename
sed -e 's/old/new' -e 's/old/new' filename ...
sed -i 's/old/new 's/old/new' filename ...
sed -r 's/new/new'   #filename支持扩展正则表达式
```

```bash
[root@localhost~]#echo a a a > aflie
[root@localhost~]#sed 's/a/aa/' afile
[root@localhost~]#aa a a

[root@localhost~]#sed 's!/!abc!' afile  #替换斜线时可用其他符号做分隔符

[root@localhost~]#sed -e 's/a/aa/' -e 's/aa/bb' file   #a替换为aa，aa替换为bb
[root@localhost~]#sed 's/a/aa/;s/aa/bb' afile  #与上面相同，简写

[root@localhost~]#cat file
[root@localhost~]#a a a
[root@localhost~]#sed -i 's/a/aa/;s/aa/bb/' afile   #替换写入原始文件
[root@localhost~]#cat afile
[root@localhost~]#bb a a

[root@localhost~]#sed ’s/a/aa/;s/aa/bb/‘ afile >bfile    #输出到其他文件

[root@localhost~]#grep root /etc/passwd | sed ’s/^root//‘   #路径下以root开头的一行替换为空
```


十、扩展元字符

```bash
[root@localhost~]#cat bfile
b
a
aa
aaa
ab
[root@localhost ~]# sed 's/ab*/!/' bfile  #替换有（0～n）个b的字符串
b
!
!a
!aa
!


[root@localhost ~]#sed 's/ab+/!/' bfile  #替换b至少出现1次的字符串
b
a
aa
aaa
!

（?为b出现1次或0次）
（管道符|表示a或b）
```

十一、sed全局替换与寻址
> *全局替换

```bash
sed 's/root/!!!!/g'  #全部匹配替换
sed 's/root/!!!!/2'  #替换第二个匹配
sed 's/root/!!!!/3'  #第三个

sed -n 's/root/!!!!/p' #只输出替换成功的行
```

> * 寻址

```bash
1.限制指定行数
sed '1s/root/!!!!/'   #第一行
sed '1,3s/root/!!!!/' #第一行到第三行
sed '1,$s/root/!!!!/' #第一行到最后一行

2.正则表达式
head -6 /etc/passwd | sed '/^bin/,$s/root/!!!!/g'
#以bin开头，到最后一行结尾
```

十二、sed高级用法
1.N追加到模式空间（换行替换）

```bash
[root@openstack ~]#cat a.txt
hel
lo
[root@openstack ~]#sed 'N;s/hel\nlo/!!!/' a.txt
!!!
[root@openstack ~]#sed 'N;s/hel.lo/!!!/‘ a.txt   #效果同上，用.匹配换行符
```
2.N P D

```bash
#非交互式创建多行文件
[root@openstack ~]#cat > b.txt << EOF
>hell
>o bash hel
>lo bash
>EOF
[root@openstack ~]#cat b.txt
hell
o bash hel
lo bash
[root@openstack ~]#sed ’N;s/\n//;s/hello bash/hello sed\n/;P;D’ b.txt
hello sed
 hello sed
#换行符替换为空，文本变为2行，替换字段，内部循环#
```

十三、awk

>* awk用于规范文本

> * awk的字段引用和分离（$0 表示一整行）

```bash
[root@jenkins ~]# awk '/^menu/{ print $0 }' /boot/grub2/grub.cfg
menuentry 'CentOS Linux (3.10.0-1062.el7.x86_64) 7 (Core)' --class centos --class gnu-linux --class gnu --class os --unrestricted $menuentry_id_option 'gnulinux-3.10.0-1062.el7.x86_64-advanced-3404fd86-604f-4b38-a032-01ed886aca55' {
menuentry 'CentOS Linux (0-rescue-3b2009beaa0743698c63592b58e49421) 7 (Core)' --class centos --class gnu-linux --class gnu --class os --unrestricted $menuentry_id_option 'gnulinux-0-rescue-3b2009beaa0743698c63592b58e49421-advanced-3404fd86-604f-4b38-a032-01ed886aca55' {
[root@jenkins ~]#
[root@jenkins ~]# awk -F "'" '/^menu/{ print $2 }'  /boot/grub2/grub.cfg
CentOS Linux (3.10.0-1062.el7.x86_64) 7 (Core)
CentOS Linux (0-rescue-3b2009beaa0743698c63592b58e49421) 7 (Core)
[root@jenkins ~]#
```

> * 系统变量

```bash
[root@jenkins ~]# head -5 /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
[root@jenkins ~]#

> 输出第一个字段：FS/BEGIN
[root@jenkins ~]# head -5 /etc/passwd | awk -F ":" '{ print $1 }'
[root@jenkins ~]# head -5 /etc/passwd | awk 'BEGIN{FS=":"}{ print $1 }'
root
bin
daemon
adm
lp
[root@jenkins ~]#head -5 /etc/passwd | awk 'BEGIN{FS=":"}{ print $1,$2 }' #增加字段

> RS分隔符换行
[root@jenkins ~]# head -5 /etc/passwd | awk 'BEGIN{RS=":"}{ print $0 }'
root
x
0
0
root
/root
/bin/bash
bin
x
1
1
bin
/bin
/sbin/nologin
daemon
x
2
2
daemon
/sbin
/sbin/nologin
adm
x
3
4
adm
/var/adm
/sbin/nologin
lp
x
4
7
lp
/var/spool/lpd
/sbin/nologin

> 字段数量NF
[root@jenkins ~]# head -5 /etc/passwd | awk 'BEGIN{FS=":"}{ print NF }'
7
7
7
7
7

> 输出最后一个字段内容
[root@jenkins ~]# head -5 /etc/passwd | awk 'BEGIN{FS=":"}{ print $NF }'
/bin/bash
/sbin/nologin
/sbin/nologin
/sbin/nologin
/sbin/nologin
```

