![Build Status](http://src.baerk.com/wp-content/uploads/2018/03/Cisco-AnyConnect1-300x146.png?imageView2/2/w/1400/interlace/1/q/100|watermark/1/image/aHR0cDovL3NyYy5iYWVyay5jb20vd3AtY29udGVudC91cGxvYWRzLzIwMTcvMDgvd2F0ZXIucG5n/dissolve/100/gravity/SouthEast/dx/5/dy/5#)

# Cisco 与 AnyConnect安装

## 系统环境
Centos 7.x [CentOS（Community Enterprise Operating System）是Linux发行版之一]
Ocserv版本：0.10.8(使用0.10.8版本的原因较稳定，其它版本也可以尝试安装)
### 安装说明
支持自动判断防火墙，请确保 Firewalld 或者 iptables 其中一个是 active 状态；
默认采用用户名密码验证，本安装脚本编译的 ocserv 也支持 pam 验证，只需要修改配置文件即可；
默认配置文件在 /usr/local/etc/ocserv/ 目录，可自行更改脚本里的参数；
安装时会提示你输入端口、用户名、密码等信息，也可直接回车采用默认值，密码是随机生成的；
安装脚本会关闭 SELINUX；
自带路由表，只有路由表里的 IP 才会走 VPN，如果你有需要添加的路由表可自行添加，最多支持 200 条；
如果你有证书机构颁发的证书，可以把证书放到脚本的同目录下，确保文件名和脚本里的匹配，安装脚本会使用你的证书，客户端连接时不会提示证书错误；
配置文件修改为每个账号允许 10 个连接，全局 1024 个连接，可修改脚本前面的变量。1024 个连接大约需要 2048 个 IP，所以虚拟接口的 IP 配置了 8 个 C 段。

# 开始安装 Ocserv
### 推荐（自用实时更新可用路由表）：

wget https://src.zt.tn/Script/ocserv-install-script-for-centos7.sh

### 备用：
wget https://raw.githubusercontent.com/travislee8964/Ocserv-install-script-for-CentOS-RHEL-7/master/ocserv-install-script-for-centos7.sh

### 可自由修改脚本中相关参数，例如：

将其中 ocserv_version=”0.10.8″ 这一行的版本号改成 你想要的版本，据说0.10.8此版本较稳定。
vi ocserv-install-script-for-centos7.sh
修改后保存，然后运行脚本。
sh ocserv-install-script-for-centos7.sh
安装过程中会提示你输入端口、用户名和密码等，自己按需填写。 安装完成后可编辑配置文件，添加用户或者删除用户等待操作。
### 配置目录：

/usr/local/etc/ocserv/ocserv.conf

相关参数命令：

#### 添加用户

ocpasswd -c /usr/local/etc/ocserv/ocpasswd username  (username是要添加的用户名)

#### 删除用户

编辑在/usr/local/etc/ocserv目录下名为“ocpasswd”的文件，可以看到
username:*:********************************
找到你要删除的用户名，删除那一行就ok.

* ## 特别说明：在修改路由或者相关参数配置文件时，需要重新启动服务才能生效。
为了实现全局路由，亲注释掉route。如#route = *.*.*.*/*.*.*.*(即安全路由为0.0.0.0/0)
* 启动：
systemctl start ocserv.service

* 停止：
systemctl stop ocserv.service

* 重启：
systemctl restart ocserv.service

* 状态：
systemctl status ocserv.service
