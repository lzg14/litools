# 移除 Tab 栏，固定工具栏上位为主导航 实施计划

**目标**：删除顶部 tab 栏，pinnedToolbar（固定工具栏）成为唯一模块导航，默认显示全部模块；设置面板合并"工具栏固定"与"Tab 栏显示"为一个勾选区。

## 现状

- tab 栏：HTML 静态 `.tabs` 块（line 318-328），10 个 `.tab` 元素
- pinnedToolbar：`renderPinnedToolbar()` 按 `getPinned()`（`litools_pinned`）渲染，默认空
- `switchTab()`：同时操作 `.tab` 和 `.tab-pane` 的 active 类
- 设置面板：`settingsPinList`（固定）+ `settingsTabList`（显示）两区块
- sessionStorage 恢复 activeTab（3833）查 `.tab[data-tab]`

## 步骤

- [ ] Step 1: HTML 删除 tab 栏 `.tabs` 块（318-328）；顶部标题栏去掉 tabs 容器
  - verify: grep 无 `.tab active data-tab="json"` 元素

- [ ] Step 2: `getPinned()` 默认返回 ALL_MODULES 全部 id（首次访问全选）；`renderPinnedToolbar()` 按钮加 active 态（当前模块高亮），删除 `renderTabs`/`getHiddenTabs`/`setHiddenTabs`
  - verify: 首次加载 pinnedToolbar 显示 10 个模块按钮，无 `.tab` 报错

- [ ] Step 3: `switchTab()` 移除 `.tab` 相关操作，改为更新 pinnedToolbar 按钮 active 态；sessionStorage 恢复（3833）改用 `switchTab()` 或直接设 active；删除 `.tab` click 绑定（1238）
  - verify: 点击 pinnedToolbar 按钮正常切换，恢复 activeTab 正常

- [ ] Step 4: 设置面板删除"Tab 栏显示"区块，`settingsPinList` 改名"模块显示"提示文案；`renderSettings()` 只渲染一个勾选区（控制 pinned）
  - verify: 设置面板只有"模块显示"一个勾选区，取消勾选模块后导航中消失

- [ ] Step 5: 移除 `.tabs`/`.tab` CSS（或保留给子tab不影响）；更新 _L 翻译（settings_pin 相关文案）；README 更新
  - verify: 无 `.tab` 残留引用（除 .tab-pane/.sub-tab）；双语正确

- [ ] Step 6: headless Chrome 验证（默认全显示、勾选隐藏、切换、Ctrl+K 跳转、sessionStorage 恢复）
  - verify: 全程无报错，行为符合预期

## 不做什么

- 保留 `.tab-pane` 显隐逻辑（核心布局）
- 保留 Ctrl+K 搜索（tabMeta 不变）
- 不删除任何功能模块代码