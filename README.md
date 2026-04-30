# VPN Rules

这是一个用于维护公司 VPN / 代理直连规则的 Git 仓库，当前规则文件采用 Clash 兼容的 `.list` 格式。

## 目录结构

- `rules/company-direct.list`: 公司内网和相关域名直连规则
- `examples/clash-verge-mixin.yaml`: Clash Verge 可直接参考的 DNS 与规则片段

## 规则内容

当前规则覆盖以下场景：

- `cf.myhexin.com`
- `paas.myhexin.com`
- `unite-passport.myhexin.com`
- `10.0.39.220/32`
- `192.168.0.1/32`
- `10.39.254.0/32`
- `10.10.0.1/32`
- `192.168.51.148/32`
- `10.0.0.0/8`
- `192.168.0.0/16`
- `172.16.0.0/12`

## 使用方式

可将 `rules/company-direct.list` 作为规则集引入你的 Clash 配置，再在规则中引用对应的策略组。

远程规则链接：

`https://raw.githubusercontent.com/OneStepAway/vpn-rules/main/rules/company-direct.list`

示例：

```yaml
rule-providers:
  company-direct:
    type: http
    behavior: classical
    url: https://raw.githubusercontent.com/OneStepAway/vpn-rules/main/rules/company-direct.list
    path: ./ruleset/company-direct.list
    interval: 86400

rules:
  - RULE-SET,company-direct,DIRECT
```

## Clash Verge 配置片段

如果你在 Clash Verge 中遇到公司内网页面无法访问，除了 `DIRECT` 规则外，通常还需要关闭 `fake-ip` 对公司域名的影响。

可参考 [examples/clash-verge-mixin.yaml](/Users/huoju/Desktop/AICoding/vpn-rules/examples/clash-verge-mixin.yaml) 中的配置片段：

```yaml
prepend:
  - 'DOMAIN-SUFFIX,myhexin.com,DIRECT'
  - 'DOMAIN-SUFFIX,unite-passport.myhexin.com,DIRECT'
  - 'DOMAIN-SUFFIX,paas.myhexin.com,DIRECT'
  - 'DOMAIN-SUFFIX,cf.myhexin.com,DIRECT'
  - 'IP-CIDR,10.0.39.220/32,DIRECT'
  - 'IP-CIDR,10.0.0.0/8,DIRECT'
  - 'IP-CIDR,192.168.0.0/16,DIRECT'
  - 'IP-CIDR,172.16.0.0/12,DIRECT'

append: []
delete: []

dns:
  enable: true
  ipv6: false
  enhanced-mode: redir-host
  fake-ip-filter:
    - "*.myhexin.com"
  nameserver:
    - 10.0.39.220
    - 192.168.0.1
    - 10.39.254.0
    - 10.10.0.1
    - 192.168.51.148
```
