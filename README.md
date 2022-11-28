## [hysteria](https://github.com/apernet/hysteria/releases)安装指南(Quick Installation Guide)

1. 下载程序(Download hysteria)

linux-amd64
```
curl -Lo /root/hysteria https://github.com/apernet/hysteria/releases/latest/download/hysteria-linux-amd64 && chmod +x /root/hysteria
```

2. 下载配置(Download config)

```
curl -Lo /root/hysteria_config.json https://raw.githubusercontent.com/chika0801/hysteria-install/main/config_server.json
```

3. 下载systemctl配置(Download systemctl config)

```
curl -Lo /etc/systemd/system/hysteria.service https://raw.githubusercontent.com/chika0801/hysteria-install/main/hysteria.service
```

4. 上传证书和私钥(Upload certificate and private key)

- 将证书文件改名为`fullchain.cer`，将私钥文件改名为`private.key`，使用WinSCP登录你的VPS，将它们上传到`/root/`目录。(Rename the certificate file to `fullchain.cer` and the private key file to `private.key`, log in to your VPS using WinSCP, upload them to the `/root/` directory.)
- [用acme申请 SSL 证书](https://github.com/chika0801/Xray-install#1%E7%94%A8acme%E7%94%B3%E8%AF%B7-ssl-%E8%AF%81%E4%B9%A6)

5. [多端口(端口跳跃)](https://hysteria.network/zh/docs/port-hopping/#%e6%9c%8d%e5%8a%a1%e7%ab%af%e9%85%8d%e7%bd%ae)服务器配置

- 以 Debian 11 为例，将 eth0 上的 UDP 16386-16486 端口转发到 16384 端口：

<details><summary>点击查看详细步骤</summary>

```
apt install -y iptables
```

```
iptables -t nat -A PREROUTING -i eth0 -p udp --dport 16386:16486 -j DNAT --to-destination :16384
```

```
iptables-save > /root/iptables
```

```
cat > /etc/network/if-pre-up.d/iptables << EOF
#!/bin/sh
/sbin/iptables-restore < /root/iptables
EOF
```

```
chmod +x /etc/network/if-pre-up.d/iptables
```

</details>

iptables -t nat -nL --line
iptables -t nat -D PREROUTING 1


6. 启动程序(Start hysteria)

```
systemctl daemon-reload && systemctl enable --now hysteria
```

```
systemctl status hysteria
```

| 项目 | |
| :--- | :--- |
| 程序(hysteria file) | /root/hysteria |
| 配置(config) | /root/hysteria_config.json |
| 证书(certificate) | /root/fullchain.cer |
| 私钥(private key) | /root/private.key |
| systemctl配置(systemctl config) | /etc/systemd/system/hysteria.service |
| 查看日志(view log) | journalctl -u hysteria --output cat -e |
| 实时日志(real-time logs) | journalctl -u hysteria --output cat -f |

## v2rayN配置指南

1. 下载Windows客户端程序[hysteria-windows-amd64.exe](https://github.com/HyNetwork/hysteria/releases/latest/download/hysteria-windows-amd64.exe)，重命令为`hysteria.exe`，复制到v2rayN文件夹。

2. 下载客户端配置[config_client.json](https://raw.githubusercontent.com/chika0801/hysteria-install/main/config_clinet.json)，修改`chika.example.com`为证书中包含的域名，修改`10.0.0.1`为VPS的IP。

3. `服务器` ——> `添加自定义配置服务器` ——> `浏览(B)` ——> `选择客户端配置` ——> `Core类型 hysteria` ——> `Socks端口 50000`

![1](https://user-images.githubusercontent.com/88967758/195763557-f9706952-f2fc-466f-9787-bf00d138562d.jpg)

小技巧：只要证书在有效期内，证书中包含的域名不用解析到VPS的IP。一份证书，在多个VPS上使用。
