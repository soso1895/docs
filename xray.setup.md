

useradd -d /home/ubuntu -G sudo -m ubuntu

passwd ubuntu

usermod groups /etc/group /etc/passwd


=======================================================================================



sudo apt install nginx certbot python3-certbot-nginx

sudo certbot certonly --nginx --register-unsafely-without-email

id nobody

sudo install -d -o nobody -g nogroup /etc/ssl/v2ray/

sudo install -m 644 -o nobody -g nogroup /etc/letsencrypt/live/example.com/fullchain.pem -t /etc/ssl/v2ray/

sudo install -m 600 -o nobody -g nogroup /etc/letsencrypt/live/example.com/privkey.pem -t /etc/ssl/v2ray/

----------------------------------------------------------------------------------------

sudo nano /etc/letsencrypt/renewal-hooks/deploy/xray.sh

            #!/bin/bash

            V2RAY_DOMAIN='example.com'

            if [[ "$RENEWED_LINEAGE" == "/etc/letsencrypt/live/$V2RAY_DOMAIN" ]]; then
                install -m 644 -o nobody -g nogroup "/etc/letsencrypt/live/$V2RAY_DOMAIN/fullchain.pem" -t /etc/ssl/v2ray/
                install -m 600 -o nobody -g nogroup "/etc/letsencrypt/live/$V2RAY_DOMAIN/privkey.pem" -t /etc/ssl/v2ray/

                sleep "$((RANDOM % 2048))"
                systemctl restart xray.service
            fi

sudo chmod +x /etc/letsencrypt/renewal-hooks/deploy/xray.sh

=========================================================================================================
mkdir -p ~/www/webpage/ && sudo cp /usr/share/nginx/html/index.html ~/www/webpage/index.html

sudo nano /etc/nginx/conf.d/xray.conf

      server {
              listen 80;
              server_name example.com;
              return 301 https://$http_host$request_uri;
      }


      server {
        listen 127.0.0.1:8080;
        root /home/ubuntu/www/webpage;
        index index.html;
        add_header Strict-Transport-Security "max-age=63072000" always;
      }
---------------------------------------------------------------------
      server {
        listen 80;
        server_name example.com;
        root /home/ubuntu/www/webpage;
        index index.html;

      }
----------------------------------------------------------------------

sudo systemctl reload nginx

===========================================================================

wget https://github.com/XTLS/Xray-install/raw/main/install-release.sh

sudo bash install-release.sh

sudo nano /usr/local/etc/xray/config.json



        {
            "inbounds": [
              {
                "port": 443,
                "protocol": "vless",
                "settings": {
                  "clients": [
                    {
                      "id": "3122c059-c34c-472d-9874-b03814851235",
                      "flow": "xtls-rprx-direct",
                      "level": 0,
                      "email": "love@example.com"
                    }
                  ],
                  "decryption": "none",
                  "fallbacks": [
                    {
                      "dest": 8080
                    }
                  ]
                },
                "streamSettings": {
                  "network": "tcp",
                  "security": "xtls",
                  "xtlsSettings": {
                    "allowInsecure": false,
                    "minVersion": "1.2",
                    "alpn": ["http/1.1"],
                    "certificates": [
                      {
                        "certificateFile": "/etc/ssl/v2ray/fullchain.pem",
                        "keyFile": "/etc/ssl/v2ray/privkey.pem"
                      }
                    ]
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

========================================================================

sudo nano /etc/sysctl.conf

        net.core.default_qdisc=fq
        net.ipv4.tcp_congestion_control=bbr

sudo sysctl -p