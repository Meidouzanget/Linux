# Linux

```
安装mariadb
sudo zypper install MariaDB-server MariaDB-client

#启动mariadb服务
systemctl start mariadb;
  
#查看mariadb服务状态
systemctl status mariadb;
 
#关闭mariadb服务
systemctl stop mariadb;
 
#重启mariadb服务
systemctl restart mariadb
```

###### openSUSE系统已预装了MariaDB ，首先我们要更新一下

```
执行所系统所有软件更新
zypper dist-upgrade

单独执行更新
zypper update mariadb
```



#### 设置远程连接

##### 1.修改MaraDB 配置文件my.cnf
```
   cd /etc
   nano my.conf
   把 bind-address = 127.0.0.1这一行注释掉
```

##### 2. 授权 Mariadb 连接用户的远程访问权限。您可以使用以下命令创建一个新用户，并授权该用户从任何位置连接到 Mariadb 服务器：
```
   mysql -u root -p
   #输入管理员密码后进入 Mariadb 命令行
   CREATE USER 'username'@'%' IDENTIFIED BY 'password';
   GRANT ALL PRIVILEGES ON *.* TO 'username'@'%' WITH GRANT OPTION;
   FLUSH PRIVILEGES;
   
   其中，username 和 password 是您要创建的新用户的用户名和密码。% 表示允许该用户从任何远程客户端连接到 Mariadb 服务器 
   
   重新启动 Mariadb 服务，使设置生效：
   sudo systemctl restart mariadb
 ```
##### 3.设置防火墙

SLE/openSUSE上的防火墙默认策略是`DROP`，如果用`iptables -F`清空规则的话，这个机器就不能连接了，
大多数从红帽过来的运维都得掉坑里╮(￣▽￣)╭

设置防火墙开启MariaDB 远程端口
```
    sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent
    sudo firewall-cmd --reload
```

#### 到这里配置已全部完成，去Navicat配置连接信息即可












