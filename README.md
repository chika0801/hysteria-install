# [Hysteria 2](https://github.com/apernet/hysteria) 安装指南

## 服务端

### 安装

1. 下载程序（**linux-amd64**）或 [编译程序](compile_hysteria.md)

```
curl -Lo hysteria https://github.com/apernet/hysteria/releases/latest/download/hysteria-linux-amd64 && chmod +x hysteria && mv -f hysteria /usr/local/bin/
```                 

2. 下载配置

```
curl -Lo /root/hysteria_config.yaml https://raw.githubusercontent.com/chika0801/hysteria-install/main/config_server.yaml
```

3. 下载systemctl配置

```
curl -Lo /etc/systemd/system/hysteria.service https://raw.githubusercontent.com/chika0801/hysteria-install/main/hysteria.service && systemctl daemon-reload
```

4. 上传证书和私钥

- 将证书文件改名为 **fullchain.cer**，将私钥文件改名为 **private.key**，将它们上传到 **/root** 目录

5. 启动程序

```
systemctl enable --now hysteria
```

| 项目 | |
| :--- | :--- |
| 程序 | **/usr/local/bin/hysteria** |
| 配置 | **/root/hysteria_config.yaml** |
| 重启 | `systemctl restart hysteria` |
| 状态 | `systemctl status hysteria` |
| 查看日志 | `journalctl -u hysteria -o cat -e` |
| 实时日志 | `journalctl -u hysteria -o cat -f` |

### 卸载

```
systemctl disable --now hysteria && rm -f /usr/local/bin/hysteria /root/hysteria_config.yaml /etc/systemd/system/hysteria.service
```

## 客户端

### 由 v2rayN 提供 HTTP SOCKS5 代理，由 v2rayN 提供路由规则

1. 下载Windows客户端程序[hysteria-windows-amd64.exe](https://github.com/apernet/hysteria/releases/latest/download/hysteria-windows-amd64.exe)，重命名为hysteria.exe，复制到v2rayN\bin\hysteria文件夹。

2. 下载客户端配置[config_client.yaml](config_client.yaml)，修改chika.example.com为证书中包含的域名，修改10.0.0.1为VPS的IP。

3. v2rayN：服务器 ——> 添加自定义配置服务器 ——> 浏览 ——> 选择客户端配置 ——> Core类型 hysteria ——> Socks端口 50000

### 由 sing-box 提供 Tun 模式（透明代理），由 sing-box 提供路由规则

1. sing-box：参考[Windows 使用方法](https://github.com/chika0801/sing-box-examples/blob/main/README.md)，将[客户端配置](https://github.com/chika0801/sing-box-examples/blob/main/Tun/config_client_windows.json)进行如下修改。

原内容

```jsonc
        {
            "tag": "proxy",
            // 粘贴你的客户端配置，需要保留 "tag": "proxy",
        },
```

替换为

```jsonc
        {
            "type": "socks",
            "tag": "proxy",
            "server": "127.0.0.1",
            "server_port": 50000
        },
```

检查此处有 **hysteria.exe**

```
            {
                "process_name": [ // 直连的 Windows 可执行程序
                    "xray.exe",
                    "hysteria.exe",
                    "tuic.exe",
                    "tuic-client.exe",
                    "juicity.exe",
                    "juicity-client.exe"
                ],
                "outbound": "direct"
            },
```

2. v2rayN：服务器 ——> 添加自定义配置服务器 ——> 浏览 ——> 选择客户端配置 ——> Core类型 hysteria ——> Socks端口 0。
