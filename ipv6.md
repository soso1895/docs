[TOC]

    
---

## ufw 转发配置修改 
    ipv6 开启转发编辑 /etc/ufw/before6.rules  ipv4 编辑/etc/ufw/before.rules 

    #START OPENVPN RULES
    #NAT table rules
    *nat
    :POSTROUTING ACCEPT [0:0] 
    #Allow traffic from OpenVPN client to ens3 (change to the interface you discovered!)
    -A POSTROUTING -s 10.8.0.0/8 -o ens3 -j MASQUERADE
    COMMIT
    #END OPENVPN RULES

---
## v2ray 开启webstock 配置文件

    {
      "settings":{
        "clients":[
          {
            "id":"64e777fa-0a4c-46d0-87f8-6d6f138bdbe7",
            "alterId":64
          }
        ]
      },
      "protocol":"vmess",
      "port":80,
      "streamSettings":{
        "wsSettings":{
          "path":"/",
          "headers":{}
        },
        "network":"ws"
      }
    }

***
## v2ray官方脚本安装方法

    wget https://install.direct/go.sh

    bash go.sh

    systemctl start v2ray

---
## CDN服务商， cloudflare 的DNS

    tori.ns.cloudflare.com
    west.ns.cloudflare.com

---
## 开启BBR配置

1.编辑/etc/sysctl.conf,添加如下内容
2.sysctl -p 使内核生效

    net.ipv6.conf.all.accept_ra = 2

    net.core.default_qdisc = fq

    net.ipv4.tcp_congestion_control = bbr

***
## vscode中空格只有半个字符的解决办法
settings 编辑器设置中  修改 Font family 为 'monospace'

'Droid Sans Mono', 'monospace', monospace, 'Droid Sans Fallback'

