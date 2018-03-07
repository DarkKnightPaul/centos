# centos知识点 
- [配置网络](#配置网络)  
- [防火墙](#防火墙)  
- [创建用户](#创建用户)  
- [ssh登陆](#ssh登陆)  
- [简单的vi指令](#简单的vi指令)  
- [安装jdk](#安装jdk)  
- [安装tmux](#安装tmux)  
- [安装mysql](#安装mysql)  
- [安装git](#安装git)  
- [安装nginx](#安装nginx)  
- [安装tomcat](#安装tomcat)  

## 配置网络   
- 执行`nmcli connection show`查看网络列表
- 执行`nmcli connection modify ens33 connection.autoconnect yes ipv4.method manual ipv4.address 192.168.17.100 ipv4.gateway 192.168.17.1` 修改网卡配置
- 执行`systemctl restart network`重启网络服务
- 执行`nmcli connection show ens33`查看修改后的网卡信息
- [返回目录](#centos知识点)

## 防火墙   
- 执行`systemctl status firewalld.service`查看防火墙状态  
- 执行`firewall-cmd --list-all`查看防火墙信息  
- 执行`firewall-cmd --permanent --zone=public --add-port=3306/tcp`添加端口配置  
- 执行`firewall-cmd --permanent --zone=public --remove-port=3306/tcp`移除端口配置  
- 执行`systemctl restart firewalld.service`重启防火墙  
- [返回目录](#centos知识点)  

## 创建用户  
- 执行`adduser huhuiyu`创建用户huhuiyu  
- 执行`passwd huhuiyu`修改用户huhuiyu的密码  
- 执行`su -`可以切换到root用户  
- 执行`su - huhuiyu`可以切换到huhuiyu用户  
- [返回目录](#centos知识点)  

## ssh登陆  
- 使用putty的ssh工具生成公钥和私钥
- 在服务器中创建.ssh目录
- 执行`chmod 700 ~/.ssh`设置权限
- 创建~/.ssh/authorized_keys文件，将公钥复制到文件中
- 执行`chmod 600 ~/.ssh/authorized_keys`设置权限
- 配置putty使用私钥登陆
- 执行`vi /etc/ssh/sshd_config`，配置PasswordAuthentication no可以关闭密码登陆,执行`systemctl restart sshd.service`重启ssh服务生效
- [返回目录](#centos知识点)

## 简单的vi指令 
- `i`切换到输入模式,`esc`退出
- `:wq`保存并退出,`:q!`退出不保存修改
- [返回目录](#centos知识点)

## 安装tmux  
- 执行`yum install tmux`安装
- 执行`tmux` 开启一个tmux session
- 执行`tumx a` 恢复到上次启动的tmux session
- 执行`Ctrl+b "` 横向分割窗口 
- 执行`Ctrl+b %` 竖向分割窗口
- 执行`Ctrl+b 方向键` 移动到指定方向的窗口
- [返回目录](#centos知识点)

## 安装jdk  
- 执行`yum search java|grep jdk`查找可用的jdk版本
- 执行`yum install java-1.8.0-openjdk-devel.x86_64`安装openjdk1.8
- 执行`javac -version`测试jdk是否安装成功
- [返回目录](#centos知识点)

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
- [返回目录](#centos知识点)

## 安装git  
- 执行`yum install git`安装git
- 执行`git --version`测试git是否安装成功
- 强制替换本地版本为远程
  `git fetch --all`  
  `git reset --hard origin/master`  
  `git pull`  
- 执行`git config --global credential.helper store`可以本地存储git账号密码
- [返回目录](#centos知识点)

## 安装nginx  
- 执行`vi /etc/yum.repos.d/nginx.repo`编辑nginx下载源，内容如下  
  [nginx]  
  name=nginx repo  
  baseurl=http://nginx.org/packages/centos/7/$basearch/  
  gpgcheck=0  
  enabled=1  
- 执行`yum install nginx`安装nginx
- 执行`systemctl enable nginx`配置nginx服务开机启动
- 执行`systemctl start nginx`启动nginx
- 配置文件默认位置  
  /etc/nginx/nginx.conf  
  /etc/nginx/conf.d/*.conf  
- 通过浏览器打开`http://服务器地址`测试nginx是否搭建成功
- [返回目录](#centos知识点)

## 安装tomcat  
- 必须条件[安装jdk](#安装jdk)
- 通过网址`https://tomcat.apache.org/`找到合适的tomcat版本下载
- 执行`wget http://mirrors.shu.edu.cn/apache/tomcat/tomcat-8/v8.5.24/bin/apache-tomcat-8.5.24.tar.gz`下载tomcat8
- 执行`tar -zxvf apache-tomcat-8.5.24.tar.gz`解压tomcat8
- 执行`cd apache-tomcat-8.5.24/bin/`进入tomcat8执行文件目录
- 执行`./startup.sh`启动tomcat
- 执行`./shutdown.sh`停止tomcat
- 通过浏览器打开`http://服务器地址:8080`测试tomcat是否搭建成功
- [返回目录](#centos知识点)

## 安装nodejs  
- 执行`curl --silent --location https://rpm.nodesource.com/setup_9.x | sudo bash -`下载安装源
- 执行`yum -y install nodejs`安装nodejs
- 执行`node -v`测试nodejs安装是否成功
- 执行`npm -v`测试npm安装是否成功
- 执行`npm install -g cnpm --registry=https://registry.npm.taobao.org`安装cnpm并指定下载源为淘宝国内镜像  
- 执行`cnpm -v`测试cnpm安装是否成功
- [返回目录](#centos知识点)