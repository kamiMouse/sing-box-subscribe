# 操作说明去看[英文文档](https://github.com/Toperlock/sing-box-subscribe/blob/main/instructions/README.md)，中文文档操作说明不再提供

# 免责声明：sing-box-subscribe.vercel.app域名目前已被其他人占用，与本项目无关。后果自负
![image](https://github.com/Toperlock/sing-box-subscribe/assets/86833913/f9af80bc-f1b7-45dd-a2eb-e26910069f21)

### 使用 `/config/URL` 添加参数符号已修改，从原来的 `/&` 改为 `&`。有问题请提issue，不要打扰 `sing-box`

```
https://xxxxxxx.vercel.app/config/https://xxxxxxsubscribe?token=123456&file=https://github.com/Toperlock/sing-box-subscribe/raw/main/config_template/config_template_groups_rule_set_tun.json
```

```
https://xxxxxxx.vercel.app/config/https://xxxxxxsubscribe?token=123456&file=2
```

本地python执行脚本命令：

```
python main.py
```

或者你可以直接带template_index参数选定模板，0表示第一个模板(no flask不支持此参数)

```
python main.py --template_index=0
```

支持Docker

```
docker build --tag 'sing-box' .
docker run -p 5000:5000 sing-box:latest
```

支持自定义GitHub加速链接（使用参数&gh=1 数字代表使用第一个github加速），默认不加此参数。只有原始GitHub文件链接或者已经使用以下GitHub加速链接才能替换

```
1. "https://gh-proxy.com/",
2. "https://gh.sageer.me/",
3. "https://ghproxy.com/",
4. "https://mirror.ghproxy.com/",
5. "https://cdn.jsdelivr.net",
6. "https://testingcf.jsdelivr.net"
```

### 本地规则集路径说明

本项目模板中引用了本地规则集 `rule_sets/ypc-vpn.srs`（如 `shanghaicang.com.cn` 走 OpenVPN）。

由于 SFA / sing-box 客户端在加载配置时会将配置复制到自身工作目录（如 macOS 上的 `Library/Caches/Working/`），**相对路径 `./rule_sets/ypc-vpn.srs` 会解析失败**。使用前请根据实际存放位置修改为**绝对路径**，例如：

```json
{
  "tag": "ypc-vpn",
  "type": "local",
  "format": "binary",
  "path": "/Users/kami/Documents/ypc/sing-box-subscribe/rule_sets/ypc-vpn.srs"
}
```

若需跨设备使用，建议将 `.srs` 文件上传到可访问的 URL，并改为 `type: remote` 引用。

### 根据已有的qx，surge，loon，clash规则列表自定义规则集[https://github.com/Toperlock/sing-box-geosite](https://github.com/Toperlock/sing-box-geosite)

### wechat规则集源文件写法：
```json
{
  "version": 1,
  "rules": [
    {
      "domain": [
        "dl.wechat.com",
        "sgfindershort.wechat.com",
        "sgilinkshort.wechat.com",
        "sglong.wechat.com",
        "sgminorshort.wechat.com",
        "sgquic.wechat.com",
        "sgshort.wechat.com",
        "tencentmap.wechat.com.com",
        "qlogo.cn",
        "qpic.cn",
        "servicewechat.com",
        "tenpay.com",
        "wechat.com",
        "wechatlegal.net",
        "wechatpay.com",
        "weixin.com",
        "weixin.qq.com",
        "weixinbridge.com",
        "weixinsxy.com",
        "wxapp.tc.qq.com"
      ]
    },
    {
      "domain_suffix": [
        ".qlogo.cn",
        ".qpic.cn",
        ".servicewechat.com",
        ".tenpay.com",
        ".wechat.com",
        ".wechatlegal.net",
        ".wechatpay.com",
        ".weixin.com",
        ".weixin.qq.com",
        ".weixinbridge.com",
        ".weixinsxy.com",
        ".wxapp.tc.qq.com"
      ]
    },
    {
      "ip_cidr": [
        "101.32.104.4/32",
        "101.32.104.41/32",
        "101.32.104.56/32",
        "101.32.118.25/32",
        "101.32.133.16/32",
        "101.32.133.209/32",
        "101.32.133.53/32",
        "129.226.107.244/32",
        "129.226.3.47/32",
        "162.62.163.63/32"
      ]
    }
  ]
}
```
### ⚠️ OpenVPN 内网服务直连规则集 (ypc-vpn)

本项目内置了 `rule_sets/ypc-vpn.srs` 规则集，用于在 TUN 模式下强制 `*.shanghaicang.com.cn` 流量走 OpenVPN 接口（`utun6`）。

**使用前需要根据你的环境修改模板中的规则集路径：**

模板中 `rule_set` 的 `path` 默认使用绝对路径（如 `/Users/kami/Documents/ypc/sing-box-subscribe/rule_sets/ypc-vpn.srs`）。

因为 SFA/SFM 客户端会把配置文件复制到 `Library/Caches/Working/` 下运行，相对路径会失效。请根据你的实际部署位置修改为：

- **本地使用**：改为你本机上的绝对路径
- **远程/服务器部署**：改为远程 URL（如 `https://raw.githubusercontent.com/用户名/sing-box-subscribe/main/rule_sets/ypc-vpn.srs`）+ `type: remote`
- **合入现有规则集**：也可以把 `ypc-vpn.json` 的内容合并到你自己的规则集文件中

配置文件添加源文件规则集：
```
{
  "tag": "geosite-wechat",
  "type": "remote",
  "format": "source",
  "url": "https://raw.githubusercontent.com/Toperlock/sing-box-geosite/main/wechat.json",
  "download_detour": "auto"
}
```

