# passwordmanagement

一个纯前端的随机密码 / API Token / UUID 生成器，部署在 Cloudflare Workers 静态资源（Workers Assets）上。所有随机数都在浏览器本地通过 `crypto.getRandomValues()` 与 `crypto.randomUUID()` 生成，不会上传任何数据。

## 功能

- 密码生成
  - 长度 3–64，滑块 + 数字框双向同步
  - 小写 / 大写 / 数字 / 特殊符号四类，至少选 1 类，强制每类至少出现一次后 Fisher–Yates 洗牌
  - 可选「排除易混淆字符」（`0 O o 1 l I` `` ` ``）
  - 实时 Shannon 熵密码强度条（弱 / 一般 / 强 / 非常强）
  - 一键显示 / 隐藏密码
- API Token：16 / 32 / 64 字节随机 hex
- UUID：v4，优先调用 `crypto.randomUUID()`，否则手动设置 version / variant 位
- 一键复制（优先 `navigator.clipboard`，回落到 `execCommand`）；隐藏状态下复制不会泄露
- 拒绝采样保证字符分布无偏，规避 `% n` 取模偏置
- 移动端响应式布局，支持键盘快捷键（回车重新生成）
- 单文件 HTML，零依赖、零追踪、零外部资源（仅一份 Google Fonts CSS）

## 本地开发

```bash
npm install
npx wrangler dev
```

## 部署

```bash
npx wrangler deploy
```

`wrangler.toml` 里已经配置好自定义域作为 Workers 路由。Cloudflare 会自动在 zone 上创建 DNS 记录并签 SSL 证书。要换成自己的域名，把 `wrangler.toml` 里的 `routes` 改掉即可（zone 必须在同一个 Cloudflare 账号下）。

### 自动部署

仓库已配置 [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml)：每次 push 到 `main` 或在 Actions 页面手动触发，都会自动跑 `wrangler deploy`。需要在 GitHub 仓库的 **Settings → Secrets and variables → Actions** 里加两个 secret：

- `CLOUDFLARE_API_TOKEN`：在 [Cloudflare → My Profile → API Tokens](https://dash.cloudflare.com/profile/api-tokens) 用 "Edit Cloudflare Workers" 模板生成
- `CLOUDFLARE_ACCOUNT_ID`：在任意 Cloudflare 控制台页面右侧能看到

## 目录结构

```
.
├── public/
│   └── index.html        # 单文件应用
├── wrangler.toml         # Workers 配置
├── package.json
├── LICENSE               # MIT
└── README.md
```

---

# passwordmanagement (English)

A zero-dependency, fully client-side generator for random passwords, API tokens, and UUIDs, deployed on Cloudflare Workers Assets. Every random value is produced locally in the browser via `crypto.getRandomValues()` and `crypto.randomUUID()` — nothing is ever sent to a server.

## Features

- Password generator
  - Length 3–64, range slider and number input kept in sync
  - Four character classes (lowercase / uppercase / digits / symbols); at least one must be selected, with one of each guaranteed before a Fisher–Yates shuffle
  - Optional "exclude ambiguous characters" filter (`0 O o 1 l I` `` ` ``)
  - Real-time Shannon-entropy strength meter (weak / fair / strong / very strong)
  - Show / hide password toggle
- API token: 16 / 32 / 64 random bytes encoded as hex
- UUID: v4 via `crypto.randomUUID()`, with a manual fallback that sets the version and variant bits explicitly
- One-click copy (prefers `navigator.clipboard`, falls back to `execCommand`); copying a hidden password never reveals it
- Rejection sampling for unbiased character selection — no `% n` modulo bias
- Responsive layout for mobile; press <kbd>Enter</kbd> anywhere to regenerate the password
- Single HTML file, no dependencies, no tracking, no third-party assets aside from a single Google Fonts stylesheet

## Local development

```bash
npm install
npx wrangler dev
```

## Deploy

```bash
npx wrangler deploy
```

The `wrangler.toml` file already wires up a custom domain as the Worker's route. Cloudflare creates the DNS record and provisions the SSL certificate automatically. To use your own domain, edit the `routes` entry in `wrangler.toml` (the zone must live in the same Cloudflare account).

### Auto-deploy

The repo ships with [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml): every push to `main` (or a manual run from the Actions tab) triggers `wrangler deploy` on a GitHub-hosted runner. Add two secrets under **Settings → Secrets and variables → Actions** in your GitHub repo:

- `CLOUDFLARE_API_TOKEN` — create one with the "Edit Cloudflare Workers" template at [Cloudflare → My Profile → API Tokens](https://dash.cloudflare.com/profile/api-tokens)
- `CLOUDFLARE_ACCOUNT_ID` — visible on the right side of any Cloudflare dashboard page

## Project layout

```
.
├── public/
│   └── index.html        # Single-file app
├── wrangler.toml         # Workers configuration
├── package.json
├── LICENSE               # MIT
└── README.md
```

## License

MIT
