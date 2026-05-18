# passwordmanagement

一个纯前端的随机密码 / API Token / UUID 生成器，部署在 Cloudflare Workers 静态资源（Workers Assets）上。所有随机数都在浏览器本地通过 `crypto.getRandomValues()` 与 `crypto.randomUUID()` 生成，不会上传任何数据。

线上：<https://pa.095233.xyz/> · <https://passwordmanagement.minis233.workers.dev/>

## 功能

- 密码生成
  - 长度 3–64，滑块 + 数字框双向同步
  - 小写 / 大写 / 数字 / 特殊符号四类（至少 2 类），强制每类至少出现一次后 Fisher–Yates 洗牌
  - 可选「排除易混淆字符」（`0 O o 1 l I` `` ` ``）
  - 实时 Shannon 熵密码强度条（弱 / 一般 / 强 / 非常强）
  - 一键显示 / 隐藏密码
- API Token：16 / 32 / 64 字节随机 hex
- UUID：v4，优先调用 `crypto.randomUUID()`，否则手动设置 version / variant 位
- 一键复制（优先 `navigator.clipboard`，回落到 `execCommand`）；隐藏状态下复制不会泄露
- 深色 / 浅色主题：跟随系统，可手动切换并持久化（localStorage）
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

`wrangler.toml` 里已经配置好 `pa.095233.xyz` 作为 Workers 自定义域。Cloudflare 会自动在该 zone 上创建 DNS 记录并签 SSL 证书。

要换成自己的域名，把 `wrangler.toml` 里的 `routes` 改掉即可（zone 必须在同一个 Cloudflare 账号下）。

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

## License

MIT
