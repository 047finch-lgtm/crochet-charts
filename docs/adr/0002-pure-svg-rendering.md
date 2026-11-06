# 渲染用纯 SVG，不用 Canvas/Konva

图谱渲染走 SVG DOM：只渲染非空格、网格线合并单路径、符号经 `<defs>/<use>` 复用。选 SVG 是为了三件 Canvas 给不了的东西：缩放无损（SRS FR1.1）、导出即所见（画布渲染与导出共用同一纯函数）、文字保持可编辑（FR8.2）。HookChart 选 Konva（Canvas）后高清导出成了它自认的未完成项；knitscape 迭代过 Canvas 后最终也收敛到 SVG 输出。

## Considered Options

- **Canvas（Konva/自绘）**：4 万格场景下性能上限更高，但导出需另一套渲染、文字要转轮廓、缩放要重绘。
- **SVG + Canvas 混合**：复杂度双倍，只在纯 SVG 实测不达标时作为降级阶梯启用（HLD §3）。

## Consequences

- 性能预算 NFR3（100×100 交互 < 100ms）必须用渲染策略（非空格过滤、行分组）达成，而非换渲染引擎。
- 实测不达标的降级顺序已定：行虚拟化 → 拖动期 Canvas 预览层；不引入第三方画布库。
