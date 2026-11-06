# 毛线图谱/编织图解工具技术栈调研·事实清单

> 事实与来源线索，不下结论。**加粗**为已核实证据，「未证实」为公开渠道未查到/存疑项。
> 配套文档：[竞品格局与领域事实](crochet-chart-editor-landscape.md)。

## 1. Stitch Fiddle（stitchfiddle.com，Web SaaS）

- **前端框架：React + React Router**（Prospeo 技术栈页列出 React/React Router/Lodash/core-js；主 bundle `main-bcc098195afd0bda426a.js`≈1.6MB，"react" 字样 123 处，vue/angular 为 0）
- **构建：webpack**；**CSS 用 LESS**（`cdn.stitchfiddle.com/…/style/less/layout.css`）
- **PWA**：有 `manifest.json`（standalone、maskable 图标）；多语言路由 `/:language/…`（en/nl hreflang）
- 渲染：bundle 内 svg 字样 256 处 vs canvas 58 处；`createElement("canvas")` 8 处均在图片处理逻辑（导入图片减色）；**未发现 konva/fabric.js/pixi 等画布库**；编辑器路由 `/:language/c/:chartId`，另有独立 **`/:language/c/:chartId/print` 打印页路由 + `window.addEventListener("beforeprint",…)`**
- 导出：导出弹窗文案（bundle 内 i18n）：**"File type: Image / PDF document (use for printing) / Vector"**，压缩选项 **"No (.svg) / Yes (.svgz)"**；付费对比表出现 "Download charts as vector file (.svg, .svgz, .eps, .ps, .pdf, .wmf, .emf, .emz)"；限制文案 "PDF: Cropped (vector or rasterized)"、"chart is too large for this file type"。**bundle 中无 jsPDF/pdfmake/svg2pdf/html2canvas 字样，也无 `application/pdf` Blob 组装迹象**（PDF 来源=打印管线或服务端，未能二选一；导出弹窗带 progress tracker 文案）
- 导入：明确不支持 PDF 上传（i18n "IMPORT_PICTURE_HELP_NO_PDF"）
- 后端/托管：荷兰 **TransIP**（Amsterdam 单服务器）；HTTP `server: SF`（自建 banner）；SSL Sectigo；订阅 Premium $5.50/月 或 $33/年（LinkedIn 帖引用）

## 2. Stitchmastery（Java 桌面）

- **UI 框架：Eclipse RCP + SWT**（macOS 包内 **`Contents/Eclipse`** 与 `Stitchmastery.ini` 含 `-Xmx2g`；更新走 **p2、Eclipse 证书签名、插件 feature `com.stitchmastery.chartEditor.feature`**；Linux 包名 `stitchmastery-?-linux.gtk-x86_64.zip`）
- **内嵌 JRE**；64-bit only；默认 JVM heap 2GB，大图需手改 `-Xmx`（手册明示内存痛点）
- 工程格式：**.knt2**（v2/v3；v1 扩展名 `.knit` 需另存转换）；用户针法库 **.smlib**；**针法库通过 alias（库名+用户 namespace）间接引用**；可选自动备份 `*.knt2.bak`；一个 knt2 可含多张 chart 但仅一个 key。**内部结构官方未公开，未检索到第三方逆向资料**
- 外观体系：appearance stylesheets + stitch libraries（v3 库格式；v2 库被忽略并回退 'Dot' 库）
- 导出：**JPEG、PNG、PDF、SVG、EPS**；v3 起矢量格式可选"文字输出为 text 而非轮廓"；自动生成 knitter-friendly 文字说明（Output Text 视图）

## 3. Stitch Works Crochet Charts（github.com/StitchworksSoftware/CrochetCharts，开源）

- **技术栈：Qt 4.8 + C++11**（CMake `find_package(Qt4 REQUIRED)`；Windows 部署清单 `QtSvg4/QtCore4/QtGui4/QtNetwork4/QtXml4.dll` + MinGW + NSIS；另有 Hunspell、docbook/Apache FOP 文档管线）；**GPLv3**（针法图稿 CC BY-SA 4.0）；版权年 2010–2014
- 架构：**QGraphicsScene/QGraphicsView**（`src/scene.cpp` 2719 行，全库最复杂文件）+ 多标签 + **QUndoStack 命令模式**（crochetchartcommands.*）+ 停靠面板
- 渲染：**真 SVG 符号**——针法库为 190 个 SVG 文件（crochet.xml 890 行描述针法集）；`stitch.cpp` 用 **QSvgRenderer**，按颜色做 SVG 源码字符串替换后缓存 renderer，`cell.cpp:170` 以 `QGraphicsSvgItem::setSharedRenderer` 矢量绘制
- 导出：`exportui.cpp` 明示 **`#include <QPrinter> //for pdf`、`#include <QSvgGenerator> //for svg`**；ExportUi::exportPdf/exportSvg/exportImg + 图例单独导出 + 页眉页脚渲染
- **.pattern 格式**：Qt **QDataStream 二进制容器** = 内嵌针法图标流 + **XML 字节块**（QXmlStreamReader 解析，元素含 colors/chart/stitch_set/grid/indicator/cell/chart_image）；file_v1/file_v2 双版本兼容；MIME `vnd.stitchworks.pattern`
- 已知 bug：网格吸附、镜像翻转缺陷；相关代码位置：`src/mirrordock.cpp`、`src/ChartItemTools.cpp`、`src/scene.cpp`
- 事实性提示：Qt4 已停止维护；仓库 344 文件≈10 万行（其中 SVG 73%）

## 4. KnitBird（Adobe AIR）

- **Adobe AIR 桌面应用**（ActionScript/Flex 世代；官网明示需先装 Adobe AIR）；导出 JPEG/PDF、导入图片转图解；工程文件扩展名 **.knit**（仅 LinkedIn 帖提及，官方未证实）
- 现状：**2019 年起免费**（密钥 `free.knitbird`，仍需联网激活）；**开发与支持已终止**；安装包 v2.0.23 携 2017 年时间戳
- 技术债：**Adobe 2019-06 将 AIR 移交 HARMAN，2020 年底 Adobe 停止一切支持**（Adobe 官方博客）；依赖"激活服务器仍在线"；无厂商修补后续 OS 断裂
- 系统要求（官网）：Windows Server 2008/7/8.1/10、Mac OS X 10.7+

## 5. Chart Minder（chart-minder.com，Web）

- **LinkedIn 公司页"Tech Stack: angular, mongodb"**；本次 curl 直访落到 **Vercel Security Checkpoint**（站点本体框架未直接验证）；帮助中心 **Tawk**；支付 **Stripe**；独立开发者 1 人公司（伦敦）
- 形态：Web 云服务 + 免费/Pro 订阅；2012 起步、2014 上线
- 导出：**"Export your chart as an image"** + "print it"；Pro 提供更高分辨率下载、私有图解、复制粘贴/翻转旋转/查找替换；照片扫描成图解、stitch view、gauge 计算

## 6. HookChart（钩针王国，2026 Web beta）

- 地址 `hookchart.crochetkingdom.cn`；**Free Beta**（2026-09 公告）
- **技术栈指纹（抓取源码）**：**Vite 构建** + **React** + **Konva**（`canvas-*.js` 282KB 中 "konva" 128 处、"react-konva" 6 处）→ **Canvas 方案（Konva 场景图）**，非 SVG DOM
- 产品事实：无限画布、**矢量路径**、公共/个人针法库（可自制符号）、网格/辅助线/对齐、方格与环形结构、图层、**工程保存（账号云存 + 本地工程下载/再导入）**、导出 PNG/JPG（**高清导出、AI 推荐未完成**）；绘图需登录；后端未公示（账号体系证明存在服务端）

## 7. 编织助手（iOS，App Store id6744092783）

- 元数据（iTunes Lookup API）：**bundleId `com.ilanlan.line`**，v2.1.7（2026-11-04 更新，2025-04-16 首发），**iOS 15.6+**，94.8MB，10 语言，免费+订阅（IAP）
- 技术栈：**未检索到公开资料证实 Swift/SwiftUI**；事实线索：iPad 优先、**iCloud 备份/迁移 App 数据**
- **PDF 识别线索（App Store 更新说明）**：v1.9.6 **"自动绘制 - 新增 PDF 图解区域自动识别与吸附，棒针符号识别支持确认、修正与记忆"**；v1.9.5 "PDF 图解网格校准"；功能形态 = PDF 页面渲染 + 网格校准/区域识别 + 符号识别（人工确认-修正-记忆），非端到端 OCR；底层实现未公开
- 其他事实：像素图/提花图/棒针图解；钩针图编辑（符号沿直线/圆/椭圆/多边形/螺旋排布）；编织笔记；行圈针计数；毛线库存；导出图片/PDF 可带颜色图例+针法图例

## 8. 团团织（微信小程序）

- **个人开发者**；类目"内容资讯/兴趣社区"；2026-09 被收录
- 开发方式线索：**TRAE 官方社区参赛帖**（2026-07）——"**AI 生成初版 → 手动审查修正 → AI 辅助调试**"（TRAE AI IDE 辅助）
- 功能事实：数行计数器、**图解转 3D**（文本图解→3D 球体预览）、图片转像素图（匹配毛线色号）、图解编辑器（方形/圆形/自由画布 + 钩织符号库 + 分享）、免费图解库、20 课教程
- **框架（原生/Taro/uni-app）公开信息无法判定**
- 同域旁证（勿混淆）：**"织团团"**（zhituantuan.cn，Web）是不同产品

## 9. 开源参考实现

### craftoid/sconcho（棒针，PyQt4 桌面，GPLv3，2009–2012）

- **Python + PyQt4**（QtGui/QtXml/QtSvg）；**QGraphicsScene 画布**（PatternCanvas 2560 行）+ QUndoStack（undo_framework.py 1066 行）+ QThread 后台保存
- **符号 = 目录（SVG 文件 + `description` 元数据文件）**；自定义符号以 **zip 分发**
- 工程格式：**自定义二进制 QDataStream（magic `0xA3D1`，API_VERSION 3）**，分节写 patternGridItems/legendItems/colors/activeSymbol/settings/patternRepeats/rowRepeats/textItems 等（io.py，含 API 1/2/3 三代读写兼容）
- 导出：**QSvgGenerator 生成真 SVG**，否则 QImage ARGB32 光栅；**打印/PDF 走 QPrinter**；可设 DPI

### t0mpr1c3/knotty（棒针 DSL，Typed Racket，GPLv3）

- **DSL 文本描述花样**，**Knitspeak 语法**导入/导出（knitspeak-parser.rkt 471 行）；可从彩色图形直生成 Fair Isle（`import-png`）
- 数据模型：Pattern + rowspec/rowmap/rowcount、**knitgraph.rkt（线圈拓扑图模型）**、yarn.rkt（多色线）
- 序列化：**XML + XSD**（knotty.xsd 617 行）；针法库 knotty-stitch.xml 5442 行
- 输出：**HTML**（交互图解+文字说明）、**PNG**、CLI 格式转换；无 SVG 输出

### textiles-lab/knitdb（CMU Textiles Lab，数据集）

- 机器编织样片数据集（**KnitPick，UIST 2019**）；**`charts/*.chart` = JSON-based stitch graph**（README 自述 "undocumented and idiosyncratic"）；`knitout/*.knitout`（机器指令）→ **smobj**
- stitch graph 是比网格更通用的图解数据模型（fossasia/kniteditor 格式规范同样主张"不能以网格为基础"）

### knitscape/knitscape（棒针/机器针，浏览器，MIT，CHI 2024）

- **纯前端 JS、最小依赖**（"client-side webapp written in JavaScript"）；README：**"renders to canvas with D3 force simulation"**；纱线级模拟（TopoKnit 线圈拓扑 + 弹簧模拟）**最终渲染为 SVG**（论文：迭代过 canvas 与 SVG 两版后弃用 canvas 输出）
- **数据模型：JSON workspace 三要素** = base repeat（操作网格）+ color sequence + needle positions；**符号与颜色分离**；JSON 可回读
- 导出：JSON / BMP / TXT / SVG（打孔卡）/ PNG / SVG（模拟视图）

## 10. Web 端 PDF 导出方案

- 库方案现状（业界通行组合，本次未在同类前端 bundle 命中）：jsPDF + svg2pdf.js（SVG→PDF 矢量）、PDFKit、html2canvas+jsPDF（截图式）、**打印 CSS（window.print / print media）**
- 同类工具实际做法：
  - **Stitch Fiddle**：独立 `/print` 路由 + `beforeprint` 监听 + 导出项标注 "PDF document (use for printing)" → 浏览器打印管线强线索（服务端生成未能排除）
  - **Chart Minder**：仅 "download as image" + "print it"——图像导出 + 浏览器打印
  - **MakeBead**：宣传"可打印 PDF 图解"+"**在浏览器内转换——图片不会被上传**"（客户端生成；具体库未定位）
  - 桌面对照：CrochetCharts/sconcho = **QPrinter 直接产 PDF**、SVG 用 QSvgGenerator；Stitchmastery 自带 PDF/SVG/EPS；KnitBird（AIR）原生 PDF 导出

## 调研方法/来源线索

Wappalyzer/Prospeo/sitestatsdb/LinkedIn 公司页；站点源码与 JS bundle 直接抓取分析（Stitch Fiddle、HookChart）；iTunes Lookup API；Stitchmastery 官方手册 + Zendesk 知识库；Adobe 官方博客；GitHub 源码逐文件核对（CrochetCharts/sconcho/knotty/knitdb）；KnitScape CHI 2024 论文；TRAE 官方社区参赛帖；knittingcn.cn 公告。
