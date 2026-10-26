[English](README.md) | **中文**

# etais-web

**[etais.dev](https://etais.dev)** 官网源码：Etai 应用集营销站点（Cloudflare Worker **`etai`** + [`site/`](site/) 静态资源）。

本仓库**不包含** Android 工程，也**不托管** APK。下载按钮与二维码指向 **https://dl.etais.dev**（配置见 [`scripts/site-download-config.mjs`](scripts/site-download-config.mjs)）。在 R2 发布新 APK 后，请更新该文件，执行 `python3 scripts/generate-download-qr-svgs.py`（需安装 `qrcode`），再运行 `npm run build`。

## 目录结构

| 路径 | 说明 |
| --- | --- |
| [`site/`](site/) | 静态页面与资源（经 Workers Assets 提供） |
| [`worker/`](worker/) | Worker 入口：规范域名跳转、可选 D1 访问日志 |
| [`wrangler.jsonc`](wrangler.jsonc) | Worker 名称 **`etai`**、资源绑定、D1 **`ACCESS_LOG`** |
| [`migrations/`](migrations/) | 页面访问日志 D1 表结构 |
| [`scripts/`](scripts/) | 站点生成与校验脚本 |

## 环境要求

- **Node.js 20+** 与 npm
- Cloudflare 账号（Wrangler 见本仓库 devDependencies）
- 可选：Python 3 + `qrcode`；截图类脚本需 `npx playwright install chromium`

## 安装

```bash
npm install
```

## 重新生成页面

修改下载面板、应用页或 FAQ 等共享配置后：

```bash
npm run build
```

## 本地预览

```bash
npm run dev
```

也可仅用静态服务器托管 `site/`（重定向与访问日志需 Wrangler）。

## 部署

生产环境通常通过 Cloudflare **Workers Builds**（Git 集成，`main` 分支）自动部署。

手动部署：

```bash
npm run deploy
```

### D1 访问日志

```bash
npx wrangler d1 execute etai-access-log --remote --file=./migrations/0001_access_log.sql
```

本地未绑定 `ACCESS_LOG` 时 Worker 会跳过写日志。

## 缓存

编辑 `/css/*`、`/js/*` 后，请在 [`site/index.html`](site/index.html) 与 [`site/404.html`](site/404.html) 中更新资源 URL 的 `?v=` 参数（详见 [`site/_headers`](site/_headers)）。

## 应用与源码仓库

| 应用 | 仓库 |
| --- | --- |
| C 一周通 | [gnatecheng/c-week](https://github.com/gnatecheng/c-week) |
| 轻记账 | [gnatecheng/easy-ledger](https://github.com/gnatecheng/easy-ledger) |
| 团团记 | [gnatecheng/group-matters](https://github.com/gnatecheng/group-matters) |

## 许可证

MIT — 见 [LICENSE](LICENSE)。
