# JSON 智能修复（Smart Repair）实施计划

**目标**：粘贴任意"脏 JSON"（转义字符串、toString 产物、单引号、尾逗号、无引号 key、带杂质前缀等）到 JSON 模块，点一次格式化（或直接粘贴）就能自动修复并树形展示，达到 json.cn 级"粘贴即看"体验。

## 核心设计

新增纯函数 `smartRepairJSON(text)`，位于 index.html 的 JSON 模块区（用标记注释包裹，便于测试脚本抽取）：

```
返回 { ok, text, steps: [], error }
```

级联修复管线，**每一步都用 `JSON.parse` 实测验证**，失败则尝试下一步，全部失败返回原始错误：

| 序 | 步骤 | 典型输入 |
|---|------|---------|
| 0 | 严格解析（零成本快路径） | 标准 JSON |
| 1 | 解包字符串层（循环 ≤5 层） | `"{\"a\":1}"` |
| 2 | 无引号转义还原（Java 日志风格） | `{\"a\":1}` 带 `\"` 无外层引号 |
| 3 | Python 字典风格（单引号 + True/False/None） | `{'a': True}` |
| 4 | Java toString 风格（`=` 代替 `:`、裸词） | `{key=value, list=[a, b]}` |
| 5 | 宽松修复（逐项验证）：去注释 → 去尾逗号 → 补无引号 key → 单引号转双引号 | `{a:1,}` |
| 6 | 提取嵌入 JSON（从杂质中截取首个平衡的 `{...}`/`[...]`） | `resp: {...} 200 OK` |

## 步骤

- [x] Step 1: 实现 `smartRepairJSON()` 级联修复纯函数（标记注释 `/* SMART-REPAIR-START/END */` 包裹）
  - verify: `V:\json-repair-test.mjs` 从 index.html 抽取该函数，node 跑 15+ 用例全绿（标准不变 / 多层解包 / Java toString / Python / 尾逗号 / 单引号 / 无引号 key / 杂质提取 / 不可修复返回原错误）

- [x] Step 2: 接入 `renderJSONResult()` 与格式化按钮：strict 失败 → 自动走 smartRepair；修复仍失败才显示行列错误定位（保留现有 `fmtJSONError`）
  - verify: 浏览器手测 3 类脏输入均出树；故意输入坏 JSON 仍显示第 N 行第 N 列 + 箭头

- [x] Step 3: 格式化后默认全展开：先统计节点数，节点数 < 阈值（2000）→ 全展开渲染；≥ 阈值 → 保持折叠懒加载（性能保护），提示可右键全部展开
  - verify: 小 JSON 格式化后所有层级直接展开；构造 >2000 节点 JSON 格式化不卡顿、保持折叠；右键"全部展开"仍可用

- [x] Step 4: 结果区顶部"已自动修复"提示条：列出应用步骤 + 写回输入框按钮，`_L` 双语（key 前缀 `rp_` / `jsonRepaired`）
  - verify: 切换中英文提示正确；点写回后输入框变为修复后的标准 JSON，再格式化无提示条

- [x] Step 5: 粘贴自动格式化（`paste` 事件 debounce 300ms 触发智能格式化，仅 JSON tab）
  - verify: 粘贴脏 JSON 不点任何按钮直接出结果

- [x] Step 6: 大文件路径兼容：>1MB worker 失败 fallback 本来就走 `renderJSONResult` → 自动获得修复；>10MB 保持纯文本不修复
  - verify: 生成 2MB 带尾逗号 JSON 拖入，能成功格式化；代码走查确认调用链

- [x] Step 7: README 功能表 JSON 行更新 + git 提交（feat: JSON 智能修复 + 默认全展开）
  - verify: README 含"智能修复"描述；`git -C D:/ProjectFile/litools log -1` 显示提交

## 不做什么

- 不实现完整 JSON5 规范，覆盖常见脏格式即可
- 不改 压缩/YAML/XML/Schema 等其他按钮逻辑（仍 strict）
- 不引入第三方库、不拆分文件（铁律）
- 不自动改写用户输入框内容（修复只发生在结果区，写回需手动点击）

## 执行模式

单 agent 串行（所有改动集中在 index.html 一个文件，避免并发编辑冲突）。
