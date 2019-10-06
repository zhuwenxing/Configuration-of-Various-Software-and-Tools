# 教育网利用DigitalOcean搭建IPv6—Shadowscoks实现免流量&科学上网

## IPv6连接测试
通过网站[IPv6测试](https://test-ipv6.com)检查网络是否可以连接IPv6。可以搜索关键字--**如何查看自己的网络是否支持IPV6**

## 申请 GitHub Student Developer Pack
参考：[如何申请github student pack？](https://www.zhihu.com/question/30453252)

## 注册DigitalOcean
使用GitHub提供的DigitalOcean学生优惠，参考[从领取Github教育礼包到DigitalOcean购买服务器](https://www.jianshu.com/p/c5e7721d886c)
***注意兑换优惠码后，暂时不使用教程中的方法购买服务器***

## 购买和配置主机
Create --> Droplets

注意点：

* 个人偏向选择Ubuntu 18.04 (在这个版本中内置了谷歌的BBR算法)
* Select additional options 记得勾选IPv6
* Authentication 勾选SSH keys（可以免除账号密码登录的麻烦）: 传统的教程都是使用putty和ssh - keygen命令，但是我个人偏好是VS Code + ssh remote + Xshell。Xshell用于生成公钥-私钥对，VS Code + SSH Remote用于远程连接服务器。

## 配置主机服务端

该部分的教程主要参考：[教育网利用DigitalOcean搭建IPv6—Shadowscoks实现免流量&科学上网](https://note.junyangz.com/2015/digitalocean/)
* 部署环境
```shell
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh

chmod +x shadowsocks.sh

./shadowsocks.sh 2>&1 | tee shadowsocks.log
```
根据提示输入参数，大概示意如下
```shell
    Congratulations, shadowsocks install completed!
    Your Server IP:your_server_ip
    Your Server Port:8989
    Your Password:your_password
    Your Local IP:127.0.0.1
    Your Local Port:1080
    Your Encryption Method:aes-256-cfb
    Welcome to visit:http://teddysun.com/342.html
    Enjoy it!
```
* 修改配置，以适合IPv6环境
```shell
vi /etc/shadowsocks.json
```
主要是修改"server"的值
```shell
{
    "server":"::", # 之前的值为"0.0.0.0"，下面的值不用修改
    "server_port":8989,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"yourpassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false  
}
```
重启SS让配置生效
```shell
/etc/init.d/shadowsocks restart
```

