# tproxy setup

## lala.im global client config.json
```json
{
  "log": {
    "access": "",
    "error": "",
    "loglevel": "warning"
  },
   "inbounds": [
    {
      "port": 1080,
      "protocol": "socks",
      "settings": {
        "auth": "noauth",
        "udp": true
      }
    },
    {
      "port": 12345,
      "protocol": "dokodemo-door",
      "settings": {
        "network": "tcp,udp",
        "followRedirect": true
      },
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls"]
      }
    }
  ],
  "outbounds": [
    {
      "tag": "proxy",
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "www.example.com",
            "port": 443,
            "users": [
              {
                "id": "3122c059-c34c-472d-9874-b03814851235",
                "alterId": 64,
                "email": "t@t.tt",
                "security": "auto"
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "security": "tls",
        "tlsSettings": {
          "allowInsecure": false
        },
        "wsSettings": {
          "path": "/ray"
        }
      },
      "mux": {
        "enabled": true,
        "concurrency": 8
      }
    },
    {
      "tag": "direct",
      "protocol": "freedom",
      "settings": {}
    },
    {
      "tag": "block",
      "protocol": "blackhole",
      "settings": {
        "response": {
          "type": "http"
        }
      }
    }
  ],
  "routing": {
    "domainStrategy": "IPIfNonMatch",
    "rules": [
      {
        "type": "field",
        "inboundTag": [
          "api"
        ],
        "outboundTag": "api"
      }
    ]
  }
}
```

## Global iptables setup

        sudo iptables -t nat -N V2RAY
        sudo iptables -t nat -A V2RAY -d 192.168.0.0/16 -j RETURN
        sudo iptables -t nat -A V2RAY -p tcp -j REDIRECT --to-ports 12345
        sudo iptables -t nat -A PREROUTING -p tcp -j V2RAY



        apt -y install ipset

        wget https://raw.githubusercontent.com/17mon/china_ip_list/master/china_ip_list.txt

        ipset -N cn hash:net

        for i in $(cat china_ip_list.txt); do ipset -A cn $i; done

        iptables -t nat -N V2RAY
        iptables -t nat -A V2RAY -d 192.168.0.0/16 -j RETURN
        iptables -t nat -A V2RAY -p tcp -m set --match-set cn dst -j RETURN
        iptables -t nat -A V2RAY -p tcp -j REDIRECT --to-ports 12345
        iptables -t nat -A PREROUTING -p tcp -j V2RAY

        https://lala.im/6417.html

        mkdir -p /etc/iptables && iptables-save > /etc/iptables/rules.v4

# Official tproxy setup client config


```json
{
  "inbounds": [
    {
      "tag": "transparent",
      "port": 12345,
      "protocol": "dokodemo-door",
      "settings": {
        "network": "tcp,udp",
        "followRedirect": true
      },
      "sniffing": {
        "enabled": true,
        "destOverride": [
          "http",
          "tls"
        ]
      },
      "streamSettings": {
        "sockopt": {
          "tproxy": "tproxy",
          "mark": 255
        }
      }
    },
    {
      "port": 1080,
      "protocol": "socks",
      "sniffing": {
        "enabled": true,
        "destOverride": [
          "http",
          "tls"
        ]
      },
      "settings": {
        "auth": "noauth"
      }
    }
  ],
  "outbounds": [
    {
      "tag": "proxy",
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "www.example.com",
            "port": 443,
            "users": [
              {
                "id": "3122c059-c34c-472d-9874-b03814851235",
                "alterId": 64,
                "email": "t@t.tt",
                "security": "auto"
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "sockopt": {
          "mark": 255
        },
        "network": "ws",
        "security": "tls",
        "tlsSettings": {
          "allowInsecure": false
        },
        "wsSettings": {
          "path": "/ray"
        }
      },
      "mux": {
        "enabled": true
      }
    },
    {
      "tag": "direct",
      "protocol": "freedom",
      "settings": {
        "domainStrategy": "UseIP"
      },
      "streamSettings": {
        "sockopt": {
          "mark": 255
        }
      }
    },
    {
      "tag": "block",
      "protocol": "blackhole",
      "settings": {
        "response": {
          "type": "http"
        }
      }
    },
    {
      "tag": "dns-out",
      "protocol": "dns",
      "streamSettings": {
        "sockopt": {
          "mark": 255
        }
      }
    }
  ],
  "dns": {
    "servers": [
      {
        "address": "223.5.5.5",
        "port": 53,
        "domains": [
          "geosite:cn",
          "ntp.org",
          "www.example.com"
        ]
      },
      {
        "address": "114.114.114.114",
        "port": 53,
        "domains": [
          "geosite:cn",
          "ntp.org",
          "www.example.com"
        ]
      },
      {
        "address": "8.8.8.8",
        "port": 53,
        "domains": [
          "geosite:geolocation-!cn"
        ]
      },
      {
        "address": "1.1.1.1",
        "port": 53,
        "domains": [
          "geosite:geolocation-!cn"
        ]
      }
    ]
  },
  "routing": {
    "domainStrategy": "IPOnDemand",
    "rules": [
      {
        "type": "field",
        "inboundTag": [
          "transparent"
        ],
        "port": 53,
        "network": "udp",
        "outboundTag": "dns-out"
      },
      {
        "type": "field",
        "inboundTag": [
          "transparent"
        ],
        "port": 123,
        "network": "udp",
        "outboundTag": "direct"
      },
      {
        "type": "field",
        "ip": [
         
          "223.5.5.5",
          "114.114.114.114"
        ],
        "outboundTag": "direct"
      },
      {
        "type": "field",
        "ip": [
         
          "8.8.8.8",
          "1.1.1.1"
        ],
        "outboundTag": "proxy"
      },
      {
        "type": "field",
        "domain": [
          "geosite:category-ads-all"
        ],
        "outboundTag": "block"
      },
      {
        "type": "field",
        "protocol": [
          "bittorrent"
        ],
        "outboundTag": "direct"
      },
      {
        "type": "field",
        "ip": [
          "geoip:private",
          "geoip:cn"
        ],
        "outboundTag": "direct"
      },
      {
        "type": "field",
        "domain": [
          "geosite:cn"
        ],
        "outboundTag": "direct"
      }
    ]
  }
}
```
## Official iptables setup

    ### 设置策略路由
        sudo ip rule add fwmark 1 table 100 
        sudo ip route add local 0.0.0.0/0 dev lo table 100

    ### 代理局域网设备
        sudo iptables -t mangle -N V2RAY
        sudo iptables -t mangle -A V2RAY -d 127.0.0.1/32 -j RETURN
        sudo iptables -t mangle -A V2RAY -d 224.0.0.0/4 -j RETURN 
        sudo iptables -t mangle -A V2RAY -d 255.255.255.255/32 -j RETURN 
        sudo iptables -t mangle -A V2RAY -d 192.168.0.0/16 -p tcp -j RETURN
        sudo iptables -t mangle -A V2RAY -d 192.168.0.0/16 -p udp ! --dport 53 -j RETURN
        sudo iptables -t mangle -A V2RAY -j RETURN -m mark --mark 0xff
        sudo iptables -t mangle -A V2RAY -p udp -j TPROXY --on-ip 127.0.0.1 --on-port 12345 --tproxy-mark 1
        sudo iptables -t mangle -A V2RAY -p tcp -j TPROXY --on-ip 127.0.0.1 --on-port 12345 --tproxy-mark 1
        sudo iptables -t mangle -A PREROUTING -j V2RAY

    ### 代理网关本机
        sudo iptables -t mangle -N V2RAY_MASK 
        sudo iptables -t mangle -A V2RAY_MASK -d 224.0.0.0/4 -j RETURN 
        sudo iptables -t mangle -A V2RAY_MASK -d 255.255.255.255/32 -j RETURN 
        sudo iptables -t mangle -A V2RAY_MASK -d 192.168.0.0/16 -p tcp -j RETURN
        sudo iptables -t mangle -A V2RAY_MASK -d 192.168.0.0/16 -p udp ! --dport 53 -j RETURN
        sudo iptables -t mangle -A V2RAY_MASK -j RETURN -m mark --mark 0xff
        sudo iptables -t mangle -A V2RAY_MASK -p udp -j MARK --set-mark 1
        sudo iptables -t mangle -A V2RAY_MASK -p tcp -j MARK --set-mark 1
        sudo iptables -t mangle -A OUTPUT -j V2RAY_MASK
        
    ### 新建 DIVERT 规则，避免已有连接的包二次通过 TPROXY，理论上有一定的性能提升
        sudo iptables -t mangle -N DIVERT
        sudo iptables -t mangle -A DIVERT -j MARK --set-mark 1
        sudo iptables -t mangle -A DIVERT -j ACCEPT
        sudo iptables -t mangle -I PREROUTING -p tcp -m socket -j DIVERT

# Auto start

    mkdir -p /etc/iptables && iptables-save > /etc/iptables/rules.v4

    sudo nano /etc/systemd/system/tproxyrule.service

            [Unit]
            Description=Tproxy rule
            After=network.target
            Wants=network.target

            [Service]

            Type=oneshot
            # 注意分号前后要有空格
            ExecStart=/sbin/ip rule add fwmark 1 table 100 ; /sbin/ip route add local 0.0.0.0/0 dev lo table 100 ; /sbin/iptables-restore /etc/iptables/rules.v4
            ExecStop=/sbin/ip rule del fwmark 1 table 100 ; /sbin/ip route del local 0.0.0.0/0 dev lo table 100 ; /sbin/iptables -t mangle -F
            # 如果是 nftables，则改为以下命令
            # ExecStart=/sbin/ip rule add fwmark 1 table 100 ; /sbin/ip route add local 0.0.0.0/0 dev lo table 100 ; /sbin/nft -f /etc/nftables/rules.v4
            # ExecStop=/sbin/ip rule del fwmark 1 table 100 ; /sbin/ip route del local 0.0.0.0/0 dev lo table 100 ; /sbin/nft flush ruleset

            [Install]
            WantedBy=multi-user.target
    
    sudo systemctl enable tproxyrule.service

