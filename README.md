<div align="center">

<img src="app/src/main/res/drawable/icon.png" width="110" alt="云析" />

# 云析 YunX · 微云版（Android）

**在原项目基础上增加「腾讯微云分享解析」的构建**

[⬇ Android 版](https://github.com/leluo123/YunX/releases) ｜
[⬇ Windows 版](https://github.com/leluo123/YunX-Desktop/releases/latest) ｜
[上游安卓端](https://github.com/CYQawa/YunX) ｜
[上游桌面端](https://github.com/tidain/YunX-Desktop)

</div>

---

## 关于这个项目

[云析 YunX](https://github.com/CYQawa/YunX) 是一个开源的网盘分享链接解析下载工具，支持夸克 / UC / 迅雷 / 百度 / 139 / 123 / 115 / 光鸭 / 蓝奏云等网盘，以及抖音 / B站 / 小红书等平台的视频解析。

本项目是它的**下游构建**：在原作者代码的基础上，增加了**腾讯微云**这一家网盘的支持，并分别打包成 Android 安装包与 Windows 程序。

代码版权归原作者所有，本项目遵循上游的 **AGPL-3.0** 协议开放。

---

## 两个产物

| 平台 | 从这里下载 | 说明 |
|---|---|---|
| **Android** | [Releases](https://github.com/leluo123/YunX/releases) | `YunX-<版本>-wx.apk`，7 MB 左右 |
| **Windows** | [Releases](https://github.com/leluo123/YunX-Desktop/releases/latest) | `YunX-Desktop-portable-v<版本>-wx.zip`，解压即用 |

本仓库是 Android 端的源码与构建；Windows 端见 [leluo123/YunX-Desktop](https://github.com/leluo123/YunX-Desktop)。两端使用同一套微云解析逻辑。

---

## 本版新增：腾讯微云

| 能力 | 说明 |
|---|---|
| 分享链接解析 | `https://share.weiyun.com/{id}` |
| 目录浏览 | 分享的是文件夹时可以点进去 |
| 下载 | Range 分片并发 + 断点续传 |
| 提取码 | 链接带 `?pwd=` 时自动识别 |
| 登录 | **不需要** |

### 怎么用

「解析」页粘贴分享链接 → 解析 → 列出文件 → 点文件取直链下载。与使用其它网盘一致。

### 两个轻量工具

`tools/` 里另有浏览器脚本与命令行工具，适合临时使用：

- **[`weiyun-via.user.js`](tools/weiyun-via.user.js)** —— 打开微云分享页时右下角出现面板，列出文件并给「直链」「下载」按钮。支持 Via 浏览器与各类油猴管理器。
- **[`weiyun_share.py`](tools/weiyun_share.py)** —— 命令行工具，纯标准库。

安装与原理见 [`tools/README.md`](tools/README.md)。

---

## 相对上游做了哪些改动

改动集中在「接入一家新网盘」需要碰的地方，其余功能、界面、代码保持上游原样。

**新增**：`WeiyunConstants.kt`、`WeiyunApi.kt`、`WeiyunResolveRepository.kt`，以及图标与三处文案资源

**修改**：`ShareLinkParser`（链接识别）、`DownloadPlatform`（平台标识）、`PlatformLogos`（图标映射）、`ResolveScreen`（平台名）、`ResolveViewModel`（分发、凭证、免登录白名单、下载请求头）、`MainScreen`（装配）

**未改动**：Room 数据库、账号体系、登录页、云盘页 —— 微云免登录，这些都用不到；数据库版本与表结构无变化。

构建方面：CI 在没有配置签名证书时会现场生成临时证书（保证能出包），并在构建前校验补丁是否仍然存在；APK 同时上传到 Actions Artifacts 和 Releases。

---

## 安装提示

- 本版使用**临时自签名证书**，每次构建的签名不同，覆盖安装失败时请先卸载旧版。
- 如需稳定升级：在仓库 Settings → Secrets 配置 `KEYSTORE_BASE64` / `KEYSTORE_PASSWORD` / `KEY_ALIAS` / `KEY_PASSWORD`，构建会自动改用你的证书。
- 与上游官方包的签名不同，两者不能互相覆盖安装。

上游的完整功能说明（视频解析、多账号、云端浏览等）保留在 [`README.upstream.md`](README.upstream.md)。

---

## 已知限制

1. 微云**不支持转存到个人盘**（需要登录账号），本版只做分享直链下载。
2. 分享方删除文件但分享链接仍在时，会显示空列表。
3. 需要提取码的分享需手动填写。

---

## 致谢与协议

本项目的代码主要来自：

- **[CYQawa/YunX](https://github.com/CYQawa/YunX)** —— 安卓端与全部网盘 / 视频解析能力
- **[tidain/YunX-Desktop](https://github.com/tidain/YunX-Desktop)** —— 桌面端

感谢两位原作者把项目以 AGPL-3.0 开源出来。本项目对其代码的再分发与修改同样遵循 **AGPL-3.0**，相关源码在上述链接中完整公开。

如果本项目的描述有误，或你对改动有异议，欢迎直接提出，会尽快调整。
