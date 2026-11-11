<!-- 中文说明开始 -->

# 🇨🇳 3x-ui 中文详细说明

<!-- 徽章区开始 -->
<p align="center">
  <a href="https://github.com/aiwozhonghua81/3x-ui"><img alt="GitHub" src="https://img.shields.io/badge/GitHub-aiwozhonghua81-181717?style=flat-square&logo=github&logoColor=white"></a>
  <img alt="最近提交" src="https://img.shields.io/github/last-commit/aiwozhonghua81/3x-ui?style=flat-square&label=%E6%9C%80%E8%BF%91%E6%8F%90%E4%BA%A4">
  <img alt="主语言" src="https://img.shields.io/github/languages/top/aiwozhonghua81/3x-ui?style=flat-square&label=%E4%B8%BB%E8%AF%AD%E8%A8%80">
  <img alt="星标" src="https://img.shields.io/github/stars/aiwozhonghua81/3x-ui?style=flat-square&label=%E6%98%9F%E6%A0%87">
  <img alt="仓库体积" src="https://img.shields.io/github/repo-size/aiwozhonghua81/3x-ui?style=flat-square&label=%E4%BB%93%E5%BA%93%E4%BD%93%E7%A7%AF">
  <img alt="许可证" src="https://img.shields.io/github/license/aiwozhonghua81/3x-ui?style=flat-square&label=%E8%AE%B8%E5%8F%AF%E8%AF%81">
</p>
<p align="center">
  <img alt="中文说明" src="https://img.shields.io/badge/%E4%B8%AD%E6%96%87%E8%AF%B4%E6%98%8E-%E5%B7%B2%E5%AE%8C%E6%95%B4%E8%A6%86%E7%9B%96-brightgreen?style=flat-square">
  <img alt="给新手看" src="https://img.shields.io/badge/%E7%BB%99%E6%96%B0%E6%89%8B%E7%9C%8B-%E4%B8%8D%E7%94%A8%E6%87%82%E4%BB%A3%E7%A0%81-blue?style=flat-square">
  <img alt="配图" src="https://img.shields.io/badge/%E9%85%8D%E5%9B%BE-%E5%AE%9E%E6%8B%8D%E6%88%AA%E5%9B%BE%E4%B8%8E%E5%A4%B4%E5%9B%BE-ff69b4?style=flat-square">
</p>
<p align="center">
  <img src="media/01-overview-dark.png" alt="3x-ui 头图" width="880">
</p>
<!-- 徽章区结束 -->

> 这一节是为中文读者写的完整说明，放在最前面，方便第一次接触这个项目的人直接看懂。
> 往下滚可以看到**上游项目的原始 README（英文原文）**，内容一个字都没有删改。

---

## 🧐 一、这是什么

**一句话：一个网页控制面板，用来在自己的服务器上一键管理各种代理协议节点，不用手动敲命令改配置文件。**

展开说：

| 概念 | 大白话解释 |
| --- | --- |
| **代理协议** | 一种让流量「换个通道走」的技术方案。常见的有 VLESS、VMess、Trojan、Shadowsocks、WireGuard、Hysteria2 等，每种协议有自己擅长的场景（抗封锁、速度、兼容性等）。 |
| **Xray-core** | 真正负责转发流量的内核程序。相当于「发动机」。 |
| **3x-ui** | 给这台发动机配的一块「仪表盘 + 遥控器」。你用浏览器点点鼠标，它帮你把发动机参数调好、启动起来、看着运行状态。 |
| **节点（Inbound）** | 面板里的一条配置项 = 一个对外开放的入口。别人（你的设备或朋友）连上这个入口，就能通过你的服务器上网。 |
| **客户端（Client）** | 挂在某个节点下面的一个「用户账号」。同一个节点可以挂多个客户端，各自独立统计流量、设有效期。 |

**不装这个面板，你得干什么？**

手动改 Xray 的 JSON 配置文件 → 手写 inbound/outbound 规则 → 手写 mTLS 证书配置 → 改端口要停服务 → 想看谁用了多少流量得自己解析日志 → 想加个用户还得手动算 UUID → 出问题了只能对着 log 猜。

**装了这个面板，你得干什么？**

浏览器打开一个网页 → 填几个下拉框 → 点「添加」→ 把生成的订阅链接发给客户端 → 完事。

---

## 👥 二、适合谁用 / 能解决什么问题

### 典型使用场景

1. **自己有 VPS（云服务器），想给多台设备提供翻墙/代理服务**
   一台 1 核 512M 的小机器就能跑；装完 5 分钟出结果。

2. **想给朋友 / 家人用**
   在面板里加好客户端，直接生成「订阅链接」发过去。对方导入 Clash / v2rayN / Shadowrocket 就能用，不用手抄配置。

3. **需要精细的流量和权限管理**
   每个客户端单独限额（GB 数）、单独设到期时间、单独限制同时在线的 IP 数、单独限制设备台数（HWID）。

4. **手里有一堆服务器，想统一管**
   「多节点」功能：一个面板管多台服务器，可以把某条 inbound 配置「克隆」到另一台机器上。

5. **需要给订阅地址套一层外壳**
   内置订阅服务器会按客户端的 User-Agent 自动判断对方要什么格式：

   | 客户端类型 | 拿到的订阅格式 |
   | --- | --- |
   | Clash / Clash Verge / Mihomo | YAML（Clash 配置） |
   | v2rayN / v2rayNG / Shadowrocket | Base64 通用订阅 |
   | 其他 / 脚本抓取 | 原始 JSON 或明文链接列表 |

6. **想在手机上用**
   面板本身是 PWA（渐进式网页应用，类似 App），可以「添加到主屏幕」，用起来跟 App 差不多。

### 它**不**适合谁

- **完全不懂服务器的小白**：虽然安装只要一行命令，但出问题时（防火墙、端口被占、证书申请失败）还是需要基本的 Linux 常识。
- **要做企业级多租户 SaaS**：这是个人使用工具，不是商业化多租户平台。
- **要在生产环境跑关键业务**：上游 README 明确写了「仅供个人使用，请勿用于非法用途或生产环境」。

---

## ⚙️ 三、核心功能

| # | 功能 | 大白话说明 |
| --- | --- | --- |
| 1 | **多协议入站（Inbound）** | 一个面板里同时开 VLESS、VMess、Trojan、Shadowsocks、WireGuard、AmneziaWG、TUIC v5、Hysteria2、MTProto、HTTP、SOCKS（Mixed）、Dokodemo-door/Tunnel、TUN 共 13 种入口。 |
| 2 | **多种传输方式与加密** | TCP(Raw)、mKCP、WebSocket、gRPC、HTTPUpgrade、XHTTP 六种传输方式，配合 TLS / XTLS / REALITY 三种安全方案。REALITY 是目前抗封锁能力最强的一种。 |
| 3 | **多端口复用（Fallback）** | 一个 443 端口同时对外提供 VLESS 和 Trojan 两种服务，靠 SNI（握手时客户端声明的域名）分流。这样既省端口，又不容易被扫到。 |
| 4 | **每客户端独立管理** | 流量配额、到期日、IP 数限制（还能设置「豁免 IP」，比如你自己家的宽带不算）、设备台数限制（HWID）、自动续费周期、实时在线状态。 |
| 5 | **一键生成订阅 / 分享链接 / 二维码** | 不用手抄 UUID 和端口，点一下直接出链接，扫码即用。 |
| 6 | **流量统计** | 按入站、按客户端、按出站三个维度分别统计，还能手动重置。 |
| 7 | **多节点（Multi-node）** | 一个面板同时管理多台服务器，支持把某条 inbound 配置克隆到其他节点上批量部署。 |
| 8 | **出站与路由规则** | 内置 WARP、NordVPN、PIA 出口，支持自定义路由规则、负载均衡（balancer）、出站代理链（proxy chain）。规则编辑器里能直接浏览 geosite / geoip 分类。 |
| 9 | **内置订阅服务器** | 面板自己就是一个订阅服务器，支持 raw / JSON / Clash 三种输出，还能换订阅页面模板。 |
| 10 | **Telegram / Discord 机器人** | 在手机上用 Telegram 远程查看流量、开关服务、接收告警，不用登录网页面板。 |
| 11 | **RESTful API + API Token** | 带权限范围、可设置过期时间的访问令牌，面板里自带 API 文档页，方便写脚本自动化运维。 |
| 12 | **灵活存储后端** | 默认用 SQLite（一个文件），也可以换成 PostgreSQL（客户端多或多节点时推荐）。 |
| 13 | **Fail2ban 联动** | 客户端超出 IP 数量限制时，用 iptables 真的把它封掉，而不是只在面板上记一笔。 |
| 14 | **多语言界面 + 深浅色主题** | 面板界面可切换多种语言（包含简体中文和繁体中文），支持深色 / 浅色主题跟随系统。 |

---

## 🚀 四、怎么快速上手

### 4.1 一键安装（Linux，最常用）

**第 1 步：执行安装命令**

```bash
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh)
```

**这条命令干了什么？** 它会下载安装脚本，然后依次：检测你的系统发行版和架构 → 安装 Xray-core 内核 → 下载 3x-ui 本体 → 注册成 systemd 服务（开机自启）→ 生成随机的管理面板账号密码和访问路径 → 把这些信息打印在屏幕上。

> **为什么要用 `bash <(curl ...)` 这种写法？**
> `<(...)` 是 bash 的「进程替换」语法，意思是「先执行右边的命令，把它的输出当成一个临时文件」。所以整条命令实际上是：下载脚本 → 立刻用 bash 执行它 → 执行完临时文件自动删除。一行搞定下载和执行。

**第 2 步：记下屏幕输出的三个关键信息**

安装完会打印类似这样的内容（**具体值每次都不一样，以你屏幕上的为准**）：

```
Username: <随机生成的用户名>
Password: <随机生成的密码>
Web Base Path: <随机生成的路径>
Panel Port: <随机生成的端口>
```

**第 3 步：打开面板**

浏览器访问 `http://<你的服务器IP>:<面板端口><Web Base Path>`，用上面记下的账号密码登录。

**第 4 步：添加一个入站（节点）**

在面板左侧「入站列表」→ 点「添加入站」→ 选协议（新手建议 VLESS）→ 传输方式选 TCP 或 Raw，Xray 新版本建议配 REALITY → 保存。

**第 5 步：加客户端、拿订阅链接**

在刚建的入站下面点「+」加客户端，填个备注名 → 保存 → 点客户端那一行的「分享链接 / 二维码」→ 复制订阅地址 → 导入到 Clash / v2rayN 等客户端。

### 4.2 安装指定版本

想装固定版本（比如 v3.7.0），在命令后面加版本号：

```bash
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh) v3.7.0
```

### 4.3 安装开发版（滚动更新版）

想装每个 commit 都更新的最新版（不是正式发布版）：

```bash
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh) dev-latest
```

> **dev-latest 和正式版有什么区别？**
> 正式版（v3.x.x）是打过标签的稳定版本，dev-latest 是从 main 分支每次提交自动打的预发布包。dev 更新更快但可能踩到还没修完的 bug。

### 4.4 免交互安装（云初始化 / 批量部署）

想让服务器装完完全不问你任何问题（比如在 Hetzner、AWS、Vultr 上批量开机器），设置 `XUI_NONINTERACTIVE=1`：

```bash
export XUI_NONINTERACTIVE=1
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh)
```

装完随机凭证会写到 `/etc/x-ui/install-result.env`（权限 600，只有 root 能读）：

```bash
sudo cat /etc/x-ui/install-result.env
```

仓库里 `deploy/cloud-init/cloud-init.yaml` 是现成的 cloud-init user-data 模板，直接贴到云厂商的「启动脚本」里就能用。

### 4.5 日常运维：x-ui 管理菜单

装完之后，命令行里输入 `x-ui` 就能打开一个 29 项的交互式菜单：

```bash
x-ui
```

菜单里常用的几项：

| 菜单号 | 功能 | 说明 |
| --- | --- | --- |
| 1 | Install | 安装 |
| 2 | Update | 更新到最新正式版 |
| 3 | Update to Dev Channel | 更新到开发版 |
| 7 | Reset Username & Password | **忘记密码时用这个重置** |
| 8 | Reset Web Base Path | 忘了面板网址路径时用 |
| 10 | Change Port | 改面板端口 |
| 11 | View Current Settings | 查看当前所有配置 |
| 12 / 13 / 14 | Start / Stop / Restart | 启停面板服务 |
| 15 | Restart Xray | **只重启 Xray 内核**（不重启面板，改完 inbound 之后用这个更快） |
| 16 | Check Status | 看服务运行状态 |
| 17 | Logs Management | 日志管理 |
| 18 / 19 | Enable / Disable Autostart | 开关开机自启 |
| 20 / 21 | SSL Certificate Management / Cloudflare SSL | 证书管理 |
| 22 | IP Limit Management | IP 限制（Fail2ban）管理 |
| 23 | Firewall Management | 防火墙管理 |
| 25 | PostgreSQL Management | 数据库后端管理 |
| 26 | Enable BBR | 开启 BBR 拥塞控制（提升 TCP 速度） |
| 27 | Update Geo Files | 更新 geosite / geoip 规则库 |
| 28 | Speedtest by Ookla | 测服务器网速 |

> **为什么「重启 Xray」和「重启面板」要分开？**
> 面板是管理界面，Xray 是干活的发动机。改了入站配置只需要让 Xray 重新加载配置，重启面板会让浏览器掉线、还得重新登录，属于没必要的小麻烦。

### 4.6 Docker 部署

仓库根目录有现成的 `docker-compose.yml`：

```bash
# 1. 先把要挂载的目录建好（Windows PowerShell 用 mkdir）
mkdir -p db cert acme

# 2. 启动（默认 SQLite）
docker compose up -d

# 3. 看日志拿初始账号密码
docker compose logs -f
```

默认端口映射是 `2053:2053`（宿主机 2053 → 容器 2053）。

**要用 PostgreSQL 的话**：

```bash
# 编辑 docker-compose.yml，把 XUI_DB_TYPE 和 XUI_DB_DSN 两行的注释去掉
docker compose --profile postgres up -d
```

**用 `docker run` 而不是 compose 时必须手动加权限**：

```bash
docker run -d --cap-add=NET_ADMIN --cap-add=NET_RAW -p 2053:2053 \
  -v $(pwd)/db:/etc/x-ui/ \
  ghcr.io/mhsanaei/3x-ui
```

> **为什么一定要加 `--cap-add=NET_ADMIN --cap-add=NET_RAW`？**
> Fail2ban 靠 iptables 封 IP，而 iptables 改内核网络规则需要 `NET_ADMIN` 权限。`docker-compose.yml` 里已经用 `cap_add` 加好了，但你用 `docker run` 就得自己加。不加的话：封禁动作会被写进 fail2ban 日志、状态页里也显示「已封」，但**实际上一个 IP 都没被封掉**——这是个很常见的「以为生效了其实没生效」的坑。

### 4.7 从 SQLite 迁移到 PostgreSQL

```bash
# 1. 迁移数据（源库默认读 /etc/x-ui/x-ui.db）
x-ui migrate-db --dsn "postgres://xui:你的密码@127.0.0.1:5432/xui?sslmode=disable"

# 2. 改 /etc/default/x-ui 里的环境变量
#    XUI_DB_TYPE=postgres
#    XUI_DB_DSN=postgres://xui:你的密码@127.0.0.1:5432/xui?sslmode=disable

# 3. 重启
systemctl restart x-ui
```

原来的 SQLite 文件不会被删。**确认新库跑正常了之后**，你再手动删。

### 4.8 从源码构建（开发者）

```bash
# 后端 + 前端一起构建（需要 Go 1.27.1+ 和 Node 24+）
make build

# 只跑后端测试
make test-go

# 只跑前端测试
make test-fe

# 跑完整的本地 CI 门禁（代码生成检查 + 两个 linter + 格式检查 + 类型检查 + 两套测试 + 完整构建 + Storybook 编译检查）
make verify

# 看所有可用 make 目标
make help
```

前端单独开发：

```bash
cd frontend
npm install
npm run dev        # 本地起 Vite 开发服务器
npm run gen        # 从 Go 源码重新生成 Zod schema + OpenAPI
```

> **为什么要先跑 `make gen` / `npm run gen`？**
> 前端的类型定义（Zod schema）和 OpenAPI 文档是从 Go 源码自动生成的。如果 Go 代码改了但没重新生成，前端类型就对不上，`make gen-check` 在 CI 里会直接失败。

---

## 🩺 五、常见问题 / 注意事项

### 5.1 安全相关（最重要）

| 问题 | 说明 |
| --- | --- |
| **别把面板端口直接暴露在公网上** | 面板本身是管理后台，暴露出去等于把钥匙挂在门上。正确做法：用防火墙（`ufw` / `firewalld`）只放行特定 IP，或者放在 Nginx/Caddy 后面加 HTTPS，或者用 SSH 端口转发本地访问。 |
| **一定要配 HTTPS** | 面板走明文 HTTP 时，登录密码在网络上是裸奔的。面板菜单 20 / 21 项可以申请 Let's Encrypt 证书。 |
| **随机初始凭证一定记住** | 安装时生成的是随机用户名 + 随机密码 + 随机路径。忘了用 `x-ui` 菜单第 7、8 项重置。 |
| **订阅链接 = 完整权限凭证** | 谁拿到订阅链接，谁就能用你的节点。别发到公开的地方。 |
| **官方声明仅供个人使用** | 上游 README 明确写了：请勿用于非法用途或生产环境。 |

### 5.2 安装 / 运行类

| 现象 | 原因和解决办法 |
| --- | --- |
| **装完 8080/2053 端口打不开** | 三种可能：① 云厂商有「安全组」，得去控制台放行入站端口；② 服务器本机防火墙拦了；③ 你用的不是安装时打印的那个端口和路径。 |
| **`x-ui` 命令提示找不到** | 安装脚本会往 `/usr/local/bin/` 放 `x-ui` 软链。检查 `echo $PATH` 里有没有这个目录，或者直接用 `/usr/local/x-ui/x-ui.sh`。 |
| **Docker 里看不到面板** | `docker-compose.yml` 把数据挂到 `./db/`、`./cert/`、`./acme/` 三个目录，这三个目录必须先手动建好，否则 Docker 会以 root 身份创建它们，后面面板写入时会遇到权限问题。 |
| **装完 CPU 占用很高** | 面板默认开了 Go 的 `GOGC=75` 主动更频繁地回收内存，牺牲一点 CPU 换更低的内存占用。内存特别紧张的小机器可以在 `docker-compose.yml` 里调 `XUI_GOGC`。 |
| **改了配置但没生效** | 大部分情况只需要 `x-ui` → 第 15 项「Restart Xray」，不用重启整个面板。 |
| **内核版本太老编译不过** | 项目需要 Go 1.27.1+（`go.mod` 里写的 `go 1.27.1`），Node 24+（`frontend/package.json` 的 `engines`，仓库里的 `.nvmrc` 也写的 24）。 |

### 5.3 使用 / 客户端类

| 问题 | 说明 |
| --- | --- |
| **客户端导入订阅显示「无节点」** | 先在浏览器里直接打开订阅链接看看返回什么。如果返回 404，多半是 Web Base Path 记错了。 |
| **同一个端口开多个协议（Fallback）** | Fallback 的分流依据是 SNI，所以**不同协议必须用不同的 SNI 值**，否则会互相串流导致握手失败。 |
| **REALITY 配置对但连不上** | REALITY 对端要求：目标域名（dest）必须是你**真的能访问**的第三方网站（最好是同国家、同 TLS 指纹特征的），公网服务端不能和 dest 是同一个 IP。 |
| **限 IP 数用 Fail2ban 封禁没生效** | 99% 是 Docker 缺 `NET_ADMIN` 权限，见 4.6 节的说明。 |
| **订阅页显示的用量和面板对不上** | 订阅页显示的是「按当前时间窗口推算」的估算值，面板里是精确统计值。以面板为准。 |
| **开启隧道健康监控后连接全断** | `XUI_TUNNEL_HEALTH_MONITOR` 会在探测连续失败 3 次后重启 Xray，而重启 Xray 会踢掉所有已连接客户端。生产环境用之前先想清楚。 |

### 5.4 数据库类

| 场景 | 建议 |
| --- | --- |
| **个人用、客户端少于 50 个** | 用默认 SQLite，零配置，最省心。 |
| **客户端多、或者多节点共用一个库** | 换 PostgreSQL。SQLite 是单文件写锁模型，多节点并发写会撞车。 |
| **想回滚到旧版本** | 先备份 `/etc/x-ui/x-ui.db`。面板每次升级都会做数据库迁移，向下不兼容。 |

---

## 🗂️ 六、目录结构速览

```text
3x-ui/
├── main.go                 # 程序入口：解析命令行参数（run / migrate-db / setting）
├── install.sh              # 一键安装脚本（安装时通过 curl 下载执行）
├── update.sh               # 一键更新脚本
├── x-ui.sh                 # 管理菜单脚本（`x-ui` 命令实际执行的就是它）
├── x-ui.service.*          # systemd 服务单元文件（debian / rhel / arch 三种）
├── Dockerfile              # 官方镜像构建文件
├── docker-compose.yml      # 官方 compose 配置（默认 SQLite，带 postgres profile）
├── DockerEntrypoint.sh     # 容器入口脚本
├── DockerInit.sh           # 容器初始化脚本
├── .env.example            # 所有 XUI_* 环境变量的完整说明（开发/生产两用）
├── Makefile                # 统一任务入口（make help 看列表）
├── CLAUDE.md               # 给 AI 编码助手看的项目开发约定
├── CONTRIBUTING.md         # 贡献指南
├── go.mod / go.sum         # Go 依赖清单
├── internal/               # ★ 后端核心代码
│   ├── web/                #   Web 服务层：路由、控制器、中间件、会话、WebSocket
│   ├── xray/               #   Xray-core 进程管理：配置生成、进出站 API、流量统计
│   ├── database/           #   数据库层：模型定义、迁移、种子数据、SQLite/PG 兼容层
│   ├── config/             #   配置层：全局配置读写、版本号
│   ├── sub/                #   订阅服务：raw / JSON / Clash 输出、分享链接生成
│   ├── amneziawg/          #   AmneziaWG 内置实现（用户态网络栈，免内核模块）
│   ├── amneziawgnet/       #   AmneziaWG 网络栈适配
│   ├── tuic/               #   TUIC v5 sidecar（QUIC 协议实现）
│   ├── mtproto/            #   MTProto 代理（FakeTLS、广告标签、配额）
│   ├── pia/                #   PIA (Private Internet Access) 出站支持
│   ├── crypto/             #   加密工具
│   ├── eventbus/           #   事件总线（面板各模块间的消息通知）
│   ├── logger/             #   日志模块
│   ├── tunnelmonitor/      #   隧道健康监控（可选的看门狗）
│   └── util/               #   通用工具函数
├── frontend/               # ★ 前端（React 19 + Ant Design 6 + Vite 8）
│   ├── src/                #   页面、组件、hooks、i18n（多语言）、生成的 schema
│   ├── public/             #   静态资源，含生成的 openapi.json
│   ├── package.json        #   前端依赖和脚本定义
│   └── vite.config.js      #   Vite 构建配置
├── docs/                   # ★ 文档站源码（Next.js + fumadocs）
│   ├── content/            #   MDX 文档正文（安装、运维、API 参考等）
│   └── app/                #   文档站页面
├── deploy/                 # 部署辅助
│   ├── cloud-init/         #   cloud-init user-data 模板（免交互安装）
│   ├── marketplace/hetzner/ #  Hetzner Cloud 部署说明
│   └── test/               #   容器冒烟测试脚本
├── tools/openapigen/       # 代码生成器：从 Go 源码生成 OpenAPI / JSON Schema / Zod
├── media/                  # README 用到的截图和徽章图片
├── windows_files/          # Windows 版服务相关文件
└── .github/workflows/      # CI/CD：ci / docker / release / smoke / codeql 等
```

**怎么看这些目录？**

| 你想做的事 | 该看哪里 |
| --- | --- |
| 改面板界面 | `frontend/src/` |
| 加一个新的协议支持 | `internal/xray/` |
| 改订阅链接的输出格式 | `internal/sub/` |
| 改数据库字段 | `internal/database/model/` + 迁移文件 |
| 看 API 文档 | 面板内「API 文档」页，或 `docs/content/` |
| 改安装脚本行为 | `install.sh` / `x-ui.sh` |
| 改环境变量 | `.env.example`（那份文件把每个变量都写清楚了） |

---

## 📊 七、本仓库说明

### 7.1 这是什么

本仓库是 **[`sdhfsl/3x-ui`](https://github.com/sdhfsl/3x-ui)** 的 fork（派生副本）。

| 层级 | 项目 | 地址 |
| --- | --- | --- |
| **本仓库** | `aiwozhonghua81/3x-ui` | https://github.com/aiwozhonghua81/3x-ui |
| **直接上游（本仓库 fork 自它）** | `sdhfsl/3x-ui` | https://github.com/sdhfsl/3x-ui |
| **原始上游（`sdhfsl/3x-ui` 又 fork 自它）** | `MHSanaei/3x-ui` | https://github.com/MHSanaei/3x-ui |

> **为什么会有两层 fork？**
> GitHub 的 fork 可以再被 fork。`MHSanaei/3x-ui` 是这个项目最原始、也是目前维护最活跃的版本（作者 Sanaei）。`sdhfsl/3x-ui` 在它基础上做了一些改动，本仓库再从 `sdhfsl/3x-ui` 派生，同时保留了对 `MHSanaei/3x-ui` 的追踪关系（GitHub 上显示的 "source" 就是 `MHSanaei/3x-ui`）。

### 7.2 本仓库的改动

**代码层面：目前没有本地独立改动。**

写这份说明时的核对结果：

| 核对项 | 结果 |
| --- | --- |
| 本地 `main` 与 `sdhfsl/3x-ui` 的 `main` 的差异 | **完全一致**（双向差异 0 个 commit，文件差异 0 个） |
| 本地独有的 commit | 无 |
| 已删除的上游 commit | 无 |

也就是说，**本仓库当前的代码状态和直接上游 `sdhfsl/3x-ui` 的 `main` 分支一模一样**，本仓库唯一的独有改动就是这一节中文说明。

这个仓库的用途是：作为个人自用的镜像 / 备份，方便在不改上游的情况下加中文说明、随时同步上游更新。

**同步上游的方法：**

```bash
git fetch sdhfsl
git merge sdhfsl/main
# 有冲突就解决后
git push origin main
```

### 7.3 README 的结构

本 README 分成上下两半：

| 位置 | 内容 | 语言 |
| --- | --- | --- |
| **上半部分** | 「中文说明开始」～「中文说明结束」之间 | 中文（本节，你正在看的） |
| **下半部分** | 从 `[English](/README.md) | [فارسی]...` 那行开始往下 | 上游原始 README，英文，**一字未改** |

上游还提供了其他语言的独立文件，本仓库同样完整保留：

| 文件 | 语言 |
| --- | --- |
| `README.md` | 英文（主文件） |
| `README.zh_CN.md` | 简体中文 |
| `README.fa_IR.md` | 波斯语 |
| `README.ar_EG.md` | 阿拉伯语 |
| `README.es_ES.md` | 西班牙语 |
| `README.ru_RU.md` | 俄语 |
| `README.tr_TR.md` | 土耳其语 |

> 想看其他语言？直接打开对应的 `README.xxx.md` 文件。

### 7.4 使用须知

- 上游项目采用 **GPL-3.0** 许可证，本 fork 同样遵循。
- 本仓库的「中文说明」部分由本仓库维护者撰写，与上游无关。
- 项目本身声明**仅供个人学习与自用**，使用者需自行确保使用方式符合相关法律法规和服务条款。

---

## 🔗 八、相关链接

| 资源 | 地址 |
| --- | --- |
| 官方文档站 | https://docs.sanaei.dev |
| 原始上游仓库 | https://github.com/MHSanaei/3x-ui |
| 直接上游仓库 | https://github.com/sdhfsl/3x-ui |
| Xray-core 内核 | https://github.com/XTLS/Xray-core |
| Release 下载页 | https://github.com/MHSanaei/3x-ui/releases |
| Docker 镜像 | `ghcr.io/mhsanaei/3x-ui` |


## 🖼️ 界面截图一览
下面这些图都是**这个仓库里原本就带的真实截图**，不是画的示意图：

**① 面板首页总览（深色）**

<img src="media/01-overview-dark.png" alt="面板首页总览（深色）" width="820">

**② 入站列表**

<img src="media/02-inbounds-dark.png" alt="入站列表" width="820">

**③ 客户端配置**

<img src="media/03-client-dark.png" alt="客户端配置" width="820">

**④ 节点管理**

<img src="media/05-nodes-dark.png" alt="节点管理" width="820">

**⑤ 设置页**

<img src="media/06-settings-dark.png" alt="设置页" width="820">

**⑥ API 文档页**

<img src="media/08-api-docs-dark.png" alt="API 文档页" width="820">

<!-- 中文说明结束 -->

---

（原上游 README 从这里开始）
[English](/README.md) | [فارسی](/README.fa_IR.md) | [العربية](/README.ar_EG.md) | [中文](/README.zh_CN.md) | [Español](/README.es_ES.md) | [Русский](/README.ru_RU.md) | [Türkçe](/README.tr_TR.md)

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="./media/3x-ui-dark.png">
    <img alt="3x-ui" src="./media/3x-ui-light.png">
  </picture>
</p>

<p align="center">
  <a href="https://github.com/MHSanaei/3x-ui/releases"><img src="https://img.shields.io/github/v/release/mhsanaei/3x-ui" alt="Release"></a>
  <a href="https://github.com/MHSanaei/3x-ui/actions"><img src="https://img.shields.io/github/actions/workflow/status/mhsanaei/3x-ui/release.yml.svg" alt="Build"></a>
  <a href="#"><img src="https://img.shields.io/github/go-mod/go-version/mhsanaei/3x-ui.svg" alt="GO Version"></a>
  <a href="https://github.com/MHSanaei/3x-ui/releases/latest"><img src="https://img.shields.io/github/downloads/mhsanaei/3x-ui/total.svg" alt="Downloads"></a>
  <a href="https://www.gnu.org/licenses/gpl-3.0.en.html"><img src="https://img.shields.io/badge/license-GPL%20V3-blue.svg?longCache=true" alt="License"></a>
  <a href="https://pkg.go.dev/github.com/mhsanaei/3x-ui/v3"><img src="https://pkg.go.dev/badge/github.com/mhsanaei/3x-ui/v3.svg" alt="Go Reference"></a>
  <a href="https://docs.sanaei.dev"><img src="https://img.shields.io/badge/docs-docs.sanaei.dev-22d3ee" alt="Documentation"></a>
</p>

**3X-UI** is an advanced, open-source web control panel for managing [Xray-core](https://github.com/XTLS/Xray-core) servers. It provides a clean, multi-language interface for deploying, configuring, and monitoring a wide range of proxy and VPN protocols — from a single VPS to multi-node deployments.

Built as an enhanced fork of the original X-UI project, 3X-UI adds broader protocol support, improved stability, per-client traffic accounting, and many quality-of-life features.

> [!IMPORTANT]
> This project is intended for personal use only. Please do not use it for illegal purposes or in a production environment.

## Features

- **Multi-protocol inbounds** — VLESS, VMess, Trojan, Shadowsocks, WireGuard, AmneziaWG, TUIC v5, Hysteria2, MTProto, HTTP, SOCKS (Mixed), Dokodemo-door / Tunnel, and TUN.
- **Modern transports & security** — TCP (Raw), mKCP, WebSocket, gRPC, HTTPUpgrade, and XHTTP, secured with TLS, XTLS, and REALITY.
- **AmneziaWG built in** — DPI-resistant WireGuard runs inside the panel on a userspace network stack, with no kernel module, DKMS, or extra packages to install.
- **TUIC v5 sidecar** — High-performance QUIC-based proxy with native UDP relay traffic metering, 0-RTT handshakes, and BBR congestion control.
- **MTProto proxies** — per-client FakeTLS secrets, ad-tags, and quotas, applied live without dropping existing connections.
- **Fallbacks** — serve multiple protocols on a single port (e.g. VLESS and Trojan on 443) using Xray's fallback support.
- **Per-client management** — traffic quotas, expiry dates, IP limits with trusted-address exemptions, HWID device limits, scheduled renewal cycles, live online status, and one-click share links, QR codes, and subscriptions.
- **Traffic statistics** — per inbound, per client, and per outbound, with reset controls.
- **Multi-node support** — manage and scale across multiple servers from a single panel, including cloning inbounds onto other nodes.
- **Outbound & routing** — WARP, NordVPN, PIA, custom routing rules, load balancers with balancer-to-balancer fallback, and outbound proxy chaining. Bundled geosite and geoip categories are browsable straight from the rule editor.
- **Built-in subscription server** — raw, JSON, and Clash output, auto-selected from the client's User-Agent, plus [custom page templates](docs/custom-subscription-templates.md).
- **Telegram and Discord bots** for remote monitoring and management.
- **RESTful API** with scoped, optionally expiring tokens and an in-panel API reference.
- **Installable panel (PWA)** — pin 3X-UI to a desktop or phone home screen.
- **Flexible storage** — SQLite (default) or PostgreSQL.
- **13 UI languages** with dark and light themes.
- **Fail2ban integration** for enforcing per-client IP limits.

## Screenshots

<details>
<summary>Click to expand</summary>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./media/01-overview-dark.png">
  <img alt="Overview" src="./media/01-overview-light.png">
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./media/02-add-inbound-dark.png">
  <img alt="Inbounds" src="./media/02-add-inbound-light.png">
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./media/03-add-client-dark.png">
  <img alt="Add client" src="./media/03-add-client-light.png">
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./media/05-add-nodes-dark.png">
  <img alt="Configs" src="./media/05-add-nodes-light.png">
</picture>

</details>

## Quick Start

```bash
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh)
```

To install a specific version, append its tag (e.g. `v3.7.0`):

```bash
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh) v3.7.0
```

To install the rolling **dev** build (latest per-commit pre-release from `main`, not a stable release), pass `dev-latest`:

```bash
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh) dev-latest
```

During installation a random username, password, and access path are generated. After installation, run `x-ui` to open the management menu, where you can start/stop the service, view or reset your login credentials, manage SSL certificates, and more.

Every release asset is published with a `.sha256` sum next to it. Both `install.sh` and the updater verify the archive against that sum and abort on a mismatch.

For full documentation — installation, configuration, operations, and the complete API reference — visit **[docs.sanaei.dev](https://docs.sanaei.dev)**.

### Unattended install

The installer also runs **non-interactively** for cloud-init.
Set `XUI_NONINTERACTIVE=1` (or pipe with no TTY) and it installs end-to-end with
zero prompts, generating random credentials and writing them to
`/etc/x-ui/install-result.env`. See [`deploy/`](deploy/) for:

- [Cloud-init user-data](deploy/cloud-init/) — unattended install on any cloud (Hetzner/AWS/DO/Vultr/GCP/Azure/Oracle)
- [Hetzner Cloud notes](deploy/marketplace/hetzner/) — cloud-init deployment on Hetzner

## Supported Platforms

**Operating systems:** Ubuntu, Debian, Armbian, Fedora, CentOS, RHEL, AlmaLinux, Rocky Linux, Oracle Linux, Amazon Linux, Virtuozzo, Arch, Manjaro, Parch, openSUSE (Tumbleweed / Leap), Alpine, and Windows.

**Architectures:** `amd64` · `386` · `arm64` (aarch64) · `armv7` · `armv6` · `armv5` · `s390x`.

## Database Options

3X-UI supports two backends, chosen during the install:

- **SQLite** (default) — a single file at `/etc/x-ui/x-ui.db`. Zero setup, ideal for small and medium deployments.
- **PostgreSQL** — recommended for high client counts or multi-node setups. The installer can install PostgreSQL locally for you, or accept a DSN to an existing server.

At runtime the backend is selected via environment variables (the installer writes these to `/etc/default/x-ui` for you):

```
XUI_DB_TYPE=postgres
XUI_DB_DSN=postgres://xui:password@127.0.0.1:5432/xui?sslmode=disable
```

### Migrating an existing SQLite install to PostgreSQL

```bash
x-ui migrate-db --dsn "postgres://xui:password@127.0.0.1:5432/xui?sslmode=disable"
# then set XUI_DB_TYPE and XUI_DB_DSN in /etc/default/x-ui and restart:
systemctl restart x-ui
```

The source SQLite file is left untouched; remove it manually once you have verified the new backend.

### Docker

The default `docker compose up -d` keeps using SQLite. To run with the bundled PostgreSQL service, uncomment the two `XUI_DB_*` env lines in `docker-compose.yml` and start with the profile:

```bash
docker compose --profile postgres up -d
```

The image bundles Fail2ban (enabled by default) to enforce per-client **IP limits**. Fail2ban bans offenders with `iptables`, which requires the `NET_ADMIN` capability. `docker-compose.yml` already grants it via `cap_add`; if you start the container with `docker run` instead, add the capabilities yourself, otherwise bans are logged but never applied:

```bash
docker run -d --cap-add=NET_ADMIN --cap-add=NET_RAW ... ghcr.io/mhsanaei/3x-ui
```

## Environment Variables

| Variable | Description | Default |
| --- | --- | --- |
| `XUI_DB_TYPE` | Database backend: `sqlite` or `postgres` | `sqlite` |
| `XUI_DB_DSN` | PostgreSQL connection string (when `XUI_DB_TYPE=postgres`) | — |
| `XUI_DB_FOLDER` | Directory for the SQLite database file | `/etc/x-ui` |
| `XUI_DB_MAX_OPEN_CONNS` | Maximum open connections (PostgreSQL pool) | — |
| `XUI_DB_MAX_IDLE_CONNS` | Maximum idle connections (PostgreSQL pool) | — |
| `XUI_INIT_WEB_BASE_PATH` | The initial URI path for the web panel | `/` |
| `XUI_ENABLE_FAIL2BAN` | Enable Fail2ban-based IP-limit enforcement | `true` |
| `XUI_LOG_LEVEL` | Log verbosity (`debug`, `info`, `warning`, `error`) | `info` |
| `XUI_DEBUG` | Enable debug mode | `false` |
| `XUI_TUNNEL_HEALTH_MONITOR` | Enable the tunnel health monitor (probes a URL and restarts xray after repeated failures; a restart drops all clients) | `false` |
| `XUI_TUNNEL_HEALTH_PROXY` | Proxy the probe is sent through; point it at a local xray inbound so the probe tests the tunnel (e.g. `socks5://127.0.0.1:1080`). Empty means the probe only checks host connectivity | — |
| `XUI_TUNNEL_HEALTH_URL` | URL probed for tunnel health | `https://www.cloudflare.com/cdn-cgi/trace` |
| `XUI_TUNNEL_HEALTH_INTERVAL` | Interval between probes | `30s` |
| `XUI_TUNNEL_HEALTH_TIMEOUT` | Per-probe timeout | `10s` |
| `XUI_TUNNEL_HEALTH_FAILURES` | Consecutive failures before a restart is triggered | `3` |
| `XUI_TUNNEL_HEALTH_COOLDOWN` | Minimum delay between consecutive restarts | `5m` |
| `NODE_TOKEN_ENCRYPTION` | Encryption at rest for node API tokens: `off`, `migration`, or `required` (note: no `XUI_` prefix) | `off` |
| `XUI_NODE_TOKEN_KEY_FILE` | JSON keyring (mode `0600`) holding the active key id and its base64 32-byte keys | `/etc/x-ui/node_token_key.json` |
| `XUI_NODE_TOKEN_KEY` | A single base64 32-byte key, used only when the key file cannot be loaded | — |

The complete list is on the [environment variables reference](https://docs.sanaei.dev/docs/reference/env-vars).

## Supported Languages

The panel UI is available in 13 languages:

English · فارسی · العربية · 中文（简体） · 中文（繁體） · Español · Русский · Українська · Türkçe · Tiếng Việt · 日本語 · Bahasa Indonesia · Português (Brasil)

## Contributing

Contributions are welcome. Please read the [Contributing Guide](/CONTRIBUTING.md) before opening an issue or pull request.

## A Special Thanks to

- [alireza0](https://github.com/alireza0/)

## Acknowledgment

- [Iran v2ray rules](https://github.com/chocolate4u/Iran-v2ray-rules) (License: **GPL-3.0**): _Enhanced v2ray/xray and v2ray/xray-clients routing rules with built-in Iranian domains and a focus on security and adblocking._
- [Russia v2ray rules](https://github.com/runetfreedom/russia-v2ray-rules-dat) (License: **GPL-3.0**): _This repository contains automatically updated V2Ray routing rules based on data on blocked domains and addresses in Russia._

## Community Tools

Tools and integrations built by the community around 3x-ui.

- [terraform-provider-3x-ui](https://github.com/batonogov/terraform-provider-threexui) (License: **MIT**): _Manage inbounds, clients, panel settings, and Xray configuration as code with Terraform / OpenTofu._
- [3X-UI Manager](https://github.com/yukh975/3X-UI-Manager) (License: **MIT**): _Native Android client for 3x-ui — dashboard, inbounds, clients with QR sharing, nodes and multi-panel management. Available on F-Droid._

## Support project

**If this project is helpful to you, you may wish to give it a**:star2:

<a href="https://www.buymeacoffee.com/MHSanaei" target="_blank">
<img src="./media/default-yellow.png" alt="Buy Me A Coffee" style="height: 70px !important;width: 277px !important;" >
</a>

</br>
<a href="https://nowpayments.io/donation/hsanaei" target="_blank" rel="noreferrer noopener">
   <img src="./media/donation-button-black.svg" alt="Crypto donation button by NOWPayments">
</a>

## Star History

<a href="https://www.star-history.com/?repos=mhsanaei%2F3x-ui&type=date&legend=top-left">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/chart?repos=mhsanaei/3x-ui&type=date&theme=dark&legend=top-left" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/chart?repos=mhsanaei/3x-ui&type=date&legend=top-left" />
   <img alt="Star History Chart" src="https://api.star-history.com/chart?repos=mhsanaei/3x-ui&type=date&legend=top-left" />
 </picture>
</a>

<p align="center">
 <a href="https://www.star-history.com/mhsanaei/3x-ui">
  <picture><source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/badge?repo=MHSanaei/3x-ui&type=rank&theme=dark" /><source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/badge?repo=MHSanaei/3x-ui&type=rank" /><img alt="Star History Rank" src="https://api.star-history.com/badge?repo=MHSanaei/3x-ui&type=rank" /></picture> <picture><source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/badge?repo=MHSanaei/3x-ui&type=trending&theme=dark" /><source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/badge?repo=MHSanaei/3x-ui&type=trending" /><img alt="GitHub Trending Repository of the Day" src="https://api.star-history.com/badge?repo=MHSanaei/3x-ui&type=trending" /></picture>
 </a>
</p>
