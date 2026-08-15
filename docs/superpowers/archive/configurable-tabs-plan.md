# 可配置 Tab 栏 实施计划

**目标**：功能全部保留，让每个用户自定义 tab 栏显示哪些模块（隐藏的 tab 仍在功能里、Ctrl+K 可搜到），兼顾个人精简与其他用户需求。

## 现状

- tab 栏是 HTML 写死的 10 个 `.tab` 元素（line 318-327）
- 已有 `litools_pinned`（固定工具栏，设置面板 `settingsPinList` 勾选），`ALL_MODULES`（line 3946）
- `switchTab()`（line 1223）按 id 操作，restoreTabResult 依赖 id
- 设置面板 HTML：line 839-859

## 步骤

- [ ] Step 1: JS 添加 `getHidden()/setHidden()`（localStorage `litools_hiddenTabs`，默认 `[]`）+ `renderTabs()`：遍历 tab 栏 `.tab[data-tab]`，hidden 的加 `display:none`
  - verify: 手动调用 `setHidden(['sql','gen'])` 后 `renderTabs()`，对应 tab 不可见

- [ ] Step 2: 设置面板新增"显示模块"区块（勾选 ALL_MODULES，默认全勾），change 时更新 hidden 并 `renderTabs()`
  - verify: 设置面板取消勾选"SQL"→ tab 栏 SQL 消失；重新勾选→ 恢复

- [ ] Step 3: 隐藏的 tab 若为当前 activeTab，需切回第一个可见 tab（避免空白）；Ctrl+K 搜索结果仍全部显示（不过滤 tabMeta）
  - verify: 隐藏当前所在 tab 时自动切到可见 tab；Ctrl+K 仍能搜到并跳转隐藏 tab

- [ ] Step 4: 新增 _L 翻译（设置区标题/提示中英文）+ README 特性说明
  - verify: 切中英文文案正确；README 有该特性

- [ ] Step 5: headless Chrome 验证（隐藏/恢复/SwitchTab 兜底/Ctrl+K/中英文）
  - verify: 全程无报错，行为符合预期

## 不做什么

- 不删除任何功能代码
- 不改 tabMeta 搜索范围（隐藏的仍可搜索）
- 不改已有 litools_pinned 机制