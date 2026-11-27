<div align="center">

<img src="app/src/main/res/drawable/icon.png" width="120" alt="云析 YunX" />

# 云析 YunX · Android（含腾讯微云）

**在 YunX 安卓版基础上增加「腾讯微云分享解析」的构建**

[⬇ 下载最新版](../../releases) · [原项目](https://github.com/CYQawa/YunX) · [PC 端](https://github.com/tidain/YunX-Desktop) · [上游 README 原文](README.upstream.md)

</div>

---

## 致谢与立场

这个项目的**代码、设计和绝大部分功能都不是我做的**。

- 感谢 **[CYQawa](https://github.com/CYQawa)** 写出安卓端与整套网盘 / 视频解析能力
- 感谢 **[tidain](https://github.com/tidain)** 写出桌面端

两者均以 **AGPL-3.0** 开源。本仓库是它们的下游：**只新增了「腾讯微云」这一家网盘的支持**，其余功能、代码、UI 全部来自原作者。

> **云析永远免费开源。** 如果你是在任何地方「花钱买到」的，说明你被骗了，请退款。

---

## 本版新增：腾讯微云

上游支持夸克 / UC / 迅雷 / 百度 / 139 / 123 / 115 / 光鸭 / 蓝奏云 / 蓝奏云优享版，**不含微云**。本版补上，且**免登录**。

| 能力 | 说明 |
|---|---|
| 分享链接解析 | `https://share.weiyun.com/{id}` |
| 目录浏览 | 分享的是文件夹时可以点进去 |
| 下载 | Range 分片并发 + 断点续传（微云 CDN 支持 `Accept-Ranges`） |
| 提取码 | 链接带 `?pwd=` 自动识别 |
| 登录 | **不需要** |

### 怎么用

1. 「解析」页粘贴微云分享链接 → 开始解析
2. 列出文件后点文件 → 取直链下载

### 也给了浏览器脚本和命令行

- [`tools/weiyun-via.user.js`](tools/weiyun-via.user.js) —— Via / 油猴脚本，打开分享页右下角出面板（不想装 App 用这个）
- [`tools/weiyun_share.py`](tools/weiyun_share.py) —— 纯标准库 CLI

装法见 [`tools/README.md`](tools/README.md)。

---

## 本仓库相对上游改了什么

**新增 3 个 Kotlin 文件**：`WeiyunConstants.kt` / `WeiyunApi.kt` / `WeiyunResolveRepository.kt`
**新增资源**：`logo_weiyun.png`、`platform_weiyun` / `rs_platform_weiyun` / `rs_short_weiyun`

**改动 8 处**：

| 文件 | 改动 |
|---|---|
| `data/network/ShareLinkParser.kt` | 枚举加 `WEIYUN` + 链接正则 + parse 分支 |
| `data/download/DownloadPlatform.kt` | 加 `WEIYUN` 常量 |
| `ui/items/PlatformLogos.kt` | 穷尽 `when` 补图标分支（不加就编译失败） |
| `ui/screens/ResolveScreen.kt` | 平台短名 |
| `ui/viewmodel/ResolveViewModel.kt` | 注入 / 凭证 `null` / `currentRepo()` 分发 / 平台名 / 免登录白名单 / 默认目录 / 下载请求头 |
| `ui/MainScreen.kt` | 装配 |

**不碰**：Room 表、账号、登录页、云盘页 —— 微云免登录，`AppDatabase` 仍是 version 24，无迁移。

**CI**（`.github/workflows/build-apk.yml`）：

| 改动 | 原因 |
|---|---|
| 签名步骤改成「有 Secrets 用正式证书，没有就现场自签名」 | 上游要求 4 个 Secrets，缺一个直接 `exit 1`；本 fork 没有 ⇒ 以前必然失败 |
| 新增「校验微云补丁仍在」 | 防止补丁被同步/重构吃掉却仍出包 |
| 新增「Publish APK to Release」 | 上游只传 Actions Artifacts；本 fork 额外发 Release，方便直接下载 |

---

## 下载与安装

去 [**Releases**](../../releases) 下载 `YunX-<版本>-wx.apk`。

- 本仓库用的是**临时自签名证书**，所以**每次构建签名不同** ⇒ 覆盖安装会失败，先卸载旧版再装。
- 想稳定升级：在仓库 Settings → Secrets 里配好 `KEYSTORE_BASE64` / `KEYSTORE_PASSWORD` / `KEY_ALIAS` / `KEY_PASSWORD`，workflow 会自动改用你的正式证书。
- 与上游 `CN=Modder Hub` 签名的包**不能互相覆盖安装**。

其余功能（视频解析、多账号、云端浏览等）见[上游文档](README.upstream.md)。

---

## 微云支持的边界

1. **不支持转存到微云个人盘**（需要登录账号），只做分享直链下载。
2. **空壳分享**会显示空列表。
3. **需要提取码**的分享手动填。

---

## 授权

[AGPL-3.0](./LICENSE)。代码版权归 CYQawa / tidain；本仓库的改动同样以 AGPL-3.0 开放。
