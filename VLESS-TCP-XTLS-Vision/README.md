## [XTLS Vision](https://github.com/XTLS/Xray-core/discussions/1295)[[ENG]](https://github.com/seakfind/examples/blob/main/xtls-vision/README.md)安装指南(Quick Installation Guide)

1. 安装程序(Install Xray)

```
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install --beta
```

2. 下载配置(Download config)

```
curl -Lo /usr/local/etc/xray/config.json https://raw.githubusercontent.com/chika0801/Xray-examples/main/VLESS-TCP-XTLS-Vision/config_server.json
```

3. 上传证书和私钥(Upload certificate and private key)

- 将证书文件改名为`fullchain.cer`，将私钥文件改名为`private.key`，使用WinSCP登录你的VPS，将它们上传到`/etc/ssl/private/`目录，执行下面的命令。(Rename the certificate file to `fullchain.cer` and the private key file to `private.key`, log in to your VPS using WinSCP, upload them to the `/etc/ssl/private/` directory, and execute the following command.)

```
chown -R nobody:nogroup /etc/ssl/private/
```

- [使用证书时权限不足](https://github.com/v2fly/fhs-install-v2ray/wiki/Insufficient-permissions-when-using-certificates-zh-Hans-CN)
- [用acme申请 SSL 证书](https://github.com/chika0801/Xray-install#1%E7%94%A8acme%E7%94%B3%E8%AF%B7-ssl-%E8%AF%81%E4%B9%A6)

4. 启动程序(Start xray)

```
systemctl restart xray
```

```
systemctl status xray
```

| 项目 | |
| :--- | :--- |
| 配置(config) | /usr/local/etc/xray/config.json |
| 证书(certificate) | /etc/ssl/private/fullchain.cer |
| 私钥(private key) | /etc/ssl/private/private.key |
| 路由规则文件(routing rules file) | /usr/local/share/xray/ |
| 查看日志(view log) | journalctl -u xray --output cat -e |
| 实时日志(real-time logs) | journalctl -u xray --output cat -f |

## v2rayN配置指南

1. `服务器` ——> `添加[VLESS服务器]`

![1](https://user-images.githubusercontent.com/88967758/200235011-84299a14-7a5c-409b-a7c6-0ad42ba2c672.jpg)

## 自定义配置

1. 下载客户端配置[config_client.json](https://raw.githubusercontent.com/chika0801/Xray-examples/main/VLESS-TCP-XTLS-Vision/config_client.json)

2. 找到`"address": ""`，在`""`中间输入VPS的IP。找到`"serverName": ""`，在`""`中间输入证书中包含的域名。

3. `服务器` ——> `添加自定义配置服务器` ——> `浏览(B)` ——> `选择客户端配置` ——> `Core类型 Xray` ——> `Socks端口 0`

![1](https://user-images.githubusercontent.com/88967758/199512235-7f7d78a6-e27d-4db8-b6f5-7ef4212f1af9.jpg)

小技巧：只要证书在有效期内，证书中包含的域名不用解析到VPS的IP。一份证书，在多个VPS上使用。

## v2rayNG配置指南

| 选项 | 值 |
| :--- | :--- |
| 地址(address) | VPS的IP |
| 端口(prot) | 16387 |
| 用户ID(id) | chika |
| 流控(flow) | xtls-rprx-vision |
| 传输协议(network) | tcp |
| 传输层安全(tls) | tls |
| SNI | 证书中包含的域名 |
| uTLS | chrome |

## XTLS 流控模式与客户端选项速查表

| 服务器配置 | 客户端选项 | uTLS 支持 |
| :--- | :--- | :--- |
| "flow": "**xtls-rprx-vision**" | 流控(flow): **xtls-rprx-vision**  + 传输层安全(tls): **tls** | 是 |
| "security": "**tls**" | 流控(flow): **留空** + 传输层安全(tls): **tls** | 是 |
| "**tls**Settings" | | |

| 服务器配置 | 客户端选项 | uTLS 支持 |
| :--- | :--- | :--- |
| "flow": "**xtls-rprx-direct**" | 流控(flow): **xtls-rprx-direct** + 传输层安全(tls): **xtls** | 否 |
| "security": "**xtls**" | 流控(flow): **xtls-rprx-splice** + 传输层安全(tls): **xtls** | 否 |
| "**xtls**Settings" | 流控(flow) **留空** + 传输层安全(tls) **tls** | 是 |
