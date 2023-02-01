# Linux

Navicate 无法远程连接连接 mariadb(mysql)

![image](https://user-images.githubusercontent.com/59044398/216087145-4352ff45-3e11-4a3d-8f8b-6730b50de513.png)


笔者用的是Debian 11 安装 zabbix

解决方法

### 进入目录，更改配置文件

    /etc/mysql/mariadb.conf.d

编辑 50-server.cnf 配置文件，把 bind-address 改为0.0.0.0，允许所有IP接入

    nano 50-server.cnf


![image](https://user-images.githubusercontent.com/59044398/216082153-9eaf74e3-7bc7-4567-b014-56bd415d5088.png)

### 开启远程访问权限


输入命令登录mysql

    mysql -u root -p
    
例子1：允许root使用123456从任何主机连接到mysql服务器；    

    GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
    
    刷新权限
    FLUSH PRIVILEGES;


例子2：允许用户abc从ip为10.10.50.127的主机连接到mysql服务器,并使用654321作为密码

    GRANT ALL PRIVILEGES ON *.* TO 'abc'@'10.10.50.127' IDENTIFIED BY '654321' WITH GRANT OPTION;
    
查看远程权限，可以看到添加了 root % 的一行

    select User,host from mysql.user;

![image](https://user-images.githubusercontent.com/59044398/216085472-7ac43168-f396-4edd-9675-c2aecdff10cd.png)



