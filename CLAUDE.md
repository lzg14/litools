# litools 开发规范

## 项目定位

单文件开发者工具箱，整个项目就是一个 `index.html`，零依赖、纯原生 HTML + CSS + JavaScript。

**铁律：所有代码写在 index.html 一个文件里，不拆分、不引入任何第三方库。**

## 文件结构

```
index.html    ← 唯一代码文件（HTML + CSS + JS 全在里面）
static/       ← 静态资源（截图等）
README.md     ← 项目说明
LICENSE       ← MIT 协议
```

## HTML 结构约定

### Tab 模块模板

每个功能模块遵循固定结构：

```html
<!-- 🎯 模块名 -->
<div class="tab-pane" id="p-{tabId}">
  <!-- 按钮组（可选） -->
  <div class="btn-group" style="margin-bottom:12px;">
    <button class="btn btn-primary" id="{tabId}ActionBtn" data-lk="{tabId}ActionBtn">🎯 操作</button>
  </div>
  <!-- 内容区 -->
  ...
</div>
```

### Tab 注册

在 HTML 顶部 tab 栏添加：

```html
<div class="tab" data-tab="{tabId}" data-lk="tab_{tabId}">🎯 模块名</div>
```

### 子 Tab（如需要）

```html
<div class="sub-tabs">
  <div class="sub-tab active" data-sub="{tabId}-{subId}" data-lk="sub_{tabId}_{subId}">子页签</div>
</div>
<div class="sub-pane active" data-sub="{tabId}-{subId}">内容</div>
```

## CSS 约定

### 变量

所有颜色、圆角使用 CSS 变量，**禁止硬编码颜色值**：

```css
/* 亮色主题 */
--bg, --surface, --surface2, --border, --text, --text-dim, --accent, --accent-dim, --red, --orange, --radius

/* 暗色主题 */
[data-theme="dark"] { ... }

/* JSON 语法高亮 */
--jt-str, --jt-num, --jt-bool, --jt-null
```

### 新增样式

- 用现有 class：`.btn`, `.btn-primary`, `.btn-group`, `.result-item`, `.result-box`, `.copy-btn-sm`
- 新增 class 写在 `</style>` 标签内，按模块分区注释
- 响应式用 `@media` 查询

## JavaScript 约定

### 代码组织

```javascript
// ═══════════════════════════════════════
//  🎯 模块名
// ═══════════════════════════════════════

// 核心逻辑函数
function doSomething() { ... }

// 渲染函数（更新 DOM）
function renderSomething(data) { ... }

// 事件绑定
$('btnId').addEventListener('click', () => { ... });
```

### 命名规则

| 类型 | 规则 | 示例 |
|------|------|------|
| ID 命名 | `{模块缩写}{功能}` | `jsonInput`, `screenRefreshBtn` |
| 函数命名 | 动词开头驼峰 | `renderColor()`, `refreshScreenData()` |
| 常量命名 | 全大写下划线 | `SRCH_CHUNK`, `REX_CHUNK_SIZE` |
| 临时变量 | 下划线前缀 | `_srchBackup`, `_lang` |

### DOM 操作

```javascript
// 获取元素（全局 $ 已定义）
const el = $('elementId');

// 安全设置文本
const safe = (id, val) => { const e = $(id); if (e) e.textContent = val; };

// 事件委托
document.addEventListener('click', (e) => {
  const btn = e.target.closest('.btn');
  if (!btn) return;
  ...
});
```

### 文件读取

使用项目封装的 `setupFileInput()` 或 `readDroppedFile()`，不要自己写 FileReader。

### Toast 提示

```javascript
toast('消息');        // 普通提示
toast('错误', 1);    // 错误提示（红色）
```

## 国际化（i18n）规范

**每个用户可见的文字都必须支持双语，无例外。**

### 翻译注册

在 `_L` 对象中添加：

```javascript
const _L = {
  // 按钮文本
  myBtn:{zh:'🎯 中文',en:'🎯 English'},
  // 标签文本
  myLabel:{zh:'标签',en:'Label'},
  // 占位符
  pl_myInput:{zh:'请输入',en:'Enter...'},
  // Toast 消息
  my_toast:{zh:'操作成功',en:'Done'},
  // 错误消息
  err_my_error:{zh:'出错了',en:'Error'},
};
```

### HTML 中使用

```html
<!-- 文本节点 -->
<button data-lk="myBtn">🎯 中文</button>

<!-- placeholder -->
<input data-lk-pl="pl_myInput" placeholder="请输入">

<!-- title -->
<button data-lk-tl="tl_myTip" title="提示">?</button>
```

### JS 中动态文本

```javascript
const L = (k) => _L[k]?.[_lang] || k;
el.textContent = L('my_label') + ': ' + value;
```

### 翻译 key 命名

| 类型 | 前缀 | 示例 |
|------|------|------|
| Tab 标题 | `tab_` | `tab_screen` |
| 子 Tab | `sub_` | `sub_screen_basic` |
| 按钮 | `{模块}Btn` | `screenRefreshBtn` |
| 标签 | `{字段}_label` | `scrRes_label` |
| 占位符 | `pl_` | `pl_jsonInput` |
| 标题提示 | `tl_` | `tl_themeToggle` |
| Toast | 无前缀 | `copied_toast` |
| 错误 | `err_` | `err_input_json` |

### 注意事项

- `_L` 中的 key 必须唯一
- 翻译值中的引号用 `\'` 转义
- 动态值（如竖屏/横屏）单独定义翻译 key，在 JS 中通过 `L('key')` 调用
- 数字、单位（px、bit）不翻译

## 新模块开发清单

1. **HTML**：在 `</div><!-- p-{上一个tab} -->` 后添加 tab-pane
2. **Tab 栏**：在 tab 栏添加 `.tab` 元素
3. **_L 翻译**：添加所有按钮、标签、提示的中英文
4. **JS**：在 `// ═══` 分隔线之间写模块代码
5. **事件绑定**：所有按钮绑定 `addEventListener`
6. **tabMeta**：如果希望 Ctrl+K 能搜到，在 `tabMeta` 数组中添加
7. **刷新数据**：切换到该 tab 时应自动加载/刷新数据

## 主题适配

- 新元素使用 CSS 变量，不要写死颜色
- 暗色主题下可能需要单独调整的颜色，写在 `[data-theme="dark"]` 里
- 测试：切换亮/暗主题各检查一遍

## 性能注意

- 大文件操作（>50MB）使用流式处理，参考搜索模块的 `srchStreamScan()`
- 避免在 `input` 事件中做重计算，用 debounce（参考正则模块的 `rexDelayed()`）
- 不要在循环中操作 DOM，用 `DocumentFragment` 批量插入

## 浏览器兼容

- 最低支持：Chrome 90+、Firefox 90+、Safari 15+
- 不用 `??=`、`&&=` 等新语法（兼容性差）
- `?.` 可以用（Chrome 80+）
- 需 HTTPS 或 localhost 才能用的功能（如 `crypto.subtle`）要加提示

## 提交前自检

- [ ] 所有用户可见文字都有 `data-lk` 或在 `_L` 中有翻译
- [ ] 亮色/暗色主题都正常显示
- [ ] Ctrl+K 搜索能搜到新模块（如需要）
- [ ] 切换中英文后新模块文字正确
- [ ] 无控制台报错
