[TOC]

## v2ray 安装 官方脚本安装
    // 安裝執行檔和 .dat 資料檔
    bash <(curl -O https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
    // 只更新 .dat 資料檔
    bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-dat-release.sh)
    
    移除 V2Ray

    bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh) --remove



## 利用certbot申请证书


    sudo apt install certbot python-certbot-nginx nginx
    sudo apt install certbot python3-certbot-nginx 

    sudo certbot certonly --nginx --register-unsafely-without-email

## nginx配置文件 /etc/nginx/conf.d/v2ray.conf

   server {
        
        listen       443 ssl; 
        server_name  www.example.com;
        ssl on;
        ssl_certificate    /etc/letsencrypt/live/www.example.com/fullchain.pem;
        ssl_certificate_key   /etc/letsencrypt/live/www.example.com/privkey.pem;
        ssl_protocols TLSv1.2 TLSv1.3;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
        ssl_prefer_server_ciphers on;
        ssl_session_cache shared:SSL:10m;
        ssl_session_timeout 10m;
        error_page 497  https://$host$request_uri;

        location /ray {
            proxy_pass       http://127.0.0.1:8880;
            proxy_redirect             off;
            proxy_http_version         1.1;
            proxy_set_header Upgrade   $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host      $http_host;
        }
    }
## v2ray 服务端配置

    {
    "inbounds": [
        {
        "port": 8880,
        "listen":"127.0.0.1",
        "protocol": "vmess",
        "settings": {
            "clients": [
            {
                "id": "3122c059-c34c-472d-9874-b03814851235",
                "alterId": 64
            }
            ]
        },
        "streamSettings": {
            "network": "ws",
            "wsSettings": {
            "path": "/ray"
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

  
  ~/.acme.sh/acme.sh --renew -d mydomain.com --force --ecc

## 233boy scripts
  bash <(curl -s -L https://git.io/v2ray.sh)
  
  bash -c "$(curl -s -L https://git.io/v2ray.sh)"
