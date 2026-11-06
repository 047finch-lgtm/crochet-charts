# PDF 走浏览器打印管线，不用前端 PDF 库

"导出 PDF" 实现为专用打印路由 + print CSS（`@page` A4 排版：页眉图名、图谱、图例、页脚页码），由浏览器"另存为 PDF"产出文件；不引入 jsPDF/svg2pdf.js 生成 PDF。同构先例：Stitch Fiddle 的 `/print` 路由 + `beforeprint` 监听、Chart Minder 的 "print it"。这是一条**有意偏离**——用户预期一个"下载 PDF"按钮，请勿"修复"为前端生成。

## Considered Options

- **jsPDF + svg2pdf.js**：可一键下载，但排版（页眉/图例/分页）要自己写一套，A4 双浏览器一致性是无底洞；Stitch Fiddle 在 bundle 里也没走这条路。
- **打印管线（选定）**：排版交给浏览器打印引擎，成本近零；代价是多一步"另存为"，且只承诺 A4 可打印、不承诺像素级一致（SRS FR8.3 已如此约定）。

## Consequences

- 打印样式表是导出质量的全部责任面，Chrome/Safari 打印预览进 E2E 关键路径。
- 若将来用户强需求"一键下载 PDF"，优先在打印管线外加一个薄封装，而不是换生成器。
