# 超魔丸Busters

A Minecraft Modpack for kingstar and his friends.

本仓库同时承担两件事：

- **整合包发布**：[Releases](https://github.com/hxabcd/ChouMowanBusters/releases) 挂 Lite / Standard 两个 zip
- **下载站**：根目录的 `index.html`（单页静态站，无构建）

---

## 目录结构

```
ChouMowanBusters/
├── index.html                  单页下载站（无依赖、无构建）
├── README.md
├── assets/
│   ├── icons/                  OreUI 像素图标 PNG（3 个）
│   └── README.md               素材来源与授权
├── fonts/
│   ├── Cubic_11.woff2          中文像素字体（SIL OFL 1.1）
│   ├── Minecraft-Ten.otf       OreUI 标题字体
│   ├── Minecraft-Seven.otf     OreUI 小标题字体
│   ├── LICENSE.txt
│   └── README.md               字体来源与覆盖率说明
└── .gitignore                  排除 dl/（旧转存）与 *.bak
```

`dl/` 与 `index.html.*.bak` 只存在于本地、**已被 `.gitignore` 排除**：
前者是接入 Release 之前的 129 MB 本地转存，后者是旧版页面备份。
线上部署与仓库都不需要它们。

**部署必须带上 `fonts/` 和 `assets/`**（字体与图标都是本地引用，不走 CDN）；
缺了只会掉回系统字体 / 图标显示空白，功能不受影响。

**UI 采用 [OreUI 设计](https://github.com/Spectrollay-OreUI/OreUI)**（MIT，© 2020 Spectrollay）：
沿用其设计令牌（灰阶 `#1E1E1F → #FFFFFF`、品牌绿 `#3C8527`、状态色）、
2px 实心边框 + 内浮雕高光的视觉规范、Minecraft 系字体与像素图标。
本站是纯静态页、无构建链，因此**没有引入它的 Vite 组件体系**（`oreui-button` 等自定义元素），
只用原生 HTML/CSS 按其规范实现；细节见 `assets/README.md`。

**页面里没有任何写死的版本信息**：版本号、更新日期、体积、SHA-256、下载直链、
更新日志全部来自 GitHub Release，真源只有一个 ——
`https://github.com/hxabcd/ChouMowanBusters/releases`。
加载时请求一次 `releases?per_page=10`，取最新的正式版（跳过 draft / prerelease）。

读不到时**不猜版本**：两个按钮改指 Releases 页面、体积换成指向箭头图标、
校验区与更新日志说明原因，避免给出可能过期的死链。
release 里只缺某一个包时，只有那个按钮降级，其余照常。

**启动器不打包、不转存**，页面直接链到 PCL 官方作者的发布页。
理由：启动器是通用工具（装一次一直用），转存 exe 既会过期也不便更新，
而且 PCL 自带更新，朋友装上后自己会升级。

---

## 页面结构

页面顺序：**怎么装（0）→ 启动器（01）→ 整合包（02）→ 服务器（03）→ 更新日志（04）**，
先看流程，再按编号操作。

| 位置 | 内容 | 体积 |
|---|---|---|
| 0 · 怎么装 | 五步流程概览 | — |
| 01 · 启动器 | 蓝奏云目录（提取码 `PCL2`） | 外链 |
| 02 · 整合包 | **Lite**（推荐） | 2.1 MB（取自 release） |
| | Standard | 65.8 MB（取自 release） |
| 03 · 服务器 | `07f4acdef99b.ofalias.net:56477`，带「复制地址」与「连通性测试」 | — |
| 04 · 更新日志 | 取自 release 说明正文 | — |

### 下载镜像

「整合包」标题下方有一个**镜像下载**开关，默认关闭（所有卡片直连 GitHub 真源）。
打开后右侧出现端点下拉框，当前选中的反代对 **Lite / Standard 全部卡片**同时生效
（共用一份状态）；关掉即恢复直连。默认端点 `gh-proxy.org`。

**校验值不变** —— 实测完整下载后与直链 SHA-256 一致。

| 镜像 | 地址前缀 | 实测 |
|---|---|---|
| gh-proxy.org | `https://gh-proxy.org/` | 206 / Range 支持 / 哈希一致 |
| ghproxy.net | `https://ghproxy.net/` | 206 / Range 支持 / 哈希一致 |
| gh-proxy.com | `https://gh-proxy.com/` | 206 / Range 支持 / 哈希一致 |
| ghfast.top | `https://ghfast.top/` | 206 / Range 支持 / 哈希一致 |

用法就是把 GitHub 原始 URL 直接拼在前缀后面。想增删改 `index.html` 里的 `MIRRORS` 数组即可，
数组第一项就是下拉框的默认选项：

```js
const MIRRORS = [
  { name: "gh-proxy.org", base: "https://gh-proxy.org/" },
  { name: "ghproxy.net",  base: "https://ghproxy.net/" },
  { name: "gh-proxy.com", base: "https://gh-proxy.com/" },
  { name: "ghfast.top",   base: "https://ghfast.top/" }
];
```

> 这些是**第三方公共反代**，可用性不由我们控制：挂了只是那条链路下载失败，不影响直连与
> 其他镜像。当初排除的候选：`ghproxy.cc`（TLS 证书校验失败）、`gh.llkk.cc`（连接超时）。
> 读不到 release（拿不到直链）时整条镜像栏隐藏。

### 服务器连通性测试

- **在线**（绿灯）：MOTD、版本、玩家数、玩家名单，有就显示服务器图标（base64）
- **离线**（红灯）：`ping` 报 `Failed to connect` / DNS 查不到 —— 真的连不上
- **探测异常**（灰灯）：mcsrvstat 单次探测失败（`Unknown problem…`）但服务器其实可能正常，
  与「真离线」区分开；它会把失败结果缓存 300s，命中缓存时文案会提示「稍后重试」
- **查询失败**（灰灯）：12s 超时或状态服务不可达

> 状态行**不重复显示地址**（页面上方已有地址框），只显示版本 / 玩家数 chips；
> 玩家名单单独一行，用胶囊列出。在线数在 chips，名单在下方。

地址从 DOM 读取，不额外硬编码；调试可用 `?server=host:port` 临时覆盖测试目标。
**想重新测试就再点一次「连通性测试」按钮**（面板里没有单独的重测按钮）。

Lite = 仅含 Mod 清单，体积小，需联网下载；Standard = 内置 Mod 资源，避免部分下载问题，
游戏本体仍需联网下载。

PCL 下载渠道：

- 蓝奏云（页面主入口）：`https://ltcat.lanzouv.com/b0aj6gsid` · 提取码 **PCL2**
- 爱发电（页面小字备用入口）：`https://afdian.com/p/0164034c016c11ebafcb52540025c377`

> **不放 GitHub 链接**：`Hex-Dragon/PCL2` 已迁到 `Meloong-Git/PCL`，
> 且它的 release 只挂版本说明、不含 binary（实测 5 个 release 的 `assets` 全为空），
> 指过去也下载不到，只会让人白跑。

> 蓝奏云里最新是 **PCL 正式版 2.13.1.1.zip**（2.5 MB，2026-08-07，分享者就是作者龙腾猫跃）。
> 注意别和 **PCL2-CE**（社区版）搞混 —— 那是另一条产品线，版本号不可比。
> 给普通用户用官方正式版。

PCL 不能自动更新整合包，所以每次更新朋友都要重下整包 ——
**日常发版只推 Lite，2 MB 几秒下完。**

---

## 部署（Cloudflare Pages）

本仓库只存源文件，**由 Cloudflare Pages 直接从 `main` 分支构建发布**：

1. CF Dashboard → Workers & Pages → Create → Pages → Connect to Git → 选 `hxabcd/ChouMowanBusters`
2. 构建配置（纯静态，无需构建步骤）：
   - **Framework preset**：`None`
   - **Build command**：留空
   - **Build output directory**：`/`
3. Save and Deploy；之后每次 push 到 `main` 自动重新部署

站点根目录即仓库根目录，`index.html` 直接被当作首页，`fonts/` 与 `assets/`
按相对路径加载，**不需要改任何配置**。

### 可选：缓存头

在仓库根放 `_headers`（CF Pages 支持），或在 Dashboard → Caching 里配：

```
/index.html
  Cache-Control: public, max-age=300

/fonts/*
  Cache-Control: public, max-age=31536000, immutable

/assets/*
  Cache-Control: public, max-age=31536000, immutable
```

> 字体与图标文件名不含版本号，**升级素材时要么改文件名，要么把这条 immutable 去掉**，
> 否则浏览器会一直用旧缓存。页面文件本身只给 300s，够短。

### 其他托管

同一份文件也能直接丢给 Netlify / Vercel / R2 / OSS 等：传 `index.html`、`fonts/`、`assets/`
即可（`dl/` 不需要，它只在本地且已被 `.gitignore` 排除）。

> 页面里没有需要改的 `BASE` 常量；换仓库只改 `index.html` 里的 `REPO`。
> 跨域方面：`api.github.com` 与 `api.mcsrvstat.us` 都返回 `Access-Control-Allow-Origin: *`，
> 无需任何代理或 CORS 配置。

### 外部服务与配额

- `api.github.com`（页面加载时 1 次）：未登录限 **每小时 60 次 / 每 IP**。
  朋友几个人够用；撞限流时页面自动降级为「指向 Releases 页面」，不会给出过期直链。
- `api.mcsrvstat.us`（**只在点「连通性测试」时**请求，不自动跑）：无公开配额限制，
  响应自带 300s 缓存。所以打开页面本身不会消耗它，也不会拖慢首屏。
  状态查询纯属附加信息，查询失败不影响下载与地址复制。

---

## 每次发新版

1. 打包，命名跟 release tag 和类型走：`Busters-26.4.0-lite.zip`、`Busters-26.4.0-standard.zip`
2. 在 GitHub 建 release，tag 写版本号（如 `26.4.0`），把 zip 作为 asset 传上去
3. 完成 —— 页面下次打开自动显示新版本、新体积、新 SHA-256、新下载链接、新更新日志

**不用改 `index.html`，也不用算哈希。** 页面读取 asset 名末尾的 `-lite.zip` /
`-standard.zip` 来识别两个包（优先精确匹配 `Busters-<tag>-<类型>.zip`），
SHA-256 直接取 GitHub 的 asset `digest` 字段，更新日志取 release 说明正文。

### release 说明的写法

正文即页面的「更新日志」，支持 `- ` 列表与 `**加粗**`。**不显示**的版本与加载器数字
靠正文开头的两行驱动，写就有徽章、不写就没有徽章：

```
Minecraft: 26.4
Fabric Loader: 0.20.0

- 新增 **X**
- 移除 Y
```

> 这两行只喂徽章，不会出现在更新日志里。
> 只发 Lite 也没问题：Standard 的按钮会单独降级指向 Releases 页面，并在页面上说明。

想本地核对哈希：

```powershell
Get-FileHash .\Busters-26.4.0-lite.zip -Algorithm SHA256 | Select-Object -ExpandProperty Hash
```

---

## 本地预览

```powershell
cd D:\MyFiles\Desktop\超魔丸Busters-site
python -m http.server 8099
# 打开 http://127.0.0.1:8099/
```

---

## 备注

- `servers.dat` 已内置，服务器地址 `07f4acdef99b.ofalias.net:56477`，
  装完进游戏服务器列表就有，不用手填。
- 页面上的服务器地址和提取码都带「复制」按钮。
- 已适配手机，朋友用手机点链接也能下。
