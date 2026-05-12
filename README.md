<div align="center">
  <img src="./favicon.png" alt="流量分析" width="110" height="110" />
  <h1>流量分析</h1>
  <p>一个简洁的 EdgeOne 站点流量看板。</p>
</div>

## 项目介绍

流量分析是一个面向 Tencent Cloud EdgeOne 的轻量级数据面板，用于集中查看站点流量、带宽、请求量、性能、安全以及 EdgeOne Pages 相关统计数据。

项目将腾讯云密钥保存在服务端环境变量中，前端只请求本项目提供的 `/api/*` 接口，避免将敏感密钥暴露到浏览器侧。适合个人站点、博客、轻量运维面板或 EdgeOne Pages 项目监控场景。

## 目录

- [项目介绍](#项目介绍)
- [功能特性](#功能特性)
- [基于什么开发](#基于什么开发)
- [项目结构](#项目结构)
- [快速开始](#快速开始)
- [环境变量配置](#环境变量配置)
- [权限建议](#权限建议)
- [本地接口](#本地接口)
- [部署说明](#部署说明)
- [开源说明](#开源说明)

## 功能特性

- 数据总览：展示总流量、请求流量、响应流量、带宽峰值与请求数。
- 趋势分析：支持按 1 小时、当天、昨天、7 天、31 天等范围查看数据变化。
- 多维排行：支持域名、URL、状态码等维度的 TOP 数据统计。
- 站点筛选：可查看全部站点，也可选择指定站点或子域名。
- Pages 统计：支持查看 EdgeOne Pages 构建次数、云函数请求数与月度统计。
- 本地文档：内置 `/docs.html`，便于查看接口说明与参数结构。
- 界面体验：支持响应式布局、浅色 / 暗色模式与自定义站点信息。
- 隐私保护：支持通过黑名单隐藏指定域名，避免展示不希望公开的数据。

## 基于什么开发

| 类型 | 使用内容 |
| --- | --- |
| 云服务 | Tencent Cloud EdgeOne、EdgeOne Pages |
| 前端 | HTML、Tailwind CSS、ECharts |
| 服务端 | Node.js、Express |
| SDK | Tencent Cloud SDK for Node.js |
| 环境变量 | dotenv |
| 运行方式 | EdgeOne Pages Functions、本地 Node.js 环境 |

## 项目结构

```text
.
├─ index.html                # 数据面板首页
├─ docs.html                 # 本地 API 文档页
├─ favicon.png               # 站点图标
├─ assets/                   # 前端静态资源
├─ node-functions/api/       # EdgeOne Pages Functions 接口
├─ package.json              # Node.js 依赖配置
├─ .env.example              # 环境变量示例
└─ README.md                 # 项目说明
```

## 快速开始

### 1. 安装依赖

```bash
npm install
```

### 2. 创建环境变量文件

复制 `.env.example` 为 `.env`，并根据自己的腾讯云账号与站点信息修改配置。

```bash
cp .env.example .env
```

### 3. 启动本地开发

如果使用 EdgeOne Pages 本地开发能力，请先安装并登录 EdgeOne CLI。

```bash
npm install -g edgeone
edgeone login
edgeone pages dev
```

默认本地访问地址通常为：

```text
http://localhost:8088
```

## 环境变量配置

`.env.example` 当前包含以下配置：

```env
SECRET_ID=your_tencentcloud_secret_id
SECRET_KEY=your_tencentcloud_secret_key
SITE_NAME=清羽飞扬流量分析
SITE_ICON=/favicon.png
ICP=陕ICP备2024028531号
BLACK_LIST=["xxx.example.com","xxx2.example.com"]
```

| 变量 | 必填 | 示例 | 说明 |
| --- | --- | --- | --- |
| `SECRET_ID` | 是 | `your_tencentcloud_secret_id` | 腾讯云 API 访问密钥 SecretId，用于服务端请求 EdgeOne 接口。 |
| `SECRET_KEY` | 是 | `your_tencentcloud_secret_key` | 腾讯云 API 访问密钥 SecretKey，请勿提交到公开仓库。 |
| `SITE_NAME` | 否 | `清羽飞扬流量分析` | 页面标题和站点显示名称，可改成自己的面板名称。 |
| `SITE_ICON` | 否 | `/favicon.png` | 页面图标地址，默认使用项目根目录下的 `favicon.png`。 |
| `ICP` | 否 | `陕ICP备2024028531号` | 页脚展示的备案号，不需要展示时可留空。 |
| `BLACK_LIST` | 否 | `["xxx.example.com","xxx2.example.com"]` | 域名黑名单，用于隐藏或过滤不希望展示的域名数据，建议使用 JSON 数组格式。 |

### 配置建议

- 本地开发时使用 `.env` 保存配置，不要将真实密钥提交到仓库。
- 线上部署时建议在 EdgeOne Pages 的环境变量中配置 `SECRET_ID` 与 `SECRET_KEY`。
- `BLACK_LIST` 适合隐藏测试域名、私有域名或不希望公开展示的域名。
- `SITE_ICON` 可以继续使用 `/favicon.png`，也可以替换为自己的图片路径。

## 权限建议

腾讯云密钥建议使用独立子用户，并只授予 EdgeOne 只读权限，例如：

```text
QcloudTEOReadOnlyaccess
```

请避免使用主账号密钥或高权限密钥，降低密钥泄露后的风险。

## 本地接口

浏览器侧请求会统一发往本站 `/api/*`，由服务端读取环境变量并调用腾讯云接口。

| 接口 | 说明 |
| --- | --- |
| `/api/config` | 获取站点名称、图标、备案号等页面配置。 |
| `/api/zones` | 获取 EdgeOne 站点列表。 |
| `/api/hosts` | 获取指定站点下的域名列表。 |
| `/api/traffic` | 查询流量、带宽、请求、性能、安全与 TOP 指标。 |
| `/api/pages/build-count` | 查询 EdgeOne Pages 构建次数。 |
| `/api/pages/cloud-function-requests` | 查询 Pages 云函数请求数据。 |
| `/api/pages/cloud-function-monthly-stats` | 查询 Pages 云函数月度统计。 |

更详细的参数、返回结构与错误说明，请访问部署后的 `/docs.html`。

## 部署说明

### EdgeOne Pages 部署

1. 将项目导入 EdgeOne Pages。
2. 在项目设置中配置环境变量。
3. 确认 `node-functions/api/` 能被 Pages Functions 正确识别。
4. 部署完成后访问首页查看流量分析面板。
5. 如需查看接口文档，可访问 `/docs.html`。

### 常见检查项

- `SECRET_ID` 与 `SECRET_KEY` 是否已正确配置。
- 腾讯云子用户是否拥有 EdgeOne 只读权限。
- `BLACK_LIST` 是否为合法 JSON 数组格式。
- `SITE_ICON` 指向的图标文件是否存在。
- Pages Functions 是否成功部署。

## 开源说明

本项目基于原始项目 [MoForgt/edgeone-status](https://github.com/MoForgt/edgeone-status) 修改与完善。

本项目遵循仓库内许可证文件声明的开源协议。若你修改并通过网络提供服务，请注意遵守对应许可证要求。
