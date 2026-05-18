# passwordmanagement

一个纯前端的随机密码 / API Token / UUID 生成器，部署在 Cloudflare Workers 静态资源（Workers Assets）上。所有随机数都在浏览器本地通过 `crypto.getRandomValues()` 与 `crypto.randomUUID()` 生成，不会上传任何数据。

## 功能

- 密码生成：长度 3–64，可勾选小写 / 大写 / 数字 / 特殊符号（至少 2 类），强制包含每个被选中的字符类，再 Fisher–Yates 洗牌
- Token 生成：32 字节随机 hex（256 bit）
- UUID 生成：v4，优先调用 `crypto.randomUUID()`，否则手动填写 version / variant 位
- 一键复制（优先 `navigator.clipboard`，回落到 `document.execCommand('copy')`）
- 使用拒绝采样（rejection sampling）保证字符分布无偏

## 本地开发

```bash
npm install
npx wrangler dev
```

## 部署到 Cloudflare Workers

```bash
npx wrangler deploy
```

部署后会得到一个 `*.workers.dev` 子域；如果要绑定自定义域名，在 Cloudflare 控制台的 Worker → Settings → Domains & Routes 里加。

## 目录结构

```
.
├── public/
│   └── index.html        # 单文件应用
├── wrangler.toml         # Workers 配置（Assets directory + SPA fallback）
├── package.json
├── LICENSE               # MIT
└── README.md
```

## License

MIT
