# centos知识点 
- [配置网络](#配置网络)  
- [创建用户](#创建用户)  

## 配置网络  
- 执行`nmcli connection show`查看网络列表
- 执行`nmcli connection modify ens33 connection.autoconnect yes ipv4.method manual ipv4.address 192.168.17.100 ipv4.gateway 192.168.17.1` 修改网卡配置
- 执行`systemctl restart network`重启网络服务
- 执行`nmcli connection show ens33`查看修改后的网卡信息

## 创建用户  
- 执行`adduser huhuiyu`创建用户huhuiyu
- 执行`passwd huhuiyu`修改用户huhuiyu的密码

