# 毛线图谱编辑器 概要设计（HLD）V0

**状态**：V0，待评审。**输入**：[SRS](crochet-chart-editor-srs.md)（需求与验收）、[FSR](crochet-chart-editor-fsr.md)（约束与估算）。本文只承载架构决策与模块契约；需求行为以 SRS 为单一事实源，本文不复述。

**架构基线**（已确认的六个决策）：领域层零依赖 React、命令模式单 store、纯 SVG 只渲染非空格、PDF 只走打印管线、单测全覆盖 + 关键路径 E2E、Vite + TS strict 工具链。

## 1. 分层架构

```
┌─────────────────────────────────────────────┐
│ ui/            React 视图与交互（工具、面板） │   依赖方向：上 → 下
├─────────────────────────────────────────────┤
│ app/           store、命令分发、派生查询      │
├─────────────────────────────────────────────┤
│ core/          模型、命令、不变式（零依赖）    │   ← 可脱离 DOM 全量单测
├─────────────────────────────────────────────┤
│ infra/         IndexedDB、导出、打印、PWA     │
└─────────────────────────────────────────────┘
```

依赖只向下，`core/` 不 import 任何上层或第三方运行时库。SRS §4 的一致性不变式是 `core/` 的存在理由：所有修改必须经过命令入口，不变式由 core 保证，而非由 UI 约束。

## 2. 模块划分

| 模块              | 职责                                                         | 关键接口（签名示意）                                                                                                 |
| ----------------- | ------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| `core/model`      | 图谱数据结构与约束（SRS §4 实体表）                          | `createChart(opts): Chart`、`validate(chart): Invariant[]`                                                           |
| `core/commands`   | 命令定义 + 撤销栈                                            | `interface Command { apply(c: Chart): Chart; revert(c: Chart): Chart }`、`class UndoStack { do, undo, redo, depth }` |
| `core/legend`     | 图例派生（针法序排序、使用集同步）                           | `deriveLegend(chart): LegendItem[]`                                                                                  |
| `core/rowNumbers` | 行号派生（恒连续，不落存储）                                 | `deriveRowNumbers(chart): number[]`                                                                                  |
| `app/store`       | 唯一状态持有者，命令唯一入口                                 | `dispatch(cmd)`、`useChart()`、`useTool()`                                                                           |
| `ui/canvas`       | 指针交互 → 命令（铅笔拖动按"落笔区间"聚合成单命令）          | `<ChartCanvas />`                                                                                                    |
| `ui/*`            | 符号面板、颜色面板、工具栏、文件列表、导出对话框、新建对话框 | 各 React 组件                                                                                                        |
| `render/svg`      | Chart → SVG 元素（无 React 依赖的纯函数，供画布与导出共用）  | `renderChart(chart, opts): SVGElement`                                                                               |
| `infra/storage`   | 图谱仓库（IndexedDB）、自动保存、JSON 工程导入导出           | `interface ChartRepository { load, list, save, remove }`                                                             |
| `infra/export`    | PNG / SVG / 打印（PDF）三条导出管线                          | `exportPng(chart, scale)`、`exportSvg(chart)`、`printChart(chart)`                                                   |

**关键设计点**：

- **命令即事实**：`Chart` 不可变（命令返回新图），撤销栈存命令序列而非快照——SRS FR4.3 的"50 步两态一致"由此保证（对照 FSR 风险表：这是对交互复杂度风险的主要对冲）。
- **拖动聚合**：一次拖动绘制产生一个 `SetCellsCommand`（含全部格子的前后值），撤销一步抹掉整笔。
- **render/svg 纯函数**：画布渲染与导出渲染是同一段代码的两次调用，消灭"所见非所导"。
- **派生不落存储**：行号、图例均为纯函数派生（SRS §4 不变式的实现位置），撤销/重做后自动成立，无同步问题。

## 3. 渲染策略（性能预算：NFR3，100×100 交互 < 100ms）

- 只渲染非空格子；空态格子不产生 DOM 节点。
- 网格线合并为单个 `<path>`（每行两条横线 + 每列两条竖线一次绘制）。
- 每行一个 `<g class="row">` 分组，符号为 `<use>` 引用符号库 `<defs>`（同符号复用渲染器，Stitch Works 同款思路）。
- 交互期间（拖动落笔）只更新受影响格子，不做全量重渲染。
- 实测不达标时的降级阶梯：① 行分组虚拟化（只渲染视口内行）→ ② 拖动期间改用 Canvas 覆盖层预览、落笔后回 SVG。不引入 Konva 等画布库（保持纯 SVG 的缩放/导出/文本编辑优势）。

## 4. 数据结构（TypeScript 形状，约束以 SRS §4 为准）

```ts
interface Chart {
  id: string;
  name: string;
  rows: number;
  cols: number; // ∈ [1,200]
  cells: Cell[]; // 行优先，rows × cols
  marks: RepeatMark[];
  formatVersion: number;
}
interface Cell {
  symbolId: string | null;
  lineColor?: Color;
  bgColor?: Color;
} // null = 空态
interface SymbolDef {
  symbolId: string;
  svg: string;
  cnName: string;
  usTerm: string;
  ukTerm: string;
  order: number;
}
interface RepeatMark {
  id: string;
  start: [r, c];
  end: [r, c];
  label: string;
} // 仅视觉
```

工程文件 = `Chart` + `SymbolDef` 引用的 JSON 序列化（FR7.3：含 `formatVersion`，高版本拒绝导入）。符号库内置为静态数据（FR3.3：新增符号 = 一条 `SymbolDef` + 一个 SVG）。

## 5. 存储与自动保存

- IndexedDB 两表：`charts`（列表元数据 + 缩略图）、`chart_data`（Chart JSON）。
- 自动保存：命令提交后 500ms 防抖写入（FR7.1）；写失败（配额/权限）→ `store` 置错误态 → UI 顶部告警条，**不静默**。
- 缩略图：图表变更后防抖重渲染小尺寸 SVG 存入 `charts` 表（FR7.2）。

## 6. 导出管线（FR8）

| 格式 | 管线                                                                                                              | 要点                                                               |
| ---- | ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| SVG  | `renderChart` 序列化 → Blob 下载                                                                                  | 文字保持 `<text>`（FR8.2）；符号经 `<defs>/<use>` 展开为自包含文件 |
| PNG  | SVG Blob → `Image` → `canvas`（倍率 1x/2x/4x）→ `toBlob`                                                          | 2x 默认；图例一并渲染进画布                                        |
| PDF  | 专用 `/print` 路由 + print CSS（`@page` A4 纵向；页眉图名/图谱/图例/页脚页码由 DOM+CSS 排版）→ 浏览器"另存为 PDF" | 只承诺 A4 可打印（FR8.3）；svg2pdf.js 不进一期                     |

## 7. 出错处理

| 故障                            | 行为                                                            |
| ------------------------------- | --------------------------------------------------------------- |
| 存储写失败                      | 顶部告警条 + 重试入口；内存态保留，可导出 JSON 兜底             |
| 工程文件解析失败/版本过高       | 导入对话框内联报错，拒绝对文件做任何写入                        |
| 导出失败（如超大 PNG 内存不足） | 对话框报错并建议降倍率（Stitch Fiddle 同款"too large"提示思路） |

## 8. 工程约定（AI-first）

- **工具链**：Vite + TypeScript strict + Vitest + Playwright + ESLint/Prettier + pnpm（Q6）。
- **护栏**：`tsc --noEmit` + lint + 单测进 pre-commit（用仓库 setup-pre-commit skill 落地）；单测即验收——SRS §4 不变式与 FR 验收标准逐一映射为 `core/` 测试用例名。
- **可导航**：目录即分层（`src/core|app|ui|infra`）；领域词汇（图谱/格子/空态/重复标记/针法序）按仓库约定入 `GLOSSARY.md`。
- **E2E 关键路径**（3–5 条，Playwright）：① 画图→撤销/重做→刷新不丢；② 行列增删后行号连续；③ PNG/SVG/打印三种导出产物存在且含图例；④ JSON 导出→导入逐格一致。

## 9. 里程碑映射（承接 SRS §7）

| 里程碑 | 本设计交付物                                              |
| ------ | --------------------------------------------------------- |
| M0     | `core/model` + `render/svg` 静态渲染 + 数据结构定型       |
| M1     | `core/commands` + `app/store` + `ui/canvas` 绘制交互      |
| M2     | 符号库数据 + `core/rowNumbers` + 结构编辑命令 + `ui` 面板 |
| M3     | `infra/storage` + `infra/export` 三管线                   |
| M4     | 渲染策略降级阶梯按实测启用 + PWA 离线缓存 + 性能达标      |
