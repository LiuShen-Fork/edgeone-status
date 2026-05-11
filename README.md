# 清羽飞扬流量分析

一个面向 Tencent Cloud EdgeOne 与 EdgeOne Pages 的轻量级站点数据面板。项目聚合流量、带宽、请求、性能、安全与 Pages 函数数据，并提供本地 API 文档，适合用于个人站点或轻量运维场景。

<p align="center">
  <a href="LICENSE"><img alt="License" src="https://img.shields.io/badge/license-AGPL--3.0-111827?style=flat-square"></a>
  <img alt="Runtime" src="https://img.shields.io/badge/runtime-Node.js-334155?style=flat-square">
  <img alt="Deploy" src="https://img.shields.io/badge/deploy-EdgeOne%20Pages-0f172a?style=flat-square">
</p>

## 预览

- 首页：站点数据总览、趋势图表、排行分析与筛选控件。
- 文档页：当前项目本地接口说明与调用参数。
- 主题：支持浅色 / 暗色模式，并会记住用户选择。

## 特性

### 数据总览

- 展示总流量、请求流量、响应流量、带宽峰值与请求数。
- 支持 1 小时、当天、昨天、7 天、31 天等常用时间范围。
- 自动适配统计粒度，减少手动配置成本。

### 多维分析

- 流量分析：总流量、请求流量、响应流量趋势。
- 带宽分析：总带宽、请求带宽、响应带宽峰值。
- 请求分析：请求数变化与热点分布。
- 性能分析：L7 响应耗时趋势。
- 安全分析：安全相关指标展示。
- TOP 排行：域名、URL、状态码等维度排行。

### 站点筛选

- 支持查看全部站点或指定站点。
- 支持按子域名筛选数据。
- 对 EdgeOne Pages 默认站点展示名做了更友好的显示处理。

### 页面体验

- 精致浅色风格与完整暗色模式。
- 响应式布局，兼容桌面端与移动端。
- 自定义下拉框、卡片、页脚与滚动条样式。
- 内置 API 文档页，便于理解前端与服务端接口关系。

## 页面结构

```text
/
├─ index.html                # 数据面板首页
├─ docs.html                 # 本地 API 文档页
├─ assets/                   # 前端静态资源
├─ node-functions/api/       # EdgeOne Pages Functions API
├─ package.json              # Node.js 依赖
└─ README.md
```

## 快速开始

### 1. 克隆项目

```bash
git clone https://github.com/MoForgt/edgeone-status.git
cd edgeone-status
```

### 2. 安装依赖

```bash
npm install
```

如需使用 EdgeOne Pages 本地开发能力，请确保已安装并登录 EdgeOne CLI：

```bash
npm install -g edgeone
edgeone login
```

### 3. 配置密钥

推荐使用环境变量：

```bash
SECRET_ID=你的腾讯云 SecretId
SECRET_KEY=你的腾讯云 SecretKey
```

也可以在项目根目录创建 `key.txt`：

```text
SecretId：你的腾讯云 SecretId
SecretKey：你的腾讯云 SecretKey
```

### 4. 启动开发服务

```bash
edgeone pages dev
```

默认访问地址通常为：

```text
http://localhost:8088
```

## 环境变量

| 变量 | 必填 | 说明 | 默认值 |
| --- | --- | --- | --- |
| `SECRET_ID` | 是 | 腾讯云访问密钥 ID | - |
| `SECRET_KEY` | 是 | 腾讯云访问密钥 Key | - |
| `SITE_NAME` | 否 | 页面标题 / 站点名称 | `清羽飞扬流量分析` |
| `SITE_ICON` | 否 | 页面图标地址 | `/favicon.png` |
| `ICP` | 否 | 备案号 | `陕ICP备2024028531号` |

## 权限要求

腾讯云密钥建议只授予 EdgeOne 只读权限：

```text
QcloudTEOReadOnlyaccess
```

请避免使用主账号密钥或高权限密钥。推荐创建独立子用户，仅开启编程访问，并按需绑定最小权限策略。

## 本地 API

本项目的浏览器请求均发往本站 `/api/*`，腾讯云密钥只在服务端读取，不会暴露到前端。

| 路径 | 说明 |
| --- | --- |
| `/api/config` | 获取站点名称、图标与备案号配置 |
| `/api/zones` | 获取 EdgeOne 站点列表 |
| `/api/hosts` | 获取站点下的域名列表 |
| `/api/traffic` | 查询流量、带宽、请求、性能、安全与 TOP 指标 |
| `/api/pages/build-count` | 查询 Pages 构建次数 |
| `/api/pages/cloud-function-requests` | 查询 Pages 云函数请求数据 |
| `/api/pages/cloud-function-monthly-stats` | 查询 Pages 云函数月度统计 |

更完整的参数、返回结构与错误说明，请访问部署后的 `/docs.html`。

## 部署

### EdgeOne Pages

1. Fork 或导入本仓库。
2. 在 EdgeOne Pages 中创建项目并连接仓库。
3. 配置 `SECRET_ID`、`SECRET_KEY` 等环境变量。
4. 部署后访问首页与 `/docs.html`。

### 注意事项

- 确认密钥具备 EdgeOne 只读权限。
- 确认 Pages Functions 已正确识别 `node-functions/api/[[default]].js`。
- 若接口返回未配置密钥，请检查环境变量或 `key.txt` 格式。

## 技术栈

| 层级 | 技术 |
| --- | --- |
| 前端 | HTML、Tailwind CSS、ECharts |
| 服务端 | Node.js、Express |
| 云服务 | Tencent Cloud EdgeOne、EdgeOne Pages |
| SDK | Tencent Cloud SDK for Node.js |

## 开源说明

本项目基于开源项目 [MoForgt/edgeone-status](https://github.com/MoForgt/edgeone-status) 构建，并遵循原项目许可协议。

当前仓库使用 [AGPL-3.0](LICENSE) 许可证。若你修改并通过网络提供服务，请按照 AGPL-3.0 要求提供对应源代码。

## 致谢

感谢 EdgeOne、ECharts、Tailwind CSS 以及相关开源项目提供的基础能力。