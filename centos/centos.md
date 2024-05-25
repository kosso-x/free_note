# 入门

### 1. 不是 sudoers

```linux
   sudo

   # 输入密码

   vim /etc/sudoers

   # 在 root ALL 下增加 xxx ALL
```

### 2. 设置网络

  nmcli d 显示 ens33 unmanaged 状态
  vim /etc/NetworkManager/NetworkManager.conf
```linux
  [main]
  plugins=ifupdown,keyfile

  [ifupdown]
  managed=true

  [keyfile]
  unmanaged-devices=*,except:type:wifi,except:type:gsm,except:type:cdma,except:type:wwan,except:type:ethernet,except:type:vlan

  [device]
  wifi.scan-rand-mac-address=no
```
  修改文件 vim /etc/netplan/01-netcfg.yaml
```
  network:
    version: 2
    renderer: NetworkManager
```
  保存，重启
```
  dhclient [网卡名字]
  nmcli n
  nmcli on
```
  
```linux
   sudo vim /etc/sysconfig/network-scripts/ifcfg-ens33

   # 设置几个参数：

   BOOTPROTO=static 

   # 原来是 dhcp 改成 static

   ONBOOT=yes 

   # 修改网卡

   IPADDR=192.168.100.222 

   # ip地址(看配置)

   NETMASK=255.255.255.0 

   # 子网掩码(一般是这)

   GATEWAY=192.168.100.2 

   # 默认网关(一般是.2)

   DNS1=114.114.114.114 
   DNS2=8.8.4.4 

   # 找几个国内好用的(阿里：223.5.5.5和223. 6.6.6；百度：180.76.76.76)  

   service network restart  

   # 重启网络  

   # 保存退出  
```

### 3. 关闭防火墙

```linux
  # 查看防火墙状态
  systemctl status firewalld.service  

  # 临时关闭
  sudo systemctl stop firewalld.service  

  # 永久关闭
  sudo systemctl disable firewalld  

  # 永久打开
  sudo systemctl enable firewalld  
```

### 4. 换源

```linux
  # 备份之前的源
  mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup  

  # 下载阿里源配置
  wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo  

  # 生成缓存
  yum makecache  
```

### 5. ssh远程服务器

```linux
  ssh user@ip -p端口
  然后输入密码
```

### 6. sftp命令

```linux
  sftp user@ip -p端口
  # 上传 put 要上传的本地路径 服务器的路径
  # 下载 get 服务器文件路径 本地保存路径
```

### 7. 安装zsh

```linux
  sudo yum install git  
  yum -y install zsh  
  cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc  
  cd ~  
  git clone https://gitee.com/mirrors/oh-my-zsh.git  
  mv oh-my-zsh .oh-my-zsh  
```

### 8. 切换zsh和bash

```linux
  exec bash / zsh
  # 换完之后另一个 rc不能用了  

  chsh -s /bin/zsh username  or csh -s `echo $SHELL` username  
  # 切换默认的shell(非root只能改自己的)
```

### 9. zsh安装插件

```linux
  git clone 插件地址  

  git clone https://gitee.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/   zsh-autosuggestions  

  git clone https://gitee.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/  
  
  plugins/zsh-syntax-highlighting  

  # 下载的插件放到 /home/isme/.oh-my-zsh/custom/plugins/  目录  

  vim ~/.zshrc 73行 plugins = (a b c) # 使用插件: a b c是不同的插件名称
```

***bug: 解决zsh: no matches found: 问题的方法如下***
`在 .zshrc 文件中写入：setopt no_nomatch`

### 10. usermode 使用 # 慢慢看

### 11. 创建本地源以及.repo文件

```linux
  cd /etc/yum.repos.d
  # 所有的源的存放位置（先备份之前的源）  

  vim [文件名].repo
  # 新建源
  # 文件内容
  # 源的名字（不能和其他重复）
  [local]
  name=CentOS7.9-local  

  # 源文件的地址，可以使本地，可以使url
  baseurl=file:///run/media/root/CentOS\ 7\ x86_64  

  # 启动yum源： 1-启用 0-不启用  
  enabled=1

  # 安全检测：  1-开启 0-不开启  
  gpgcheck=0
  gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7  

  # 源的优先级
  priority=1

  # 保存退出
  wq!  
```

***附件中有配好的nginx源和centos7.9的本地源***

### 12. 查找指令

```linux
  locate
  updatedb
  # 查找之前必须先执行，更新文件夹的数据库，方便检索
  locate 文件名
```

### 13. 添加ssh公钥

```linux
  ssh-keygen -t rsa -C 'xxx@mai'
```

### 14. git设置名字

```linux
  git config --global user.name '史国强'
  git config --global user.email 'shiguoqiang@s-data.cn'
```

### 15. yarn 和 npm 更换国内源

```linux
  # 查看npm源的当前地址
  npm config get registry  

  # 设置淘宝镜像
  npm config set registry https://registry.npm.taobao.org  

  # 查看yarn源的当前地址
  yarn config get registry  

  # 设置淘宝镜像
  yarn config set registry https://registry.npm.taobao.org/
```

### 16. 升级nodejs到最新

```linux
  # 先清除npm缓存
  npm cache clean -f  

  # 安装n模块
  npm install -g n  

  # 升级node.js到最新稳定版
  n stable
```

### 17. 切换命令行启动 和 图形界面启动

```linux
  # 查看启动方式
  systemctl get-default  

  # 命令行启动 multi-user.target: analogous to runlevel 3
  systemctl set-default multi-user.target  

  # 图形界面启动
  systemctl set-default graphical.target
```

### 18. MySql 

```linux
  # ERROR 1698 错误：Access denied for user 'root'@'localhost'
  update mysql.user set authentication_string=PASSWORD('123456'),plugin="mysql_native_password" where user = 'root';
  # 允许所有机器远程登录
  GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
  # 允许特定IP 远程登录
  GRANT ALL PRIVILEGES ON *.* TO 'root'@'IP' IDENTIFIED BY '123456' WITH GRANT OPTION;
  # 没开放端口，不允许其他机器访问
  vim /etc/mysql/mariadb.conf.d/50-server.cnf
  bind-address 127.0.0.1 => bind-address 0.0.0.0
  重启
  # 防火墙开放
  iptables -A INPUT -p tcp --dport 3306 -j ACCEPT
```
### 19. nmcli 使用手册
```linux
  bug: Connection 'ens33' is not available on device ens33 because device is strictly unmanaged
  # 临时方案 
    dhclient  ens33
  # 查看托管状态
    nmcli n
  # 显示 disabled 则为本文遇到的问题，如果是 enabled 则可以不用往下看了
  # 开启 托管
    nmcli n on
  # 重启

  # 显示 NetworkManager 是否接管网络设置：
  nmcli networking / nmcli n
  # 手动开启接管
  nmcli networking on
  # 查看网络连接状态
  nmcli n connectivity / nmcli n c
  # 开启网络连接
  nmcli c on
  # 关闭网络连接
  nmcli c off
  # 显示系统网络状态
  nmcli general status / nmcli g
  # 创建新连接：nmcli c add tyep 连接类型 选项 选项值
  nmcli c add type ethernet con-name 'name' ifname 'ens33'
  # 修改连接
  nmcli c m ens33 ipv4.address 192.168.80.10/22  # 修改 IP 地址和子网掩码
  nmcli c m ens33 ipv4.method manual             # 修改为静态配置，默认是 auto
  nmcli c m ens33 ipv4.gateway 192.168.80.2      # 修改默认网关
  nmcli c m ens33 ipv4.dns 223.5.5.5             # 修改 DNS
  nmcli c m ens33 +ipv4.dns 119.29.29.29         # 添加一个 DNS
  nmcli c m ens33 ipv6.method ignored            # 将 IPv6 禁用
  nmcli c m ens33 connection.autoconnect yes     # 开机启动
  # 删除指定连接
  nmcli c delete 'name'
  # 重载所有连接的配置文件
  nmcli c reload
  # 重载某一指定连接的配置文件
  nmcli c load 'name'
```
### 20.ubuntu磁盘根分区是 lvm 格式的 /dev/mapper/ubuntu--vg-ubuntu--lv 扩展分区
```linux
  # 显示存在的卷组
  vgdisplay
  # Free PE / Size 3839 / <15.00 GiB 这是还可以扩充的大小
  lvextend -L 120G /dev/mapper/ubuntu--vg-ubuntu--lv     //增大至120G
  lvextend -L +20G /dev/mapper/ubuntu--vg-ubuntu--lv     //增加20G
  lvreduce -L 50G /dev/mapper/ubuntu--vg-ubuntu--lv      //减小至50G
  lvreduce -L -8G /dev/mapper/ubuntu--vg-ubuntu--lv      //减小8G
  lvresize -L  30G /dev/mapper/ubuntu--vg-ubuntu--lv     //调整为30G
  resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv            //执行调整
```
### 21.防火墙
```linux
  # 查看版本:
  firewall-cmd --versio
  # 查看帮助
  firewall-cmd --help
  # 显示状态
  firewall-cmd --state
  # 查看所有打开的端口
  firewall-cmd --zone=public --list-ports
  # 更新防火墙规则
  firewall-cmd --reload
  # 查看区域信息
  firewall-cmd --get-active-zones
  # 查看指定接口所属区域
  firewall-cmd --get-zone-of-interface=eth0
  # 拒绝所有包
  firewall-cmd --panic-on
  # 取消拒绝状态
  firewall-cmd --panic-off
  # 查看是否拒绝
  firewall-cmd --query-panic
  # 开启一个端口
  firewall-cmd --zone=public --add-port=80/tcp --permanent    （--permanent永久生效，没有此参数重启后失效）
  # 重新载入
  firewall-cmd --reload
  # 删除
  firewall-cmd --zone=public --remove-port=80/tcp --permanent
```
### 22.WSL 桥接
```linux
  # 禁用自动生成dns解析
  touch /etc/wsl.conf
  [network] 
  generateResolvConf = false
  rm -rf /etc/resolv.conf
  vim /etc/resolv.conf
  > # generateResolvConf = false
  > nameserver 223.5.5.5 // dns
  # 设置ip和路由
  ip addr del $(ip addr show eth0 | grep 'inet\b' | awk '{print $2}' | head -n 1) dev eth0
  ip addr add 192.168.199.7/24 broadcast 192.168.199.255 dev eth0
  ip route add default via 192.168.199.1 dev eth0                  #默认路由到网关
  echo "nameserver 223.5.5.5" > /etc/resolv.conf
  # 主机上设置桥接
  Set-VMSwitch WSL -NetAdapterName 以太网
  # PowerShell 脚本自动执行
    #先设置子系统的ip和dns 同时也为了让系统创建wsl网卡
    echo "设置wsl ip 和dns"
    wsl -d Ubuntu-20.04 -u root /etc/ip_set.sh

    echo "设置wsl 桥接到以太网上"
    Set-VMSwitch WSL -NetAdapterName 以太网

    echo "设置桥接后的以太网dns"
    netsh interface ip add dnsservers name="vEthernet (WSL) 2" address=223.5.5.5
    netsh interface ip add dnsservers name="vEthernet (WSL) 2" address=219.141.136.10

    echo "设置完成"

```
### 23.rpm 安装 oracle client
```linux
  rpm -Uvh oracle-instantclient11.2-basic-11.2.0.4.0-1.x86_64.rpm
  rpm -Uvh oracle-instantclient11.2-devel-11.2.0.4.0-1.x86_64.rpm
  rpm -Uvh oracle-instantclient11.2-sqlplus-11.2.0.4.0-1.x86_64.rpm

```
### 24.screen
  ```
  # 新建screen
  screen -S your_screen_name
  
  # 进入screen
  screen -r your_screen_name
  
  Ctrl+D  # 在当前screen下，输入Ctrl+D，删除该screen
  Ctrl+A，Ctrl+D  # 在当前screen下，输入先后Ctrl+A，Ctrl+D，退出该screen
  
  # 显示screen list
  ​​​​​​​screen -ls
  
  # 连接状态为【Attached】的screen
  screen -D  -r your_screen_name  # 解释：-D -r 先踢掉前一用户，再登陆
  
  # 判断当前是否在screen中断下,Ubuntu系统,可以这样:
  sudo vim /etc/screenrc
  # 文件末尾追加一行即可允许设置screen标题
  caption always "%{.bW}%-w%{.rW}%n %t%{-}%+w %=%H %Y/%m/%d "
  
  # 删除指定screen, your_screen_name为待删除的screen name
  ​​​​​​​screen -S your_screen_name -X quit
```
### 25.iptable
```
  # 开放端口
  iptables -A INPUT -p tcp --dport 3000 -j ACCEPT
  # 永久生效
  iptables-save
```
