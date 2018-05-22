一  nginx
===
```text
/usr/local/nginx/sbin/nginx nginx  启动
/usr/local/nginx/sbin/nginx -s reload 重新加载配置文件
```


后台执行脚本
===
```text
后台执行  James 
nohup command > /opt/tools/james/james-2.3.2.1/bin/run.sh 2>&1 &
```

查看端口占用
===
```text
lsof -i:端口
```

shell
===
*** 查询 tomcat-dingding 所在的进程并且座位 kill-9  的参数，杀掉进程
```text
ps -ef|grep "tomcat-dingding"|grep -v grep |awk '{print $2}'|xargs kill -9
```
软链接
===
软链接
```text
ln -s xxx xxx(xxx to xxx)
-s 软链接  ，没有-s 硬链接
```
/bin,/sbin,/usr/sbin,/usr/bin 目录
```text
/bin是系统的一些指令。bin为binary的简写主要放置一些系统的必备执行档例如:
    cat、cp、chmod df、dmesg、gzip、kill、ls、mkdir、more、mount、rm、su、tar等。
/sbin一般是指超级用户指令。主要放置一些系统管理的必备程式例如:
    cfdisk、dhcpcd、dump、e2fsck、fdisk、halt、ifconfig、ifup、 ifdown、init、insmod、lilo、
    lsmod、mke2fs、modprobe、quotacheck、reboot、rmmod、 runlevel、shutdown等。
/usr/bin　是你在后期安装的一些软件的运行脚本。主要放置一些应用软体工具的必备执行档
    例如c++、g++、gcc、chdrv、diff、dig、du、eject、elm、free、gnome*、 gzip、htpasswd、kfm、ktop、
    last、less、locale、m4、make、man、mcopy、ncftp、 newaliases、nslookup passwd、quota、smb*、wget等。
/usr/sbin   放置一些用户安装的系统管理的必备程式
    例如:dhcpd、httpd、imap、in.*d、inetd、lpd、named、netconfig、nmbd、samba、sendmail、squid、swap、tcpd、tcpdump等。
如果新装的系统，运行一些很正常的诸如：shutdown，fdisk的命令时，悍然提示：bash:command not found。那么
首先就要考虑root 的$PATH里是否已经包含了这些环境变量。
可以查看PATH，如果是：PATH=$PATH:$HOME/bin则需要添加成如下：
PATH=$PATH:$HOME/bin:/sbin:/usr/bin:/usr/sbin
```