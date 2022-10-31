> 系统：ubuntu20.04
> 

## 安装Nginx

##### 1. 安装 Nginx
``` sh
apt install nginx -y
```

##### 2. 配置文件域名，申请证书时会读取这个配置
> 说明：mydomain.com 需要替换成自己的域名
```
# vim /etc/nginx/sites-enabled/default
# 编辑
server {
    server_name mydomain.com;

    location / {
        proxy_set_header Host mydomain.com;
        proxy_pass https://mydomain.com;
    }
}
```
## 申请证书

##### 1. 安装acme.sh
``` sh
# email替换自己的邮箱
curl  https://get.acme.sh | sh -s email=suleiabc@gmail.com
```
##### 2. 注册acme(已注册略过)
``` sh
# cd ~/.acme.sh
# 使用自己的email进行注册，注册完成后会自动保存注册信息

acme.sh --register-account - suleiabc@gmail.com --server zerossl
```
##### 3. 生成证书
生成证书前，域名的dns指向应该是服务器ip，不能开启cloudflare中的自动代理，开启自动代理后，ping你的域名解析出来的ip不是真实服务器ip。
```sh
# cd ~/.acme.sh

acme.sh --issue -d ceshi.slpro.cn --nginx
```
##### 4.  copy/install 证书
``` sh
mkdir /etc/nginx/ssl

acme.sh  --install-cert  -d  ceshi.slpro.cn   \
        --key-file   /etc/nginx/ssl/ceshi.slpro.cn.key \
        --fullchain-file /etc/nginx/ssl/fullchain.cer \
        --reloadcmd  "service nginx force-reload"

```
## Nginx配置
1. 新创建一个配置文件，并添加配置
``` sh
vim /etc/nginx/conf.d/my.conf

server {
    listen  443 ssl;
    ssl_certificate       /etc/nginx/ssl/fullchain.cer;
    ssl_certificate_key   /etc/nginx/ssl/ceshi.slpro.cn.key;
    ssl_protocols         TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers           HIGH:!aNULL:!MD5;
    server_name           ceshi.slpro.cn;
        location /ali { # 与 V2Ray 配置中ws的 path 必须保持一致
            proxy_redirect off;
            proxy_pass http://127.0.0.1:10000;#假设WebSocket监听在环回地址的10000端口上
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host $http_host;
            }
}
```
2. 重启nginx
```sh
nginx -s reload
```

## 安装v2ray
1. 使用[官方脚本](https://github.com/v2fly/fhs-install-v2ray)安装
```sh
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
```
2. 修改配置文件
```sh
vim /usr/local/etc/v2ray/config.json

{
 "inbounds": [
   {
     "port": 10000, //V2Ray 监听端口
     "listen":"127.0.0.1",
     "protocol": "vmess",
     "settings": {
       "clients": [
         {
           "id": "xxxxxxxx", //一个 UUID 组成的密钥
           "alterId": 0
         }
       ]
     },
     "streamSettings": {
       "network": "ws",
       "wsSettings": {
       "path": "/ali" //ws路径, nginx中配置必须和此一致
       }
     }
   }
 ],
 "outbounds": [
   {
     "protocol": "freedom",
     "settings": {}
   }
 ]
}
```
3. 启动v2ray
```sh
service v2ray start

service v2ray status

```

## CLOUNDFLARE设置
_*不执行这一步则不会使用 "CDN中转", 如 IP 可用则不建议开启, 开启通常会导致减速._

1.  在 CLOUNDFLARE 的 SSL 设置为 Full.
2.  在 CLOUNDFLARE 的 DNS 选项 Status 设置为 DNS and HTTP proxy(CDN).


## 防火墙设置

1. 设置防火墙规则(只开启必要端口)
```sh
ufw allow 443
ufw allow <ssh端口号>
ufw delete allow 3306 (删除已开放的端口)
```
2. 启用防火墙
```bash
ufw status
ufw enable
```
3. [[查看systemd服务日志]]