[TOC]

## v2ray 安装 官方脚本安装
    // 安裝執行檔和 .dat 資料檔
    bash <(curl -O https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
    // 只更新 .dat 資料檔
    bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-dat-release.sh)
    
    移除 V2Ray

    bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh) --remove



## 利用certbot申请证书


    sudo apt install certbot

    sudo certbot certonly --standalone --register-unsafely-without-email

sudo install -d -o nobody -g nogroup /etc/v2ray/
sudo install -m 644 -o nobody -g nogroup /etc/letsencrypt/live/www.example.com/fullchain.pem -t /etc/v2ray/
sudo install -m 600 -o nobody -g nogroup /etc/letsencrypt/live/www.example.com/privkey.pem -t /etc/v2ray/

## v2ray 服务端配置
{
    "log": {
        "loglevel": "warning"
    },
    "inbounds": [
        {
            "port": 443,
            "protocol": "vless",
            "settings": {
                "clients": [
                    {
                        "id": "3122c059-c34c-472d-9874-b03814851235",
                        "level": 0,
                        "email": "love@example.com"
                    }
                ],
                "decryption": "none",
                "fallbacks": [
                    {
                        "dest": 80
                    },
                    {
                        "path": "/ray",
                        "dest": 1234,
                        "xver": 1
                    }
                ]
            },
            "streamSettings": {
                "network": "tcp",
                "security": "tls",
                "tlsSettings": {
                    "alpn": [
                        "http/1.1"
                    ],
                    "certificates": [
                        {
                            "certificateFile": "/etc/v2ray/fullchain.pem",
                            "keyFile": "/etc/v2ray/privkey.pem"
                        }
                    ]
                }
            }
        },
        {
            "port": 1234,
            "listen": "127.0.0.1",
            "protocol": "vless",
            "settings": {
                "clients": [
                    {
                        "id": "3122c059-c34c-472d-9874-b03814851235",
                        "level": 0,
                        "email": "love@example.com"
                    }
                ],
                "decryption": "none"
            },
            "streamSettings": {
                "network": "ws",
                "security": "none",
                "wsSettings": {
                    "acceptProxyProtocol": true,
                    "path": "/ray"
                }
            }
        }
    ],
    "outbounds": [
        {
            "protocol": "freedom"
        }
    ]
}



systemctl start v2ray
systemctl start nginx

systemctl status v2ray
systemctl status nginx

## 开启BBR配置

1.nano /etc/sysctl.conf


net.ipv6.conf.all.accept_ra = 2

net.core.default_qdisc = fq

net.ipv4.tcp_congestion_control = bbr

2.sysctl -p 使内核生效

## acme 
  apt install socat cron 
  curl https://get.acme.sh | sh

  source ~/.bashrc


  ~/.acme.sh/acme.sh --issue -d www.example.com --standalone --keylength ec-256


  ~/.acme.sh/acme.sh --installcert -d www.example.com --ecc --fullchain-file /etc/v2ray/v2ray.crt --key-file /etc/v2ray/v2ray.key

  
  ~/.acme.sh/acme.sh --renew -d www.example.com --force --ecc

## 233boy scripts
  bash <(curl -s -L https://git.io/v2ray.sh)
  
  bash -c "$(curl -s -L https://git.io/v2ray.sh)"
