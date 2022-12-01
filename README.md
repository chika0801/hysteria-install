## [hysteria](https://github.com/apernet/hysteria/releases)安装指南

1. 下载程序（linux-amd64）

```
curl -Lo /root/hysteria https://github.com/apernet/hysteria/releases/latest/download/hysteria-linux-amd64 && chmod +x /root/hysteria
```

2. 下载配置

```
curl -Lo /root/hysteria_config.json https://raw.githubusercontent.com/chika0801/hysteria-install/main/config_server.json
```

3. 下载systemctl配置

```
curl -Lo /etc/systemd/system/hysteria.service https://raw.githubusercontent.com/chika0801/hysteria-install/main/hysteria.service
```

4. 上传证书和私钥

- 将证书文件改名为`fullchain.cer`，将私钥文件改名为`private.key`，使用WinSCP登录你的VPS，将它们上传到`/root/`目录。

5. [多端口（端口跳跃）](https://hysteria.network/zh/docs/port-hopping/)服务器配置

- 以 Debian 11 为例，将 eth0 上的 UDP 16386-16486 端口转发到 16384 端口。

- 服务器端正常监听在 16384 端口，在客户端用 chika.example.com:16384,16386-16486 连接即可。

- chika.example.com:16384,16386-16486 表示服务器在 16384 和 16386-16486 端口上可用（共 102 个端口）。

- 当然，服务端在多个端口可用并不代表客户端一定要使用它们。如果客户端不希望开启端口跳跃，依然可以从这些端口里随便选一个进行连接。

<details><summary>点击查看详细步骤</summary>

安装

```
apt install -y iptables-persistent
```

添加

```
iptables -t nat -A PREROUTING -i eth0 -p udp --dport 16386:16486 -j DNAT --to-destination :16384
```

```
netfilter-persistent save
```

查看

```
iptables -t nat -nL --line
```

删除

```
iptables -t nat -D PREROUTING 1
```

```
netfilter-persistent save
```

</details>

6. 启动程序

```
systemctl daemon-reload && systemctl enable --now hysteria
```

```
systemctl status hysteria
```

| 项目 | |
| :--- | :--- |
| 程序 | /root/hysteria |
| 配置 | /root/hysteria_config.json |
| 检查 | /root/hysteria server -c hysteria_config.json |
| 证书 | /root/fullchain.cer |
| 私钥 | /root/private.key |
| systemctl配置 | /etc/systemd/system/hysteria.service |
| 查看日志 | journalctl -u hysteria --output cat -e |
| 实时日志 | journalctl -u hysteria --output cat -f |

## v2rayN配置指南

1. 下载Windows客户端程序[hysteria-windows-amd64.exe](https://github.com/HyNetwork/hysteria/releases/latest/download/hysteria-windows-amd64.exe)，重命令为`hysteria.exe`，复制到v2rayN文件夹。

2. 下载客户端配置[config_client.json](https://raw.githubusercontent.com/chika0801/hysteria-install/main/config_clinet.json)，修改`chika.example.com`为证书中包含的域名，修改`10.0.0.1`为VPS的IP。

3. `服务器` ——> `添加自定义配置服务器` ——> `浏览(B)` ——> `选择客户端配置` ——> `Core类型 hysteria` ——> `Socks端口 50000`

![1](https://user-images.githubusercontent.com/88967758/195763557-f9706952-f2fc-466f-9787-bf00d138562d.jpg)

小技巧：只要证书在有效期内，证书中包含的域名不用解析到VPS的IP。一份证书，在多个VPS上使用。

## ShadowSocksR Plus+配置指南

| 选项 | 值 |
| :--- | :--- |
| 服务器节点类型 | Hysteria |
| 别名（可选） |  |
| 服务器地址 | VPS的IP |
| 端口 | 16384 |
| 传输协议 | udp |
| 验证类型 | 已禁用 |
| QUIC 连接接收窗口 | 4194304 |
| QUIC 流接收窗口 | 1677721 |
| 禁用 MTU 探测 | 不勾 |
| 上行链路容量 | 50 |
| 下行链路容量 | 150 |
| 混淆密码（可选） | pJCGB4ZmQNyYk8jkf9jq |
| TLS 主机名 | 证书中包含的域名 |
| QUIC TLS ALPN | h3 |
| 允许不安全连接 | 不勾 |
| 自签证书 | 不勾 |
| TCP 快速打开 | 勾上 |
| 启用自动切换 | 不勾 |
| 本地端口 | 1234 |
