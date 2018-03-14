# Linux定时任务

定时任务需要使用到`crontab`服务，如果没有安装`crontab`服务，可以参考一下[Linux Crontab 安装使用详细说明](https://www.cnblogs.com/binblogs/p/5206789.html)
接下来就开始介绍`crontab`的使用方法

## 添加定时任务

添加定时任务可以使用以下两种方式

*   在命令行输入: crontab -e 然后添加相应的任务，wq存盘退出
*   直接编辑/etc/crontab 文件，即vi /etc/crontab，添加相应的任务

本人在学习的时候使用的第二种方式，那就主要记录第二种方式的使用,编辑`/etc/crontab`需要`root`权限。

```
[rohon@localhost ~]$ cat /etc/crontab 
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
HOME=/

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
  32  8  *  *  * rohon bash /home/rohon/server_run/test.sh
#  33 2  *  *  * root bash /root/server/test.sh
```

通过查看`crontab`文件中的内容，得知从左想右起：

*   第一列表示分钟
*   第二列表示小时
*   第三列表示日
*   第四列表示月
*   第五列表示星期几
*   第六列表示什么用户去执行任务
*   第六列是具体任务

笔者在自己的测试环境下编写了一个`test.sh`脚本文件
```
[rohon@localhost ~]$ cat server_run/test.sh 
#!/bin/bash
cd /home/rohon/server_run/
echo "`date +%Y%m%d_%H%M%S` success to execute test.sh file" >> test_run.log
```

上面的脚本内容很简单，主要是输出一句话到`test_run.sh`文件，不过为了让输出的文件在指定的目录下，也是几经波折，后经大神指定，通过`cd`命令可以指定当前的环境，第二点就是输出文件时，内容是追加到文件中，不是清空文件，重写内容。通过查资料得知:
*   echo "xxxx" > XXX #是覆盖内容
*   echo "xxxx" >> XXX #是追加内容

我们可以修改一下定时任务时间，查看一下是否有执行定时任务，并且有追加内容到`test_run.log`文件中。

```
[rohon@localhost ~]$ cat /etc/crontab 
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
HOME=/

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
  22  17  *  *  * rohon bash /home/rohon/server_run/test.sh

[rohon@localhost ~]$ cat server_run/test_run.log 
20180314_103201 success to execute test.sh file
20180314_172201 success to execute test.sh file
[rohon@localhost ~]$ 
```
