# shadowsocks-install
shadowsock 安装方法、使用方法
# 安装Shadowsock server
参考：https://github.com/shadowsocks/shadowsocks-libev#debian--ubuntu

- Debian 8 或以上
- Ubuntu 16.10 或以上
```
sudo apt update
sudo apt install shadowsocks-libev
```

- For Debian 9 (Stretch) 建议
```
sudo sh -c 'printf "deb http://deb.debian.org/debian stretch-backports main" > /etc/apt/sources.list.d/stretch-backports.list'
sudo apt update
sudo apt -t stretch-backports install shadowsocks-libev
```

- 配置
```
vi /etc/shadowsocks-libev/config.json
{
    "server":"ip",
    "server_port":server_port,
    "local_port":local_port,
    "password":"password",
    "timeout":300,
    "method":"chacha20",
    "fast_open": true
}

```

- 开启TCP Fast Open：
```
修改 /etc/sysctl.conf ，加入如下一行：

net.ipv4.tcp_fastopen = 3

执行如下命令使之生效：
sysctl -p
```

[开启BBR](https://www.liuboping.com/%e5%bc%80%e5%90%aftcp-bbr%e6%8b%a5%e5%a1%9e%e6%8e%a7%e5%88%b6%e7%ae%97%e6%b3%95/)


```

开机后 uname -r 看看是不是内核 >= 4.9

执行 lsmod | grep bbr，如果结果中没有 tcp_bbr 的话就先执行
modprobe tcp_bbr
echo "tcp_bbr" >> /etc/modules-load.d/modules.conf
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf

执行如下命令使之生效：
sysctl -p


sysctl net.ipv4.tcp_available_congestion_control
sysctl net.ipv4.tcp_congestion_control

如果结果都有 bbr, 则证明你的内核已开启 bbr

执行 lsmod | grep bbr, 看到有 tcp_bbr 模块即说明 bbr 已启动
```
启动：

# Start
sudo systemctl start shadowsocks-libev.service
# Stop
sudo systemctl stop shadowsocks-libev.service
# Restart
sudo systemctl restart shadowsocks-libev.service

# 下载shadowsocks client 
[Download](https://github.com/shadowsocks/ShadowsocksX-NG/releases/tag/v1.9.4)
