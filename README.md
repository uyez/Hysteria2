# Hysteria2一键安装教程，搭建歇斯底里2（windows,安卓,ios,mac）客户端下载，sing-box配置使用教程
Hysteria2一键安装视频教程：▶ https://youtu.be/N6j3gWde8cQ

### 步骤：
### 一、准备一个VPS服务器（Debian）
Vultr注册账号：https://www.vultr.com/?ref=8753714

### 二、下载搭建工具 FinalShell
FinalShell下载：<a href="https://kjfx.lanzoui.com/iqm6Uosbzha" target="_blank">点击下载>></a><br>
备用下载（Windows MacOS Linux）：<a href="http://www.hostbuf.com/t/988.html" target="_blank">点击下载>></a>

### 三、更新 VPS 系统，安装组件

    apt update -y
    apt install curl sudo -y

### 四、Hysteria 2 一键安装脚本

    wget -N --no-check-certificate https://raw.githubusercontent.com/flame1ce/hysteria2-install/main/hysteria2-install-main/hy2/hysteria.sh && bash hysteria.sh

### 五、Hysteria 服务相关命令

    systemctl start hysteria-server.service    # 启动 hysteria 服务
    systemctl enable hysteria-server.service   # 设置 hysteria 服务 开机自启
    systemctl restart hysteria-server.service  # 重启 hysteria 服务
    systemctl stop hysteria-server.service     # 停止 hysteria 服务
    systemctl status hysteria-server.service   # 查看 hysteria 服务 状态

### 六、客户端配置
- Windows 建议使用 V2rayN，<a href="https://github.com/2dust/v2rayN/releases/latest" target="_blank">点击下载>></a><br>
如果你不清楚下载哪个文件，请下载 “zz_v2rayN-With-Core-SelfContained.7z” 解压使用。

- IOS/MAC 建议使用 sing-box 或 Shadowrocket<br>
需要使用美区 AppleID 登录 App Store 下载，如果没有美区ID，可以点击免费申请一个 <a href="https://github.com/kjfx/AppleID" target="_blank">美区AppleID>></a>

- 安卓手机 建议使用 sing-box<br>
可以在 Google Play 商店下载，或者<a href="https://github.com/SagerNet/sing-box/releases/latest" target="_blank">官方下载>></a><br>
请下载.apk文件，建议下载v8a.apk

### 七、sing-box 配置文件

    {
      "dns": {
        "servers": [
          {
            "tag": "cf",
            "address": "https://1.1.1.1/dns-query"
          },
          {
            "tag": "local",
            "address": "223.5.5.5",
            "detour": "direct"
          },
          {
            "tag": "block",
            "address": "rcode://success"
          }
        ],
        "rules": [
          {
            "geosite": "category-ads-all",
            "server": "block",
            "disable_cache": true
          },
          {
            "outbound": "any",
            "server": "local"
          },
          {
            "geosite": "cn",
            "server": "local"
          }
        ],
        "strategy": "ipv4_only"
      },
      "inbounds": [
        {
          "type": "tun",
          "inet4_address": "172.19.0.1/30",
          "auto_route": true,
          "strict_route": false,
          "sniff": true
        }
      ],
      "outbounds": [
        {
          "type": "hysteria2",
          "tag": "proxy",
          "server": "IP",             // VPS ip
          "server_port": 443,         // 端口
          "up_mbps": 20,              //上传速率，实际填写，过大会导致流量浪费
          "down_mbps": 100,           //下载速率，实际填写，过大会导致流量浪费
          "password": "88888888",   //hysteria2 服务密码
          "tls": {
            "enabled": true,
            "server_name": "www.bing.com",
            "insecure": true              
          }
        },
        {
          "type": "direct",
          "tag": "direct"
        },
        {
          "type": "block",
          "tag": "block"
        },
        {
          "type": "dns",
          "tag": "dns-out"
        }
      ],
      "route": {
        "rules": [
          {
            "protocol": "dns",
            "outbound": "dns-out"
          },
          {
            "geosite": "cn",
            "geoip": [
              "private",
              "cn"
            ],
            "outbound": "direct"
          },
          {
            "geosite": "category-ads-all",
            "outbound": "block"
          }
        ],
        "auto_detect_interface": true
      }
    }



### 八、常见问题
节点如果不能正常使用，请放行端口，请将以下命令复制到搭建工具，再点回车

    iptables -I INPUT -p tcp --dport 443 -j ACCEPT

*请将 443 改为你节点的端口再使用
    

