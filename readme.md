Realm的使用

1、下载并设置权限
```
wget https://github.com/zhboner/realm/releases/download/v1.2.0/realm
chmod +x realm
```
本地Realm 1.2：https://zhujiwiki.com/wp-content/uploads/2021/01/realm

2、使用

让 realm 监听本机上的 30000 端口，然后转发流量到 example.com:12345
```
./realm -l 127.0.0.1:30000 -r example.com:12345
```
具体的，启动 realm 需要两个参数：-l 和 -r。

-l 指定监听的本机地址和端口，地址可以省略，但必须指定端口。不指定地址的话会使用默认的 127.0.0.1 地址
-r 指定转发的目的地址和端口，均不能省略

3、开机自启及服务
```
vi /etc/systemd/system/realm.service
```
粘贴（改ExecStart=/usr/bin/realm -l x.x.x.x:port -r x.x.x.x:port为自己的转发信息）
```
[Unit]
Description=realm
After=network-online.target
Wants=network-online.target systemd-networkd-wait-online.service

[Service]
Type=simple
User=root
Restart=on-failure
RestartSec=5s
DynamicUser=true
ExecStart=/usr/bin/realm -l x.x.x.x:port -r x.x.x.x:port

[Install]
WantedBy=multi-user.target
```
开机启动
```
systemctl enable --now realm
```
启动/重新启动/停止
```
systemctl start/restart/stop realm
```
