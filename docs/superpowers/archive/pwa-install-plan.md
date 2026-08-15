# PWA 可安装支持 实施计划

**目标**：让 litools 具备 PWA 可安装（installable）能力 —— 用户访问 GitHub Pages 后可将 litools"安装"为桌面/手机 App，带独立图标。

## 现状

- `static/sw.js` 已存在（stale-first 离线缓存），index.html 已注册（仅 https 环境，file: 跳过）
- **缺 `manifest.json`** → 浏览器无法识别为可安装 PWA
- 缺 192x192 / 512x512 应用图标（manifest 必需）
- 缺 `theme-color`、`apple-touch-icon`、`apple-mobile-web-app-capable` meta

## 步骤

- [x] Step 1: 生成 PWA 图标（192x192 + 512x512 PNG，🧰 风格）
  - verify: `static/` 下出现 icon-192.png、icon-512.png，且尺寸正确

- [x] Step 2: 创建 `manifest.json`（name/short_name/icons/theme_color/background_color/display/start_url）
  - verify: 文件存在，JSON 可解析（node/python 校验）

- [x] Step 3: index.html head 加 `<link rel="manifest">`、`theme-color`、`apple-touch-icon`、`apple-mobile-web-app-capable`
  - verify: head 中 4 处新增标签齐全

- [x] Step 4: **sw.js 移至站点根目录**（原 static/sw.js scope 仅覆盖 /static/，无法控制 index.html 离线加载），注册路径改为 `sw.js`，缓存清单升 v2 含 manifest 与图标
  - verify: 根目录存在 sw.js，static/sw.js 已删除，index.html 注册 'sw.js'

- [x] Step 5: headless Chrome 验证可安装性（manifest 可解析、SW 注册+激活、离线可用）
  - verify: SW scope 覆盖根目录、activated、离线访问页面完整加载（停服务器后仍返回完整 DOM）

## 不做什么

- 不改单文件铁律：manifest 与图标放 static/，index.html 仍是唯一代码文件
- 不加推送通知、不加后端
- 不动现有主题/样式