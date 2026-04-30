# VPN Rules

这是一个用于维护公司 VPN / 代理直连规则的 Git 仓库，当前规则文件采用 Clash 兼容的 `.list` 格式。

## 目录结构

- `rules/company-direct.list`: 公司内网和相关域名直连规则

## 规则内容

当前规则覆盖以下场景：

- `cf.myhexin.com`
- `paas.myhexin.com`
- `unite-passport.myhexin.com`
- `10.0.39.220/16`
- `10.0.0.0/8`
- `192.168.0.0/16`
- `172.16.0.0/12`

## 使用方式

可将 `rules/company-direct.list` 作为规则集引入你的 Clash 配置，再在规则中引用对应的策略组。

示例：

```yaml
rule-providers:
  company-direct:
    type: file
    behavior: classical
    path: ./rules/company-direct.list

rules:
  - RULE-SET,company-direct,DIRECT
```
