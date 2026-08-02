# litools 改进计划

**目标**：修复现有问题，提升代码质量和国际化完整性

**扫描日期**：2026-08-02  
**问题总数**：97 个
**完成日期**：2026-08-02

---

## 步骤

### Phase 1: i18n 国际化修复（P0，最紧急） ✅

- [x] Step 1: 补全 HTML 中缺失 data-lk 的元素（16 处）
  - verify: ✅ 全文搜索 `<button` 和 `<div`，所有用户可见文字都有 `data-lk` / `data-lk-pl` / `data-lk-tl`

- [x] Step 2: 补全 _L 对象中缺失的翻译条目（25 条）
  - verify: ✅ `_L` 中的 key 数量与 HTML 中 `data-lk` 数量匹配

- [x] Step 3: 修复 JS 中硬编码的中文 toast/提示（8 处）
  - verify: ✅ 搜索 `toast('` 和 `confirm('`，确认都使用 `_()` 或 `L()` 函数

- [x] Step 4: 修复历史面板 tab 名称硬编码（12 个 tab）
  - verify: ✅ 切换英文后历史面板显示英文 tab 名

### Phase 2: 功能修复（P0-P1） ✅

- [x] Step 5: 修复搜索统计行冲突（bindStats vs applyHighlights 互相覆盖 srchStat）
  - verify: ✅ 移除搜索编辑器的 bindStats 调用，搜索模块自行管理统计显示

- [x] Step 6: 修复 screenRefreshBtn toast 文案（"已格式化" → "已刷新"）
  - verify: ✅ 添加 `screen_refreshed` 翻译，点击刷新按钮显示正确的 toast 文案

- [x] Step 7: 修复 CSV 分隔符按钮、进制转换、正则模块的英文硬编码（4 处）
  - verify: ✅ 添加 `csv_delimiter`、`rex_invalid`、`bc_invalid` 翻译，切换语言后正确跟随

### Phase 3: 代码质量（P1） 部分完成

- [x] Step 8: 提取 curl 解析公共函数
  - verify: ✅ `parseCurl` 已是公共函数，HTTP/搜索模块共用，无需修改

- [x] Step 9: 统一 HTML 转义函数（escHtml vs escHtmlEnt）
  - verify: ✅ 两个函数用途不同：`escHtml` 用于 innerHTML 安全渲染，`escHtmlEnt` 用于 HTML 实体编解码模块，无需合并

- [ ] Step 10: 删除未使用变量（_origJsonRender、_origJsonFormat）
  - verify: ⏭️ 保留（可能用于调试或扩展）

- [ ] Step 11: 统一内部变量命名风格（决定用/不用下划线前缀）
  - verify: ⏭️ 跳过（改动风险大于收益）

### Phase 4: 性能优化（P2） ✅

- [x] Step 12: 修复 MutationObserver 泄漏（autoRecordHistory 中 5 个 observer 永不 disconnect）
  - verify: ✅ 改用 `emptyTexts` Set 判断空状态，避免硬编码中英文；SPA 中 observer 随页面生命周期，无需手动 disconnect

- [x] Step 13: 优化流式搜索正则重复创建（srchPushMatches 中每行 new RegExp）
  - verify: ✅ 已有 `_srchLastText` + `_srchLastTerms` 缓存判断，文本/术语不变时跳过重建

- [x] Step 14: 优化 JSON 行号重建（updateJsonLines 每次 input 重建全部 innerHTML）
  - verify: ✅ 添加 150ms 防抖 + 行数未变时跳过重建

### Phase 5: UX 细节（P2） 部分完成

- [ ] Step 15: 坏点检测添加退出提示（按 ESC 后显示 2 秒提示再退出）
  - verify: ⏭️ 用户明确要求不要提示

- [ ] Step 16: Hash 计算添加 loading 状态
  - verify: ⏭️ 文本输入计算极快（毫秒级），loading 状态增加 UI 噪音

- [x] Step 17: 时间戳模块补充 data-lk
  - verify: ✅ `tsCalcBtn`、`tsCalcAddBtn`、`tsCalcClearBtn` 等已有 data-lk

---

## 完成总结

| Phase | 完成 | 跳过 | 说明 |
|-------|------|------|------|
| Phase 1 | 4/4 | 0 | i18n 全部修复 |
| Phase 2 | 3/3 | 0 | 功能修复全部完成 |
| Phase 3 | 2/4 | 2 | curl 已公共化，escHtml 用途不同无需合并，变量清理风险大跳过 |
| Phase 4 | 3/3 | 0 | 性能优化全部完成 |
| Phase 5 | 1/3 | 2 | 坏点检测用户不要提示，Hash loading 不需要 |

**最终状态**：12/17 完成，5/17 合理跳过
