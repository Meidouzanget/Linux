# Linux

### 安装zabbix agent

agent有版本2，相比原来的C语言编写，2使用GO语言，支持更好

官网的教程用的是1，我们可以在命令行中加入2，如下

    su -

    apt update && apt -y upgrade

    wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4%2Bdebian11_all.deb

    dpkg -i zabbix-release_6.0-4+debian11_all.deb

    apt update

    apt install zabbix-server-mysql zabbix-frontend-php zabbix-nginx-conf zabbix-sql-scripts zabbix-agent2


-------------------------------------------------------------------------------------------------------------------------

### 安装mariadb(mysql)

    wget https://dev.mysql.com/get/mysql-apt-config_0.8.22-1_all.deb
    apt install ./mysql-apt-config_0.8.22-1_all.deb
    sudo dpkg-reconfigure mysql-apt-config
    sudo apt update
    sudo apt install mariadb-server
    systemctl enable --now mysql
    systemctl status mysql
    
    mysql -u root -p
    输入密码直接回车

    重置密码
    ALTER USER 'root'@'localhost' IDENTIFIED BY 'XXXXXXXX';
    
    刷新权限表，退出
    flush privileges;
    \q
    
#### 更改配置文件

    nano /etc/zabbix/zabbix_server.conf    
    
    在文档中找到一下字段并更改
    DBHost=localhost
    DBName=zabbix_db
    DBUser=zabbix_user
    DBPassword=XXXXXXXX
------------------------------------------------------------------------------------------------------------------------

### 为zabbix创建数据库

虽然官方有默认的数据库初始表可以导入，但是并不会自动创建DB，所以需要我们先建库，在zcat导入

    重新进入DB
    mysql -uroot -p
    
    create database zabbix_db character set utf8 collate utf8_bin;
    create user zabbix_user@localhost identified by 'zabbix@123';
    GRANT CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'zabbix_user'@'localhost' WITH GRANT OPTION;
    grant all privileges on zabbix_db.* to zabbix_user@localhost;
    FLUSH PRIVILEGES;
    
    退出DB
    \q
    

    grant all privileges on zabbix_db.* to zabbix@localhost identified by 'zabbix@123';
    

    

### 为zabbix数据库导入初始数据

这个包在安装agent的时候会自动下载，如果报错找不到文件，可能是mariadb安装的时候出了问题
可以尝试参照网址重做第三步
    https://www.linuxmi.com/debian-11-10-anzhuang-zabbix.html
    
    zcat /usr/share/doc/zabbix-sql-scripts/mysql/server.sql.gz | mysql -uroot -p zabbix_db

### 修改数据库安全设置 && 时区



    mysql_secure_installation


设置确定删除匿名用户、远程禁用 root 登录、删除测试数据库及其访问权限以及应用所有更改的操作

如果实在局域网环境，root远程登录不建议禁用

    Switch to unix_socket authentication? Y

    Change the root passwword？ N	(前面已经修改过了)

    Remove anonymous users? Y	(移除匿名用户)

    Disallow root login remotely N	(禁用远程登陆)

    Reload privilege tables now？ Y




    打开文档
    nano /etc/zabbix/apache.conf
    
    修改时区
    php_value date.timezone Asia/Shanghai

    
![image](https://user-images.githubusercontent.com/59044398/216231991-72316d99-f08a-4ba1-bd9f-6622b9cae04f.png)

### 重启服务应用更改    

    systemctl restart apache2
    
    启动zabbix服务
    systemctl start zabbix-server zabbix-agent2
    systemctl enable zabbix-server zabbix-agent2
    
    
    
访问网址打开zabbix进行初始配置

    http://server_ip/zabbix





参考

https://www.linuxmi.com/debian-11-10-anzhuang-zabbix.html

https://www.zabbix.com/cn/download?zabbix=6.0&os_distribution=debian&os_version=11&components=server_frontend_agent&db=mysql&ws=nginx


https://www.ywsj.cf/archives/ubuntu2004-an-zhuang-zabbix60lts-jian-kong-fu-wu

https://www.youtube.com/watch?v=kOmojaUEAPo

https://technologyrss.com/how-to-install-zabbix-server-6-0-on-debian-11/




