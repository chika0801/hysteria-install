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

