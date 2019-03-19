# Clash-Merlin

**教程分两部分：**
1. Clash的安装；
2. 透明代理的设置。
> 完成第1步之后，已经可以通过在设备上设置 http/socks 代理来实现扶墙；
> 完成第2步后，实现透明代理，无需在设备上做任何操作。
# I. Clash的安装
## 1. 准备
ps. 觉得写的不清楚不精彩的部分，可以参见另外教程（https://shrtm.nu/nGqM）
### 1.1 Clash固件 --- https://shrtm.nu/GFRV
下载适合你机器的armv版本clash固件，**下载完后建议直接改名clash，简化后续操作。。**

### 1.2 配置文件 config.yml
机场提供，或参考：

懒人规则： https://shrtm.nu/mR1W

神机规则：https://shrtm.nu/qcyo

**！！！请确保配置文件无任何问题**

> 做透明代理的话，dns部分 & redir-port & enhance-mode:redir-host 一定要不要注释掉了。
![](https://shrtm.nu/avVB)

### 1.3.Country.mmdb
ip数据库，clash启动时会自动下载安装，但建议提前自备并上传。
链接：http://geolite.maxmind.com/download/geoip/database/GeoLite2-Country.tar.gz 
解压并改名 Country.mmdb 
## 2. 瞎操作部分
### 2.1 上传第一部分的3个文件到路由（Winscp或transmit或者terminal）
* 建议上传到/jffs/.koolshare/ 路径下；
### 2.2 Clash启动
ssh工具：win上Putty/Xshell，Mac上如termius 等，
* 运行权限： chmod +x clash
* 可以先尝试` ./clash -d .` 确认是否能正常启动clash
> 正常情况下，能看到clash的启动log，但关闭ssh工具之后，此进程也会自动关闭，所以请用下面的命令行
* 启动` ./clash -d . & `或者 `nohup ./clash -d .`
> 此时即使关闭ssh工具，clash仍旧能在后台运行

至此，你已经可以在设备的Wi-Fi设置上，通过手动填写http代理，或者：
通过app的socks代理设置，实现扶墙

### 2.3 设置路由开机自启
建立 clash.sh 文件， 写入（如有变动，路径改成自己的）：
`start-stop-daemon -S -b -x /jffs/.koolshare/clash -m -p /tmp/clash.pid — -d /jffs/.koolshare/`

放入 /jffs/scripts 目录下， 登陆路由器梅林后台：tools/scripts， 
将脚本路径加入自启动项目(WAN)：./jffs/scripts/clash.sh

至此，已经实现了clash的路由开机自启动，如需关闭clash进程，
`start-stop-daemon -K -p /tmp/clash.pid`
***


# II. 透明代理设置
懒得写了，看图片吧。
![](https://ws4.sinaimg.cn/large/006tKfTcgy1g186uhz5goj30tw0wuwnr.jpg)

`start-stop-daemon -S -b -x /jffs/.koolshare/clash -m -p /tmp/clash.pid — -d /jffs/.koolshare/
iptables -t nat -A PREROUTING -p tcp --dport 22 -j ACCEPT
iptables -t nat -A PREROUTING -p tcp -j REDIRECT --to-ports 8887
iptables -t nat -A PREROUTING -p udp -m udp --dport 53 -j DNAT --to-destination 192.168.50.1:55`


`#!/bin/sh
/jffs/scripts/clash-iptable.sh`


`chmod +x /jffs/scripts/clash-iptable.sh`

`chmod +x /jffs/scripts/firewall-start`

***
其它参考教程：

Fndroid视频： https://www.youtube.com/watch?v=HnnYl3DFOn0

Raspberry Pi：https://github.com/nkions/clash_raspberrypi

Birkhoff的OpenWrt：https://blog.birkhoff.me/Running-Clash-on-OpenWrt-as-a-transparent-proxy/
