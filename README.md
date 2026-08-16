<p align="center">
  <img src="static/pic.png" alt="litools" width="800">
</p>

<h1 align="center">litools</h1>

<p align="center">
  <strong>Li's Dev Toolkit — 单文件开发者工具箱</strong><br>
  一个 HTML 文件，零依赖，双击即用
</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-2.0.0-blue.svg" alt="Version">
  <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License">
  <img src="https://img.shields.io/badge/html-100%25-orange.svg" alt="HTML">
  <img src="https://img.shields.io/badge/zero-dependencies-brightgreen.svg" alt="Dependencies">
  <img src="https://img.shields.io/badge/i18n-中英-blueviolet.svg" alt="i18n">
</p>

<p align="center">
  <strong>🚀 在线体验</strong><br>
  <a href="https://lzg14.github.io/litools">GitHub Pages</a> · <a href="https://litools.pages.dev">Cloudflare Pages</a>
</p>

---

## 功能一览

> 23 个独立模块，扁平化导航，设置中按分类自由勾选显示哪些。

| 分类 | 模块 | 功能 |
|------|------|------|
| **核心** | JSON | 格式化 / 压缩 / 转义 / 树形展示 / JSONPath 查询 / 智能修复 / 节点统计 |
| | 搜索 | 多关键词高亮 / 替换 / 过滤 / 流式搜索大文件（50MB+） |
| | 正则 | 正则测试 / 高亮匹配 |
| | 速查 | 正则解释 / 常用正则速查 / 预览 |
| | 时间戳 | 秒/毫秒/微秒互转（自动识别）/ 相对时间 |
| | 计时器 | 秒表 / 计次 / 重置 |
| | 定时器 | 倒计时（快捷时长 / 自定义时分秒）/ 暂停 / 重置 / 到点提醒 |
| **SQL** | SQL IN | SQL IN 语句批量生成 |
| | SQL 解析 | INSERT 语句解析（列值对应，支持转义与批量）|
| | SQL 格式化 | SQL 语句格式化 |
| **编解码** | Base64 | Base64 编解码 |
| | URL | URL 编解码 / 参数解析 |
| | HTML | HTML 实体编解码 |
| | 进制 | 十进制 / 十六进制 / 二进制 / 八进制互转 |
| | 二进制文本 | 文本与二进制互转 |
| **生成** | UUID | UUID 生成（多种格式） |
| | Hash | SHA-1/256/384/512 计算 |
| | 颜色 | HEX / RGB / HSL 互转 / 调色板预览 / 对比度检测 |
| | 密码 | 随机密码生成 |
| | Cron | Cron 表达式解析 |
| **文本** | 处理 | 排序 / 去重 / 反转 / 正则替换 |
| | 大小写 | 大小写转换（camel/snake/kebab/…）|
| | CSV | CSV 解析 / 表格预览 |
| | Diff | 文本差异对比 / 高亮显示 |

## 特性

- **单文件** — 整个工具就是一个 `index.html`，无需安装、完全离线可用
- **零依赖** — 纯原生 HTML + CSS + JavaScript
- **中英双语** — 右上角一键切换，UI 实时跟随
- **自定义导航** — 23 个模块按分类勾选显示，支持全选/全不选/恢复默认
- **导航排序** — 取消勾选后重新勾选，模块排在末尾，可按此自定义顺序
- **暗色主题** — 支持亮色 / 暗色主题切换
- **大文件支持** — 搜索模块支持 50MB+ 文件的流式搜索
- **快捷键** — `Ctrl+Enter` 格式化 JSON
- **PWA 可安装** — 支持安装为独立 App（桌面 / 手机），离线可用
- **拖拽支持** — 直接拖文件到对应模块即可打开

## 快速开始

```bash
# 方式一：直接下载
# 下载 index.html，双击用浏览器打开

# 方式二：GitHub Pages（推荐）
# Fork 后在 Settings → Pages 启用即可
```

## 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl + Enter` | 格式化 JSON |
| `Escape` | 关闭弹窗 |

## 技术栈

- HTML5 + CSS3 + Vanilla JavaScript
- `crypto.subtle`（Hash 计算，需 HTTPS 或 localhost）
- Web APIs: `FileReader`, `TextEncoder`, `navigator.clipboard`

## 浏览器兼容

| 浏览器 | 版本 |
|--------|------|
| Chrome / Edge | 90+ |
| Firefox | 90+ |
| Safari | 15+ |

## License

[MIT](LICENSE)
