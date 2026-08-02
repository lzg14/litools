# litools 第二轮功能扩展实施计划

**目标**：新增 8 个实用功能，全部保持单文件零依赖 + 中英双语 i18n 特色

**规划日期**：2026-08-02

## 功能归属

| 功能 | 归属模块 | 形式 |
|------|---------|------|
| SQL 格式化 | SQL 模块 | 新增子页签 `sql-fmt` |
| XML ⇄ JSON | JSON 模块 | 新增子页签 `json-xml` |
| JSON 转代码 | JSON 模块 | 新增子页签 `json-code` |
| 大小写/命名转换 | 文本模块 | 新增子页签 `case` |
| Code Formatter | 生成模块 | 新增子页签 `fmt`（JS/CSS/HTML 美化压缩）|
| 正则解释器 | 生成模块 | 并入 `rex` 子页签（新增解释按钮）|
| URL 解析器 | 编解码模块 | 并入 `url` 子页签（新增解析结果区）|
| 颜色增强 | 生成模块 | 并入 `color` 子页签（新增 CMYK/对比度）|

## 步骤

- [ ] Step 1: SQL 格式化 — 新增 `sql-fmt` 子页签（textarea + 格式化按钮 + 结果区），实现缩进/关键字大写/子句换行
  - verify: 输入 `select a,b from t where x=1` 输出格式化 SQL；`node checksyntax` 通过

- [ ] Step 2: XML ⇄ JSON — 新增 `json-xml` 子页签（双向转换），DOMParser 解析，JSON→XML 手写序列化
  - verify: 转换 `{"a":1,"b":[2,3]}` 得到合法 XML，反向正确；语法检查通过

- [ ] Step 3: JSON 转代码 — 新增 `json-code` 子页签（TS 接口 / Python dict / Go struct 三个目标 + 生成按钮）
  - verify: 转换嵌套对象到三种目标格式，结构正确；语法检查通过

- [ ] Step 4: 大小写/命名转换 — 新增 `case` 子页签（camelCase/snake_case/kebab/Pascal/SCREAMING 五种 + 一键复制）
  - verify: 输入 `hello_world` 得到 `helloWorld`/`HELLO_WORLD`/`hello-world` 等；语法检查通过

- [ ] Step 5: Code Formatter — 新增 `fmt` 子页签（JS/CSS/HTML 美化 + 压缩两模式）
  - verify: 压缩 JS 美化后缩进正确；CSS/HTML 也能格式化；语法检查通过

- [ ] Step 6: 正则解释器 — `rex` 子页签新增"解释"按钮，解析常见结构（字符类/量词/锚点/分组）为中文说明
  - verify: `\d{4}-\d{2}` 解释为"4位数字-2位数字"；语法检查通过

- [ ] Step 7: URL 解析器 — `url` 子页签新增"解析"按钮，输出 protocol/host/path/query 键值表
  - verify: 输入含 query 的 URL，正确拆出各组件；语法检查通过

- [ ] Step 8: 颜色增强 — `color` 子页签新增 CMYK 值 + 前景/背景对比度计算结果
  - verify: 输入颜色显示 CMYK 与对比度值；语法检查通过

- [ ] Step 9: 全量验证 + tabMeta/CLAUDE.md 文档同步
  - verify: `check_dups.js`（无重复 ID/无缺失引用）+ `checksyntax.js`（SYNTAX OK）+ `_L` 无重复键；feature-plan.md 更新

## 不做什么

- 不改动现有功能逻辑
- 不引入外部库
- 不碰 PWA/断点测试（已移除/已实现）
- 正则解释器不追求完整解析，覆盖常见结构即可

## 执行模式

单 agent 串行（依赖同一文件 index.html，并行编辑有冲突风险）
