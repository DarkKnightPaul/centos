# centos知识点 
- [配置网络](#配置网络)  
- [防火墙](#防火墙)  
- [创建用户](#创建用户)  
- [ssh登陆](#ssh登陆)  
- [简单的vi指令](#简单的vi指令)  
- [安装jdk](#安装jdk)  
- [安装tmux](#安装tmux)  
- [安装mysql](#安装mysql)  

## 配置网络   
- 执行`nmcli connection show`查看网络列表
- 执行`nmcli connection modify ens33 connection.autoconnect yes ipv4.method manual ipv4.address 192.168.17.100 ipv4.gateway 192.168.17.1` 修改网卡配置
- 执行`systemctl restart network`重启网络服务
- 执行`nmcli connection show ens33`查看修改后的网卡信息
- 也可以执行`vi /etc/sysconfig/network-scripts/ifcfg-ens33`来修改网络配置,虚拟机需要配置  
   `BROADCAST=192.168.17.255`,必须是255
   `GATEWAY=192.168.17.2`,必须是2
- [返回](#centos知识点)

## 防火墙   
- 执行`systemctl status firewalld.service`查看防火墙状态
- 执行`firewall-cmd --permanent --zone=public --add-port=3306/tcp`添加端口配置
- 执行`firewall-cmd --permanent --zone=public --remove-port=3306/tcp`移除端口配置
- 执行`systemctl restart firewalld.service`重启防火墙
- [返回](#centos知识点)

## 创建用户  
- 执行`adduser huhuiyu`创建用户huhuiyu
- 执行`passwd huhuiyu`修改用户huhuiyu的密码
- 执行`su`可以切换到root用户
- 执行`su huhuiyu`可以切换到huhuiyu用户
- [返回](#centos知识点)

## ssh登陆  
- 使用putty的ssh工具生成公钥和私钥
- 在服务器中创建.ssh目录
- 执行`chmod 700 ~/.ssh`设置权限
- 创建~/.ssh/authorized_keys文件，将公钥复制到文件中
- 执行`chmod 600 ~/.ssh/authorized_keys`设置权限
- 配置putty使用私钥登陆
- 执行`vi /etc/ssh/sshd_config`，配置PasswordAuthentication no可以关闭密码登陆,执行`systemctl restart sshd.service`重启ssh服务生效

## 简单的vi指令 
- `i`切换到输入模式,`esc`退出
- `:wq`保存并退出,`:q!`退出不保存修改
- [返回](#centos知识点)

## 安装tmux  
- 执行`yum install tmux`安装
- 执行`tmux` 开启一个tmux session
- 执行`tumx a` 恢复到上次启动的tmux session
- 执行`Ctrl+b "` 横向分割窗口 
- 执行`Ctrl+b %` 竖向分割窗口
- 执行`Ctrl+b 方向键` 移动到指定方向的窗口
- [返回](#centos知识点)

## 安装jdk  
- 执行`yum search java|grep jdk`查找可用的jdk版本
- 执行`yum install java-1.8.0-openjdk-devel.x86_64`安装openjdk1.8
- [返回](#centos知识点)

## 安装mysql  
- 通过网址`https://dev.mysql.com/downloads/repo/yum/`找到mysql的yum安装源
- 执行`wget https://repo.mysql.com//mysql57-community-release-el7-11.noarch.rpm`下载安装文件
- 执行`yum localinstall mysql57-community-release-el7-11.noarch.rpm`添加到本地安装源
- 执行`yum repolist enabled | grep "mysql.*-community.*"`查看本地安装源是否添加成功
- 执行`yum install mysql-community-server`安装mysql
- 执行`systemctl enable mysqld`配置mysql服务开机启动
- 执行`systemctl start mysqld`启动mysql
- 执行`grep 'temporary password' /var/log/mysqld.log`查看mysql的默认root密码
- 执行`mysql -uroot -p`启动命令行，密码就是上一步查看的
- 执行`ALTER USER 'root'@'localhost' IDENTIFIED BY 'MySQL-123';`修改root默认密码，否则不能执行其它管理命令
- 执行`CREATE USER 'huhuiyu'@'%' IDENTIFIED BY 'MySQL-123';`添加用户
- 执行`GRANT ALL ON *.* TO 'huhuiyu'@'%';`用户授权
- 执行`FLUSH PRIVILEGES;`用户权限立即生效
- [返回](#centos知识点)
