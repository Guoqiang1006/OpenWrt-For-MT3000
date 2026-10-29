# GL-MT3000 中文 OpenWrt 定制固件

适用于 **GL.iNet Beryl AX / GL-MT3000** 的纯 OpenWrt 定制系统，提供中文网页管理、首次配置向导和预装插件。

> 当前为 **v1.0.0 预发布版**：完整镜像已通过构建及文件校验；部分功能有单台设备及在线更新验证记录，但这份最新完整镜像尚未再次整包刷入。手机 USB、真实酒店认证、公网 IPv6、WPA3 等场景仍需对应环境实测。

## 下载

进入 **[v1.0.0 发布页面](https://github.com/Guoqiang1006/OpenWrt-For-MT3000/releases/tag/v1.0.0)**，展开 Assets 下载附件。

- **整包下载**：`MT3000-Firmware-and-Manual-v1.0.0.zip`，含固件、12 页 PDF、可编辑 Word、说明及校验值。
- **单独固件**：`openwrt-25.12.6-zh-openclash-mediatek-filogic-glinet_gl-mt3000-squashfs-sysupgrade.bin`。
- [PDF 产品功能手册](MT3000-User-Manual-v1.0.pdf) · [Word 可编辑手册](MT3000-User-Manual-v1.0.docx)。
- [SHA256 校验值](SHA256SUMS) · [组件清单](packages.manifest) · [版本说明与验证范围](RELEASE-NOTES.md)。

固件放在 Releases 附件中，本仓库 Git 历史不保存固件二进制。

## 主要功能

| 功能 | 内容 |
| --- | --- |
| 中文管理 | 中文 LuCI、Argon 主题、Bootstrap 备用主题 |
| 首次配置向导 | 管理密码、地区与时区、主 Wi-Fi、WAN、IPv6、DNS 故障策略 |
| 路由器首页 | 网络、时间、服务、内存和温度状态；手动 DNS/HTTPS 检查 |
| OpenClash | 0.48.002，预装 Mihomo 1.19.33 ARM64；自行提供配置或订阅 |
| 国内网站直连 | 可通过规则模式配置国内直连和按需代理；实际行为由规则决定 |
| 无线地区切换 | 247 项中文地区名称；中国大陆、香港、日本、韩国、泰国、新加坡、澳大利亚、美国、加拿大快捷按钮 |
| AdGuard Home | 0.107.79，可选 DNS 过滤、串联及故障恢复策略 |
| 旅行上网 | Travelmate 外部 Wi-Fi 接入；预装 Android/iPhone USB 共享组件 |
| 其他连接 | WireGuard 与 OpenVPN 组件及管理入口 |
| 实体开关 | 侧面开关控制 OpenClash，指示灯反馈运行状态 |

预装不等于默认启用，也不等于所有场景都已验证。固件不含线路、订阅、私人账户或预设无线密码。提供文件与说明，不含远程安装配置服务。

## 安装前与首次使用

1. 核对设备必须为匹配硬件版本的 **GL-MT3000**；不能用于其他型号。国行/海外标签不能代替型号及硬件核对。
2. 先保存现有配置备份，准备稳定电源和 LAN 网线。
3. 从原厂 GL.iNet 系统迁移时，**取消保留设置**；不要把原厂配置备份恢复到纯 OpenWrt。
4. 按设备支持的升级入口上传匹配的 sysupgrade 镜像。系统拒绝文件或提示型号不匹配时停止，不能强制绕过检查。
5. 等待刷写及重启完成，电脑通过 LAN 自动获取地址，打开 **http://192.168.8.1**，使用 `root` 用户进入并设置管理员密码。
6. **全新安装的 Wi-Fi 默认关闭**。在“系统 → 首次配置向导”设置实际地区、时区、无线名称及密码，再启用无线。
7. 检查基础上网正常后，再配置 OpenClash、AdGuard Home 等可选功能。

全新安装默认无线地区 AU、时区 Australia/Sydney，使用前应按实际所在地设置。OpenClash、AdGuard Home 默认关闭。兼容的本系列 OpenWrt 升级通常可保留设置，但应先备份并核对版本说明；保留设置后管理地址等可能沿用原值。

这是第三方纯 OpenWrt 系统，**不保留原厂 GL.iNet App、GoodCloud 或原厂管理界面**。手机可通过浏览器管理。

### 手机 USB 共享

手机用支持数据传输的线接 **USB-A**；路由器 **USB-C 继续供电**。Android 开启 USB 网络共享，iPhone 开启个人热点并确认信任。已集成常见驱动，但尚未建立实测机型白名单，不能保证全部手机即插即用。有线 WAN 默认优先，当前未集成完整的互联网故障自动切换服务。

### 文件校验

整包解压后，在该文件夹执行：

```sh
# macOS
shasum -a 256 -c SHA256SUMS

# Linux
sha256sum -c SHA256SUMS
```

固件 SHA256：

```text
7bded5ff1548ee6b3d76bd2c56b582c2073bbf050073d2691a8413ff7b5b321b
```

校验值一致只说明文件完整，仍需核对设备型号。

## 恢复

无法正常启动时参照 [GL.iNet 官方 U-Boot 恢复说明](https://docs.gl-inet.com/router/en/4/faq/debrick/)，使用厂商为 MT3000 指定的恢复文件。普通 sysupgrade、厂商升级包和 U-Boot 恢复文件不能混用。恢复出厂设置只会清除本系统配置，不会自动恢复原厂 GL.iNet 固件。

## 项目与组件

基于 [OpenWrt](https://openwrt.org/)、[OpenClash](https://github.com/vernesong/OpenClash)、[Mihomo](https://github.com/MetaCubeX/mihomo)、[AdGuard Home](https://github.com/AdguardTeam/AdGuardHome) 和 [Argon](https://github.com/jerrykuku/luci-theme-argon) 等项目集成。各组件权利与许可证归各自项目，不因本仓库发布而改变。
