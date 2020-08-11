# 简介

**V2Ray** 路由规则文件加强版，可代替 V2Ray 官方 `geoip.dat` 和 `geosite.dat` 规则文件。利用 GitHub Actions 北京时间每天早上 6 点自动构建，保证规则最新。

## 说明

本项目适用于命令行版本 V2Ray 客户端。第三方桌面图形界面版（GUI）V2Ray 客户端一般都有路由规则图形化配置界面，但一般也都支持使用自定义 V2Ray JSON 配置和 dat 规则文件，请自行研究并修改配置。第三方移动设备版 V2Ray 客户端情况比较复杂，大概率不支持使用自定义 V2Ray JSON 配置和 dat 规则文件，请知悉。

## 规则文件生成方式

### geoip.dat

- 通过仓库 [@Loyalsoldier/geoip](https://github.com/Loyalsoldier/geoip) 生成
- 其中全球 IP 地址（IPv4 和 IPv6）来源于 [MaxMind GeoLite2](https://dev.maxmind.com/geoip/geoip2/geolite2/)，`CN`（中国大陆）类别下的 IPv4 地址来源于 [@17mon/china_ip_list](https://github.com/17mon/china_ip_list/blob/master/china_ip_list.txt)

### geosite.dat

- 通过仓库 [@v2fly/domain-list-community](https://github.com/v2fly/domain-list-community) 生成
- **加入大量中国大陆域名、Apple 域名和 Google 域名**：
  - [@felixonmars/dnsmasq-china-list/accelerated-domains.china.conf](https://github.com/felixonmars/dnsmasq-china-list/blob/master/accelerated-domains.china.conf) 加入到 `geosite:cn` 类别中
  - [@felixonmars/dnsmasq-china-list/apple.china.conf](https://github.com/felixonmars/dnsmasq-china-list/blob/master/apple.china.conf) 加入到 `geosite:geolocation-!cn` 类别中（如希望本文件中的 Apple 域名直连，请参考下面 [geosite 的 Routing 配置方式](https://github.com/IceCodeNew/v2ray-rules-dat#geositedat-1)）
  - [@felixonmars/dnsmasq-china-list/google.china.conf](https://github.com/felixonmars/dnsmasq-china-list/blob/master/google.china.conf) 加入到 `geosite:geolocation-!cn` 类别中（如希望本文件中的 Google 域名直连，请参考下面 [geosite 的 Routing 配置方式](https://github.com/IceCodeNew/v2ray-rules-dat#geositedat-1)）
- **加入最新 GFWList 域名**：通过仓库 [@cokebar/gfwlist2dnsmasq](https://github.com/cokebar/gfwlist2dnsmasq) 生成并加入到 `geosite:geolocation-!cn` 类别中
- **加入附加 GFWList 域名**：通过仓库 [@pexcn/gfwlist-extras](https://github.com/pexcn/gfwlist-extras) 获取并加入到 `geosite:geolocation-!cn` 类别中
- **加入 Greatfire Analyzer 检测到的屏蔽域名**：通过仓库 [@Loyalsoldier/cn-blocked-domain](https://github.com/Loyalsoldier/cn-blocked-domain) 获取 [Greatfire Analyzer](https://zh.greatfire.org/analyzer) 检测到的屏蔽域名，并加入到 `geosite:geolocation-!cn` 类别中
- **加入 EasyList 和 EasyListChina 广告域名**：通过 [@AdblockPlus/EasylistChina+Easylist.txt](https://easylist-downloads.adblockplus.org/easylistchina+easylist.txt) 获取并加入到 `geosite:category-ads-all` 类别中
- **加入 AdGuard DNS Filter 广告域名**：通过 [@AdGuard/DNS-filter](https://kb.adguard.com/en/general/adguard-ad-filters#dns-filter) 获取并加入到 `geosite:category-ads-all` 类别中
- **加入 Peter Lowe 广告和隐私跟踪域名**：通过 [@PeterLowe/adservers](https://pgl.yoyo.org/adservers) 获取并加入到 `geosite:category-ads-all` 类别中
- **加入 Dan Pollock 广告域名**：通过 [@DanPollock/hosts](https://someonewhocares.org/hosts) 获取并加入到 `geosite:category-ads-all` 类别中
- **加入更多代理域名**：通过仓库 [@GeQ1an/Rules](https://github.com/GeQ1an/Rules/tree/master/QuantumultX) 和 [@lhie1/Rules](https://github.com/lhie1/Rules/tree/master) 获取更多代理域名，并加入到 `geosite:geolocation-!cn` 类别中
- **可添加自定义直连、代理和广告域名**：由于上游域名列表更新缓慢或缺失某些域名，所以引入**需要添加的域名**列表。[`hidden 分支`](https://github.com/IceCodeNew/v2ray-rules-dat/tree/hidden)里的三个文件 `direct.txt`、`proxy.txt` 和 `reject.txt`，分别存放自定义的需要添加的直连、代理、广告域名，最终分别加入到 `geosite:cn`、`geosite:geolocation-!cn` 和 `geosite:category-ads-all` 类别中
- **可移除自定义直连、代理和广告域名**：由于上游域名列表存在需要被移除的域名，所以引入**需要移除的域名**列表。[`hidden 分支`](https://github.com/IceCodeNew/v2ray-rules-dat/tree/hidden)里的三个文件 `direct-need-to-remove.txt`、`proxy-need-to-remove.txt` 和 `reject-need-to-remove.txt`，分别存放自定义的需要从 `direct-list`（直连域名列表）、`proxy-list`（代理域名列表）和 `reject-list`（广告域名列表） 移除的域名

## 规则文件下载及使用方式

**下载地址**：

- **geoip.dat**：[https://github.com/IceCodeNew/v2ray-rules-dat/raw/release/geoip.dat](https://github.com/IceCodeNew/v2ray-rules-dat/raw/release/geoip.dat)
- **geosite.dat**：[https://github.com/IceCodeNew/v2ray-rules-dat/raw/release/geosite.dat](https://github.com/IceCodeNew/v2ray-rules-dat/raw/release/geosite.dat)
- **直连域名列表 direct-list.txt**：[https://github.com/IceCodeNew/v2ray-rules-dat/raw/release/direct-list.txt](https://github.com/IceCodeNew/v2ray-rules-dat/raw/release/direct-list.txt)
- **代理域名列表 proxy-list.txt**：[https://github.com/IceCodeNew/v2ray-rules-dat/raw/release/proxy-list.txt](https://github.com/IceCodeNew/v2ray-rules-dat/raw/release/proxy-list.txt)
- **广告域名列表 reject-list.txt**：[https://github.com/IceCodeNew/v2ray-rules-dat/raw/release/reject-list.txt](https://github.com/IceCodeNew/v2ray-rules-dat/raw/release/reject-list.txt)

**使用方式**：

Windows 用户可通过 [Scoop](https://scoop.sh) 来安装：

```shell
scoop bucket add v2ray https://github.com/kidonng/scoop-v2ray
scoop install v2ray-rules-dat
```

非 Windows 用户：

1. 点击上面下载地址，下载 `geoip.dat` 和 `geosite.dat`
2. 把下载下来的 `geoip.dat` 和 `geosite.dat` 文件放入到 V2Ray 的规则文件目录，替换掉原来的 `geoip.dat` 和 `geosite.dat`
3. 修改 V2Ray 配置文件，配置参考下面 👇👇👇

## 参考配置

### geoip.dat

跟 V2Ray 官方 `geoip.dat` 配置方式相同。

**Routing 配置方式**：

```json
"routing": {
  "rules": [
    {
      "type": "field",
      "outboundTag": "Direct",
      "ip": [
        "223.5.5.5/32",
        "119.29.29.29/32",
        "180.76.76.76/32",
        "114.114.114.114/32",
        "geoip:cn",
        "geoip:private"
      ]
    },
    {
      "type": "field",
      "outboundTag": "Proxy",
      "ip": [
        "1.1.1.1/32",
        "1.0.0.1/32",
        "8.8.8.8/32",
        "8.8.4.4/32",
        "geoip:us",
        "geoip:ca"
      ]
    }
  ]
}
```

### geosite.dat

跟 V2Ray 官方 `geosite.dat` 配置方式相同。`geosite:apple-cn` 和 `geosite:google-cn` 为本项目特有的两个类别，分别包含 [@felixonmars/dnsmasq-china-list/apple.china.conf](https://github.com/felixonmars/dnsmasq-china-list/blob/master/apple.china.conf) 和 [@felixonmars/dnsmasq-china-list/google.china.conf](https://github.com/felixonmars/dnsmasq-china-list/blob/master/google.china.conf) 文件里的域名，供希望 Apple 和 Google 域名直连（不走代理）的用户使用。在 Routing 配置中，类别越靠前（上），优先级越高，所以 `geosite:apple-cn` 和 `geosite:google-cn` 要放置在 `geosite:geolocation-!cn` 前（上）面。配置参考下面 👇👇👇

**Routing 配置方式**：

```json
"routing": {
  "rules": [
    {
      "type": "field",
      "outboundTag": "Reject",
      "domain": [
        "geosite:category-ads-all"
      ]
    },
    {
      "type": "field",
      "outboundTag": "Direct",
      "domain": [
        "geosite:apple-cn",
        "geosite:google-cn",
        "geosite:tld-cn",
        "domain:icloud.com",
        "domain:icloud-content.com"
      ]
    },
    {
      "type": "field",
      "outboundTag": "Proxy",
      "domain": [
        "geosite:geolocation-!cn"
      ]
    },
    {
      "type": "field",
      "outboundTag": "Direct",
      "domain": [
        "geosite:cn"
      ]
    }
  ]
}
```

**DNS 配置方式**：

```json
"dns": {
  "servers": [
    {
      "address": "223.5.5.5",
      "port": 53,
      "domains": [
        "geosite:cn"
      ]
    },
    {
      "address": "45.90.28.0",
      "port": 53,
      "domains": [
        "geosite:geolocation-!cn"
      ]
    },
    "45.90.30.0"
  ]
}
```

### 自用 V2Ray 客户端配置（仅供参考，请根据自身需求酌情修改）

注意事项：

- 由于下面客户端配置使用了 DoH (DNS over HTTPS) 功能，所以必须使用 v4.22.0 或更新版本的 [V2Ray](https://github.com/v2fly/v2ray-core/releases)
- 下面客户端配置使 V2Ray 在本机开启 SOCKS 代理（监听 1080 端口）和 HTTP 代理（监听 2080 端口），允许局域网内其他设备连接并使用代理
- BT 流量统统直连（实测依然会有部分 BT 流量走代理，尚不清楚是不是 V2Ray 的 bug。如果服务商禁止 BT 下载的话，请不要为下载软件设置代理）
- 最后，不命中任何路由规则的请求和流量，统统走代理
- `outbounds` 里的第一个大括号内的配置，即为 V2Ray 代理服务的配置。请根据自身需求进行修改，并参照 V2Ray 官网配置说明中的 [配置 > Outbounds > OutboundObject](https://www.v2fly.org/chapter_02/outbounds.html#outboundobject) 部分进行补全

```json
{
  "log": {
    "loglevel": "warning"
  },
  "dns": {
    "servers": [
      {
        "address": "https+local://223.5.5.5/dns-query",
        "domains": [
          "geosite:cn"
        ],
        "expectIPs": [
          "geoip:cn"
        ]
      },
      {
        "address": "https://1.1.1.1/dns-query",
        "domains": [
          "geosite:geolocation-!cn"
        ]
      }
    ]
  },
  "inbounds": [
    {
      "protocol": "socks",
      "listen": "0.0.0.0",
      "port": 1080,
      "tag": "Socks-In",
      "settings": {
        "ip": "127.0.0.1",
        "udp": true,
        "auth": "noauth"
      },
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls"]
      }
    },
    {
      "protocol": "http",
      "listen": "0.0.0.0",
      "port": 2080,
      "tag": "Http-In",
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls"]
      }
    }
  ],
  "outbounds": [
    {
      //下面这行，协议类别要改为socks、shadowsocks或vmess等（记得删除本行文字说明）
      "protocol": "协议类别",
      "settings": {},
      //下面这行，tag的值对应Routing里的outboundTag，这里为Proxy（记得删除本行文字说明）
      "tag": "Proxy",
      "streamSettings": {},
      "mux": {}
    },
    {
      "protocol": "dns",
      "tag": "Dns-Out"
    },
    {
      "protocol": "freedom",
      "tag": "Direct",
      "settings": {
        "domainStrategy": "UseIPv4"
      }
    },
    {
      "protocol": "blackhole",
      "tag": "Reject",
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
        "outboundTag": "Direct",
        "protocol": ["bittorrent"]
      },
      {
        "type": "field",
        "outboundTag": "Dns-Out",
        "inboundTag": [
          "Socks-In",
          "Http-In"
        ],
        "network": "udp",
        "port": 53
      },
      {
        "type": "field",
        "outboundTag": "Reject",
        "domain": [
          "geosite:category-ads-all"
        ]
      },
      {
        "type": "field",
        "outboundTag": "Proxy",
        "domain": [
          "full:www.icloud.com",
          "domain:icloud-content.com"
        ]
      },
      {
        "type": "field",
        "outboundTag": "Direct",
        "domain": [
          "geosite:tld-cn",
          "geosite:icloud"
        ]
      },
      {
        "type": "field",
        "outboundTag": "Proxy",
        "domain": [
          "geosite:geolocation-!cn"
        ]
      },
      {
        "type": "field",
        "outboundTag": "Direct",
        "domain": [
          "geosite:cn"
        ]
      },
      {
        "type": "field",
        "outboundTag": "Direct",
        "ip": [
          "geoip:cn",
          "geoip:private"
        ]
      },
      {
        "type": "field",
        "outboundTag": "Proxy",
        "network": "tcp,udp"
      }
    ]
  }
}
```

## 使用本项目的项目

- [@Loyalsoldier/clash-rules](https://github.com/Loyalsoldier/clash-rules)
- [@Loyalsoldier/surge-rules](https://github.com/Loyalsoldier/surge-rules)

## 致谢

> This product includes GeoLite2 data created by MaxMind, available from [Maxmind](https://www.maxmind.com).

- [MaxMind GeoLite2 Free IP Database](https://dev.maxmind.com/geoip/geoip2/geolite2/)
- [@v2ray/geoip](https://github.com/v2ray/geoip)
- [@Loyalsoldier/geoip](https://github.com/Loyalsoldier/geoip)
- [@v2fly/domain-list-community](https://github.com/v2fly/domain-list-community)
- [@17mon/china_ip_list](https://github.com/17mon/china_ip_list/blob/master/china_ip_list.txt)
- [@felixonmars/dnsmasq-china-list](https://github.com/felixonmars/dnsmasq-china-list)
- [@gfwlist/gfwlist](https://github.com/gfwlist/gfwlist)
- [@pexcn/gfwlist-extras](https://github.com/pexcn/gfwlist-extras)
- [@cokebar/gfwlist2dnsmasq](https://github.com/cokebar/gfwlist2dnsmasq)
- [@Loyalsoldier/cn-blocked-domain](https://github.com/Loyalsoldier/cn-blocked-domain)
- [@GeQ1an/Rules](https://github.com/GeQ1an/Rules/tree/master/QuantumultX)
- [@lhie1/Rules](https://github.com/lhie1/Rules/tree/master)
- [@AdblockPlus/EasylistChina+Easylist.txt](https://easylist-downloads.adblockplus.org/easylistchina+easylist.txt)
- [@AdGuard/DNS-filter](https://kb.adguard.com/en/general/adguard-ad-filters#dns-filter)
- [@PeterLowe/adservers](https://pgl.yoyo.org/adservers)
- [@DanPollock/hosts](https://someonewhocares.org/hosts)
- [@kidonng/scoop-v2ray](https://github.com/kidonng/scoop-v2ray)

## 项目 Star 数增长趋势

[![Stargazers over time](https://starchart.cc/IceCodeNew/v2ray-rules-dat.svg)](https://starchart.cc/IceCodeNew/v2ray-rules-dat)

