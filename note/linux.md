一  nginx
===
/usr/local/nginx/sbin/nginx nginx  启动
/usr/local/nginx/sbin/nginx -s reload 重新加载配置文件




二 rabbitMq
===
```text
1）. 服务器启动与关闭
启动: rabbitmq-server –detached
关闭:rabbitmqctl stop
若单机有多个实例，则在rabbitmqctlh后加–n 指定名称

2）. 插件管理
开启某个插件：rabbitmq-pluginsenable xxx
关闭某个插件：rabbitmq-pluginsdisablexxx
注意：重启服务器后生效。

3）.virtual_host管理
新建virtual_host: rabbitmqctladd_vhost xxx
撤销virtual_host:rabbitmqctl delete_vhost xxx

4）. 用户管理
新建用户：rabbitmqctl add_user xxxpwd
删除用户: rabbitmqctl delete_user xxx
改密码: rabbimqctlchange_password {username} {newpassword}
设置用户角色：rabbitmqctlset_user_tags {username} {tag ...}
Tag可以为 administrator,monitoring, management

5）. 权限管理
权限设置：set_permissions [-pvhostpath] {user} {conf} {write} {read}
Vhostpath
Vhost路径
user
用户名
Conf
一个正则表达式match哪些配置资源能够被该用户访问。
Write
一个正则表达式match哪些配置资源能够被该用户读。
Read
一个正则表达式match哪些配置资源能够被该用户访问。

6）. 获取服务器状态信息
服务器状态：rabbitmqctl status
队列信息：rabbitmqctl list_queues[-p vhostpath] [queueinfoitem ...]
Queueinfoitem可以为：name，durable，auto_delete，arguments，messages_ready，messages_unacknowledged，messages，consumers，memory
Exchange信息：rabbitmqctllist_exchanges[-p vhostpath] [exchangeinfoitem ...]
Exchangeinfoitem有：name，type，durable，auto_delete，internal，arguments.
Binding信息：rabbitmqctllist_bindings[-p vhostpath] [bindinginfoitem ...]
Bindinginfoitem有：source_name，source_kind，destination_name，destination_kind，routing_key，arguments
Connection信息：rabbitmqctllist_connections [connectioninfoitem ...]
Connectioninfoitem有：recv_oct，recv_cnt，send_oct，send_cnt，send_pend等。
Channel信息：rabbitmqctl list_channels[channelinfoitem ...]
Channelinfoitem有consumer_count，messages_unacknowledged，messages_uncommitted，acks_uncommitted，messages_unconfirmed，prefetch_count，client_flow_blocked

rabbimq-plugins
http://www.rabbitmq.com/man/rabbitmq-plugins.1.man.html

系统命令
卸载

#rpm -qa|grep rabbitmq

rabbitmq-server-3.6.1-1.noarch

#rpm -e --nodeps rabbitmq-server-3.6.1-1.noarch

#rpm -qa|grep erlang

esl-erlang-18.3-1.x86_64

#rpm -e --nodeps esl-erlang-18.3-1.x86_64

 服务

#service rabbitmq-server start --后台方式运行

#service rabbitmq-server stop  --停止运行

#service rabbitmq-server status --查看状态

插件安装
 

进入插件安装目录{rabbitmq-server}/plugins/（可以查看一下当前已存在的插件）

cd /usr/lib/rabbitmq/lib/rabbitmq_server-3.6.2/plugins

下载需要的插件（插件下载页面http://www.rabbitmq.com/community-plugins.html）

如下载插件rabbitmq_delayed_message_exchange

wget https://bintray.com/rabbitmq/community-plugins/download_file?file_path=rabbitmq_delayed_message_exchange-0.0.1.ez

（如果下载的文件名称不规则就手动重命名一下如：rabbitmq_delayed_message_exchange-0.0.1.ez）

启用插件

rabbitmq-plugins enable rabbitmq_delayed_message_exchange
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