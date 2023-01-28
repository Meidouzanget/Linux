# Linux

su -

apt update && apt -y upgrade

wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4%2Bdebian11_all.deb

dpkg -i zabbix-release_6.0-4+debian11_all.deb

apt update

apt install zabbix-server-mysql zabbix-frontend-php zabbix-nginx-conf zabbix-sql-scripts zabbix-agent2


-------------------------------------------------------------------------------------------------------------------------

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
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root#MISAKA20001';
刷新权限表，退出
flush privileges;
\q

------------------------------------------------------------------------------------------------------------------------
重新进入
mysql -uroot -p


create database zabbix_db character set utf8 collate utf8_bin;
create user zabbix_user@localhost identified by 'zabbix@123';
GRANT CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'zabbix_user'@'localhost' WITH GRANT OPTION;
grant all privileges on zabbix_db.* to zabbix_user@localhost;
FLUSH PRIVILEGES;
\q


 grant all privileges on zabbix.* to zabbix@localhost identified by ‘’
grant all privileges on zabbix_db.* to zabbix@localhost identified by 'zabbix@123';







参考

https://www.linuxmi.com/debian-11-10-anzhuang-zabbix.html

https://www.zabbix.com/cn/download?zabbix=6.0&os_distribution=debian&os_version=11&components=server_frontend_agent&db=mysql&ws=nginx


https://www.ywsj.cf/archives/ubuntu2004-an-zhuang-zabbix60lts-jian-kong-fu-wu

https://www.youtube.com/watch?v=kOmojaUEAPo

https://technologyrss.com/how-to-install-zabbix-server-6-0-on-debian-11/




