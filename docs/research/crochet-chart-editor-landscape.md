# 毛线图谱/钩针图解编辑器领域事实清单（供可行性分析）

> 调研时间：2026-02。仅罗列可核实事实与来源线索，不含结论。多数来源为一手（官网/官方文档/标准原文/论文），少数标注为二手。

## 1. 竞品 / 同类工具

- **Stitch Fiddle**（stitchfiddle.com，Web，SaaS）
  - 核心功能：网格逐针绘图（符号/颜色两种模式）+ 自由摆放；照片导入转图解（配色像素化）；进度跟踪（行计数/高亮当前行）；自动将图解转文字图解（Premium）；错误检查、图层、镜像/旋转/填充、多人协作、云端同步。支持钩针、棒针、十字绣、钻石画等多品类。来源：https://www.stitchfiddle.com/en/premium/pricing
  - 定价：免费版永久免费（15 张图解、最大 300×300、50 色/符号）；Premium $5/月，年付 $30（合 $2.50/月），一次性付费不自动续费；Premium：无限图解、1000×1000（100 万针）、250 色/符号、矢量下载（.svg/.svgz/.eps/.ps/.pdf/.wmf/.emf）、docx/odt、xlsx、图例单独导出、去水印。来源：同上；（Crochetly 称最大 2000×2000、$2.75–5.50/月，与官网数字有出入，以官网为准）https://crochetly.com/apps/stitch-fiddle/
  - 优缺点（用户/评测来源）：上手快、跨平台免安装、照片导入适合 graphgan/配色；缺点：矩形网格（不规则行需用 "no stitch" 灰格变通）、不适合打版/尺码放码、文字图解需付费。来源：https://www.reddit.com/r/knitting/comments/14zo6yz/ 、https://stitchrove.com/site/stitch-fiddle 、https://www.willowcrochet.com/stitch-fiddle-features-for-crochet-colorwork/
- **Stitchmastery**（stitchmastery.com，桌面：Windows 10/11、macOS、Linux/Ubuntu，Java）
  - 核心功能：棒针图解为主（平织/环织/two-pass 双色），海量针法库 + 自定义库（.smlib），自动行文字说明 + 术语表，图表合法性检查（行列加总）、重复区/边框、隐藏 no-stitch、德国/丹麦语模板；工程文件 .knt2。来源：https://stitchmastery.com/features/ 、https://stitchmastery.com/frequently-asked-questions/
  - 定价：一次性买断 £70/$95，激活 2 台机器、终身使用；demo 模式免费试用；2024-05-06 起从 £60/$90 涨价。来源：FAQ 及 https://stitchmastery.com/stitchmastery-version-3-2-2-is-now-available/
  - 导出：SVG/PDF/EPS（矢量）+ JPG/PNG；不直接打印，需导出后自行排版（FAQ 明示）。另有 Stitchmastery Knitting Font Collection（TrueType 针法字体，可用于 Word/Inkscape/InDesign 等）。来源：FAQ
  - 优缺点：专业出版级输出、符号与颜色自动对比；缺点：主打棒针（钩针支持弱于棒针）、桌面单机、无云协作。
- **KnitBird**（knitbird.com，桌面 Windows/Mac，Adobe AIR；冰岛开发）
  - 核心功能：棒针图解绘制、常用符号库、图片转图解、图案库（局部复用）、文字说明、JPEG/PDF 导出。来源：https://www.knitbird.com/
  - 现状/定价：已停止销售、免费下载，用 key `free.knitbird` 激活（约 2019 年起）；历史上售价约 $40/€59（净价 €59+税=€73.75）。来源：knitbird.com、https://www.knittingparadise.com/threads/drawing-software-knitbird-is-now-free.604547/ 、freedownloadmanager 页
  - 优缺点：CNET 评测：缺云同步、导出仅 PDF/JPEG、无明确撤销/图层（https://download.cnet.com/knitbird/3000-2130_4-75451401.html）；德语评测指出低对比度配色、撤销不可靠、帮助系统差（https://www.wockensolle.de/chartmaker-knitbird/）。版本停留在 1.3（2012）/2.0.x，不再维护。
- **knitCompanion**（iOS/Android/Mac，移动 App）——定位是"图解跟织/标记工具"而非编辑器
  - 核心功能：任意 PDF 图解的行/针标记、计数器、涂写/高亮/Magic Markers、提醒、图解拼接、**智能图解识别（Intelligent Chart Recognition，付费）**、语音控制、Ravelry/Dropbox 链接、离线可用；持有专利 US 8,506,303 & 8,529,263。来源：App Store/Google Play 页面、https://www.knitcompanion.com/wp-content/uploads/2025/07/Getting-Started.pdf
  - 定价：免费 Basics（无限项目）+ Setup+Essentials 年订 $29.99（美区 App Store，7 天试用）；订阅按商店分别购买（Apple/Google 不互通）。来源：https://illuminationdevelopment.com/costs/is-knitcompanion-free 、App Store
- **Stitch Works Crochet Charts**（桌面 Windows/Linux，Qt4；作者 Brian Milco）
  - 核心功能：钩针专用；自由摆放针法或按行/圈生成、环形图楔形辅助线、分组/图层、自定义针法库、图例自定义、一文档多图；**内置 109 个针法符号**；导出 PDF/SVG/JPEG/PNG/TIFF/BMP + 纯文本。来源：http://stitchworkssoftware.com/ 、https://www.edieeckman.com/2016/07/31/in-search-of-crochet-charting-software-part-2/
  - 现状/定价：曾售 $50；2015-09 开源（GPLv3），v1.2（2015-09-04）后停止开发，GitHub 仓库已 ARCHIVED（41 stars / 27 forks）。来源：https://github.com/StitchworksSoftware/CrochetCharts
  - 优缺点：唯一开源钩针专用编辑器、SVG 矢量针法；缺点：无 Mac 版、网格吸附/撤销 bug、官方文档缺失、镜像翻转分组不正常。来源：https://yarnandy.com/is-it-worth-using-crochet-charts-for-your-patterns/ 、sunsetcat 评测
- **Chart Minder**（chart-minder.com，Web，2013 年起，独立开发者 Abby）
  - 核心功能：棒针/钩针/十字绣配色图解绘制 + 照片扫描转图解、密度（gauge）计算器与真实尺寸、针目仿真预览、社区公开图解库 remix、进度跟踪；免费版可用核心功能。来源：https://www.chart-minder.com/ 、https://www.chart-minder.com/pro
  - 定价：免费 + Pro 订阅（季度/年度，可付完即取消）；Pro：私有图解、复制粘贴、翻转旋转、查找替换、高清下载、无限标注框、去水印等；免费版仅 95 色、1 个标注框、图解默认公开。来源：/pro 页、McGill Library 指南
  - 月访问量约 72.7K（Facebook 账号页数据，二手）。
- **Intwined Pattern Studio**（intwinedstudio.com，桌面 Windows/Mac）
  - 核心功能：棒针/蕾丝/钩针/编织图解；**内置 200+ 针法/绞花/符号**；绘图即生成文字说明、反向"打字生成图解"；内部用 **SVG** 渲染（"never look like pixels"）；输出 PDF 或图像。售价 $44，v1.5。来源：https://intwinedstudio.com/
- **StitchSketch / StitchSketchEx / KG-Chart**（Ikuta/Mozusystems，日本；iOS 及 Mac/Win/Android）
  - 核心功能：十字绣/串珠/棒针/像素图图解；**内置 JIS 编织符号（Amizu Kigou）**、照片转图解、PDF 生成（打印/邮件）、PNG 导出；最大 500×500（Ex）/250×250（旧版）。StitchSketchEx 移动版 $7.99。来源：https://www.iktsoft.net/kgchart-en/ 、App Store、https://www.mozusystems.com/products/stitchsketch/
- **中国区新工具（多为 2025–2026 新品）**
  - **编织助手**（iOS/iPadOS，独立开发者；图解制作+行针计数+毛线库存）：棒针像素图/提花图/棒针图解编辑（自定义符号、多格针法、区域选择/批量修改、模板、图例导出）；**钩针图编辑器**（符号/组合/路径，沿直线/圆/椭圆/多边形/螺旋排布，50+ 新钩针符号在 v1.9.8 加入）；PDF/图片编织笔记；**自动绘制：PDF 图解区域自动识别与吸附、棒针符号识别（带确认/修正/记忆）**（v1.9.6）；导出图片/PDF 附颜色图例+针法图例；iCloud 同步；订阅制：高级会员 ¥12、长期会员 ¥198 等。来源：App Store（apps.apple.com/cn/app/id6744092783）
  - **Yarnbie**（iOS，Serhat Gokce，2025）：AI 照片→分步图解（钩针/棒针/玩偶）、PDF 图案导入、行数计数器、针数计算器、1000+ 教学视频、社区流；免费+IAP；中文支持。口碑数据：12 个评分 2.5 分（早期）。来源：App Store（id6743322760）
  - **HookChart**（Web，hookchart.crochetkingdom.cn，钩针王国出品，2026-09 Free Beta）：钩针专用在线设计器；方格/环形网格、图层、对齐辅助线、**钢笔+节点矢量路径**、工程保存/本地下载再导入、PNG/JPG 导出；自述高清印刷级导出与 AI 推荐尚在开发。来源：https://www.knittingcn.cn/article/2026/09/16.html
  - **团团织**（微信小程序）：数行计数器、**文本图解转 3D 预览**、图片转像素图（钩织/拼豆双模式、匹配毛线色号）、可视化图解编辑器（方形/圆形/自由画布、内置符号库）、免费图解库+20 课教程。来源：https://forum.trae.cn/t/topic/146813（TRAE 大赛参赛帖）
  - **MakeBead**（Web，免费免注册）：照片→钩针图解毯（graphgan/C2C）与棒针提花图解；浏览器本地处理图片；**密度校正**（棒针针目宽>高，自动按比例加行）、真实毛线色号匹配、Floyd–Steinberg 抖动、每色针数统计、可打印 PDF（网格+符号+图例）。来源：https://makebead.com/zh-Hans/crochet-pattern-maker
- **ChartMind**：未检索到名为 "ChartMind" 的编织图解产品；检索结果均指向 **Chart Minder**（chart-minder.com），推测为同一工具的误记（来源线索：stitchrove.com/site/chart-minder 等）。
- 其它相关（工具/字库路线）：Excel + 针法字体（如 Stitchmastery 字库）是常见的低成本替代方案（俄语博主、McGill 指南均提及）；McGill Library 指南还列出 The Craftmat（照片→十字绣图解+PDF，浏览器本地处理）。来源：https://libraryguides.mcgill.ca/c.php?g=736925&p=5308741

## 2. 图谱符号标准

- **日本 JIS**：**JIS L 0201「編目記号」**（Letter symbols for knitting stitch），1955-05-21 制定、2015-10-20 最近确认，原案由日本编物检定协会制作。将符号分 4 类：(1) 家庭用编织机针目（编号 1010–1230，约 23 个）(2) 棒针针目（2010–2220，约 22 个）(3) 钩针（かぎ針）针目（3xxx 系列）(4) 阿富汗针针目。规定符号画在格子中央、原则上表示织物正面。来源：https://jis.eomec.com/jisl02011995（全文镜像，钩针部分在 PDF 第 6 页后未完整公开）
- **钩针符号国际共通画法**（基于 JIS/惯例）：椭圆=锁针、点=引拔针、X/＋=短针、带 1 横线的 T=长针（横线数量=绕线次数）；JIS 基础辞典（イトシラベ）收录钩针符号 31 项；日本 Vogue 教程列基本符号 8 类。来源：https://loopypanda.com/ja/symbols/ 、https://itoshirabe.com/guide/crochet-symbols 、https://www.tezukuritown.com/nv/c/ckihonkk4/
- **美国/欧美（CYC）**：Craft Yarn Council 发布 **Crochet Chart Symbols**（Standard Stitch Key 约 20 项含变体：ch、sl st、sc、hdc、dc、tr、dtr、shell、sc2tog/dc2tog、cluster/puff/bobble、popcorn、FPdc/BPdc、picot、BLO/FLO 等）与 **Knit Chart Symbols**（约 40 项 + 两套绞花符号，含 no-stitch、收针/起针符号）；符号免费下载使用（需注明 credit line）。来源：https://www.craftyarncouncil.com/standards 、https://media.craftyarncouncil.com/files/CYCACrochetChartSymbols.pdf
- **欧美无单一权威标准**：英文维基 "Knitting pattern" 条目明确"没有单一权威的编织符号与缩写出处"，多套标准并存（如 purl 符号横线 vs 小点）；因此每份图解必须自带 key/凡例。来源：jumble shop 博客引述（https://www.jumbleshop-one.com/apps/note/random-note/knit_pattern/）
- **美式/英式术语错位**：钩针针名美式比英式低一级：短针=sc(US)/dc(UK)、中长针=hdc/htr、长针=dc/tr、长长针=tr/dtr；**但符号图形本身国际共通**。来源：https://www.bianzhirensheng.com/a/21091.html 、loopypanda
- **中国**：未检索到专门的"手工编织符号"国家标准；相关国标 **GB/T 29867-2013《纺织品 针织物 结构表示方法》**（2014-05-01 实施）针对针织物结构表示，且声明"并非唯一表示方法"（来源：国家标准馆 https://www.ndls.org.cn/standard/detail/241bff0f9aaf17e1c90f57a4b657106f）。中文社区实用符号体系接近 JIS/国际惯例，并有中文单字母缩写：X=短针、T=中长针、F=长针、E=长长针、V=加针、A=减针、W=1针放3针、M=3针并1针、环起=环形起针（loopypanda 中日对照表）。编织人生论坛有"日、中、俄编织针法符号名称对照表"（棒针+钩针）。来源：https://www.bianzhirensheng.com/a/34534.html
- **棒针 vs 钩针体系差异（事实）**：JIS/CYC 均将两者作为独立符号集（JIS 2xxx vs 3xxx；CYC 分两页标准）；棒针图解需处理正/反面（RS/WS 左右交替读图）、no-stitch 格、绞花跨列符号；钩针图解常见环形/放射状排布、符号高度编码针高（T 形横线数）、起立针；专业工具普遍分设两个编辑器（如编织助手分"棒针图解/钩针图编辑"）。来源：各工具文档 + 上述标准
- **符号清单规模（各库实测数字）**：JIS 每类基础符号 20–25 个左右；CYC 钩针约 20、棒针约 40；Stitch Works 109 个钩针符号；Intwined 200+（棒针+编织+蕾丝+钩针合计）；编织助手一次更新即新增 50+ 钩针符号（总量未公布）；Sconcho/Chart Minder/编织助手/HookChart 均支持自定义符号。来源：同上各条

## 3. 技术实现路径（事实）

- **矢量方案**：
  - SVG 是事实上的共享底座：Stitchmastery 导出 SVG/PDF/EPS（"razor sharp"）；Intwined 内部即 SVG 渲染；Stitch Works "Scalable stitches (using SVG)"；Sconcho 自定义符号以 SVG 文件导入（用 Inkscape 制作）、可导出 SVG；HookChart 提供矢量路径+节点编辑；Stitch Fiddle Premium 导出 svg/eps/pdf/wmf/emf 等。
  - 针法字体路线：Stitchmastery Knitting Font Collection 为 TrueType 字体，可在 Word/LibreOffice/Inkscape/InDesign/Scribus 中当普通字体排图解（另一条低成本路线）。来源：https://stitchmastery.com/frequently-asked-questions/
  - Web 端 Canvas 亦有先例：knitscape（开源）用 Canvas + D3 force simulation 渲染并仿真；KnitViz（Python）用 Pillow + CairoSVG 处理 SVG 针法图。来源：https://github.com/knitscape/knitscape 、https://github.com/HiDefTextiles/KnitViz
- **开源图解数据模型/项目**（均为公开 GitHub 仓库）：
  - StitchworksSoftware/CrochetCharts（Qt4/C++，GPLv3，已归档）：钩针图解编辑器完整实现可参考，工程文件 .pattern。https://github.com/StitchworksSoftware/CrochetCharts
  - craftoid/sconcho：出版级棒针图解（Qt），工程格式 .spf，自定义符号=SVG+名称/类别/默认宽高/描述。https://github.com/craftoid/sconcho
  - fossasia/kniteditor：**FileFormatSpecification.rst** 提出 JSON 风格的编织图解文件格式设计（明确"不能以网格为基础"、指令可任意扩展、支持反面行等建模决策）。https://github.com/fossasia/kniteditor/blob/master/docs/FileFormatSpecification.rst
  - t0mpr1c3/knotty：编织图解 DSL（Typed Racket），导入/导出 Knitspeak，输出含交互图解+文字说明的 HTML。https://github.com/t0mpr1c3/knotty
  - HiDefTextiles/KnitViz（冰岛大学研究项目）：JSON 编织图解 → SVG 针法图/针法地图（stitch map）可视化。https://github.com/HiDefTextiles/KnitViz
  - imtambovtcev/knitvis（Python，MIT）：图解生成/织物仿真/进度跟踪，JSON 导入导出，PyQt GUI。https://github.com/imtambovtcev/knitvis
  - knitscape：图解 repeat 编辑 + 织物仿真，导出 JSON/PNG/SilverKnit TXT。https://github.com/knitscape/knitscape
  - CMU Textiles Lab：**knitdb** 数据集（图解为 "JSON-based stitch graph"，见 KnitPick 论文，UIST 2019；图案取自书籍与 Stitch Maps 网站）；**knitout (.k)** 文件格式规范 v0.6——机器无关的低层针织指令文本格式（含 knit/tuck/split/xfer 等指令集）。https://github.com/textiles-lab/knitdb 、https://textiles-lab.github.io/knitout/knitout.html
- **图解/图像识别现状**：
  - 照片→配色像素图解（成熟、普及）：Stitch Fiddle、Chart Minder、MakeBead、StitchSketch、编织助手、团团织 均提供；本质是缩放+量化+色号匹配，不是符号识别。
  - 图解图片/PDF→符号（商用雏形）：knitCompanion 的 "Intelligent Chart Recognition"（付费，用于 PDF 图解建档/标记）；编织助手 v1.9.6 提供"PDF 图解区域自动识别与吸附、棒针符号识别（确认/修正/记忆）"。来源：App Store 更新日志、knitCompanion 介绍
  - 织物照片→针织指令（学术界，未见消费级产品）：
    - CVPR 2019《Inverse Procedural Modeling of Knitwear》：单张照片推断针法与编织指令，**需用户在图上标注针法样本**，作者明确指出遮挡/毛线/手劲差异使识别困难、专家也难以仅凭照片判断。https://openaccess.thecvf.com/content_CVPR_2019/papers/Trunz_Inverse_Procedural_Modeling_of_Knitwear_CVPR_2019_paper.pdf
    - MIT CSAIL InverseKnit（2019）：深度网络从照片生成机器针织指令，自报在其数据集上指令准确率 94%。https://news.mit.edu/2019/computer-aided-knitting-mit-csail-0806
    - KnitStructNet（IEEE Access 2026）：真实织物 13 类针法分割，像素准确率 95.65%、mIoU 0.7829、macro F1 80.2%；自述"完整指令码生成超出范围"。DOI 10.1109/access.2026.3668326
    - MDPI Electronics 2025《Knitting Robots》、ICARCE 2024 等：两阶段深度学习流水线（正面标签生成+完整标签推断），仍在论文阶段。
    - 消费级 AI：Yarnbie 宣称"AI 把任意照片变成完整分步图解"（含玩偶/amigurumi），但无第三方精度评测，商店评分 2.5（12 个评分）。

## 4. 导出格式与打印排版要求

- **常见导出格式（各工具实测）**：
  - 光栅：PNG、JPG（几乎所有工具）；TIFF/BMP（Stitch Works）。
  - 矢量/印刷：**PDF、SVG** 为主流；EPS（Stitchmastery/Stitch Fiddle）；PS/WMF/EMF（Stitch Fiddle）。
  - 文档/数据：Word/LibreOffice（.docx/.odt）、Excel（.xlsx）——Stitch Fiddle Premium；纯文本图解（Stitch Works）；图例(key)单独导出文件（Stitch Fiddle）。
  - 工程文件互不通用：.knt2/.smlib（Stitchmastery）、.pattern（Stitch Works）、.spf（Sconcho）、.sth/.sthmb（KG-Chart/StitchSketch）、JSON（knitvis/knitscape/knitdb）。
  - 来源：各工具官网/FAQ（见第 1 节）
- **打印/出版排版要求（CYC 行业指南与技术编辑实践）**：
  - CYC《Standards & Guidelines》投稿要求：图解须用行业标准符号制作；**在图解上标注重复区（pattern repeats）、给每行编号**；含加减针的行/圈后给出针数；配套结构图（schematic）含各部位尺寸；使用标准缩写（CYC 缩写总表）；标注难度等级符号；RS/WS 标识。来源：https://www.craftyarncouncil.com/industry.html 、CYC YarnStandards PDF
  - 图解不能替代文字说明：应"图表 + 逐行文字说明"并存（Crochetly 风格指南）；图解读法惯例：自下而上，平织正面行从右往左、反面行从左往右，环织每圈从右往左；行号标在起始一侧；key 必须列出全部符号。来源：https://crochetly.com/style-guide/charts-and-graphs/
  - 技术编辑检查清单（FrasierKnits）：图解与文字一致、key 完整、平织/环织标注正确、重复可用、结构图尺寸准确。来源：https://www.frasierknits.net/post/free-pre-edit-checklist
  - 打印实现现状：Stitchmastery 不直接打印（导出图像后自行排版）；印刷级高清导出对小工具仍是难点（HookChart 自述高清导出未完成）；MakeBead/编织助手等输出"可打印 PDF（网格+符号+图例+针数统计）"。来源：同第 1 节
  - 配色图解设计约束（McGill 指南）：每行最好 ≤2 色、渡线（float）≤3 针为宜、图案宜简洁高对比。https://libraryguides.mcgill.ca/c.php?g=736925&p=5308741

## 5. 目标用户群体（社区规模/活跃平台）

- **Ravelry**（全球最大编织社区，Web）：2007 年上线；约 850 万注册（2020-03 报道）、近 900 万注册 + 约 100 万月活（2020-03，维基引述）；用户约 96% 为女性（基于 40 万+ 用户资料的研究）；2022 年社区数据：图解库累计近 120 万份、当年新增 87,106 份；年产 130 万件作品（棒针 110 万/钩针 22.5 万）；棒针用线 3.288 亿码。来源：https://www.technologyreview.com/2020/03/06/905472/ 、https://en.wikipedia.org/wiki/Ravelry 、https://blog.ravelry.com/2022-community-stats/ 、https://www.weforum.org/agenda/2018/11/what-this-giant-study-of-knitters-tells-us-about-entrepreneurship/
- **美国市场（CYC 调研）**：约 3,800 万消费者享受棒针/钩针；2004 年调研：36% 美国女性（5,300 万）会棒针或钩针；2014 年跟踪调研 3,178 人中 48% 以钩针为主、32% 棒针为主、20% 两者并重；上网用途：找图解 90%、找灵感 67%；使用的平台：Ravelry 71%、Facebook 61%、YouTube 52%、Pinterest 40%。来源：https://www.craftyarncouncil.com/know.html
- **Reddit**：r/knitting 444,998 人（2023-02，二手汇总）；r/casualknitting 5.5 万、r/KnitHacker 3.7 万等。来源：https://knitlikegranny.com/knitting-statistics/
- **中国**：
  - **编织人生论坛**（bbs.bianzhirensheng.com，Discuz，苏州编织人生网络科技）：论坛页脚显示**会员 2,537,927、帖子 32,628,278**（本次调研实时读数）；App Store 简介称"200 万注册会员"；板块帖量：棒针编织作品秀 14 万主题/935 万帖、钩针编织作品秀 14 万主题/590 万帖、钩针教程区 940 主题/18 万帖；旗下微信公众号、App（131 个评分）。
  - **小红书**：#钩针编织 话题 **32.4 亿次浏览、983 万条讨论**（2026-01 报道）；另一报道 #手工钩织 浏览量超 39 亿、#钩针编织 36.6 亿（2026-10）；《小红书 2025 年度兴趣报告》十大出圈兴趣含钩织；2026-01 小红书在上海办线下"织女的聚会"。来源：http://4g.tynews.com.cn/system/2026/01/29/030980027.shtml 、https://tidenews.com.cn/news.html?id=3598790
  - **抖音**：#手工编织 话题播放量超 270 亿、#钩针编织 超 191 亿（2026-10 报道，来源同上）。
  - **淘宝**（市场热度）：多家编织材料店铺近 30 天购买人数环比增速超 50%（2026-01，太原晚报）。
  - 微博超话：本次检索未获得可靠规模数据（待补）。
- **其它**：英国手工针织协会（UK Hand Knitting Association）称英国编织人口超 600 万（二手）；全球编织相关产业 2018 年估值 238 亿美元（二手，knitlikegranny）；#knitting 在 Instagram 2,300 万+ 帖（2020，二手）。

## 信息缺口（未核实项）

- "ChartMind" 产品本身（仅能确认与 Chart Minder 的可能混淆）。
- JIS L 0201 钩针（3xxx）符号完整清单与精确总数（公开镜像 PDF 第 6 页起未转载，需查 JIS 原文）。
- 微博超话、B 站、豆瓣等中文社区的精确规模数据。
- Yarnbie 等 AI 图解生成的输出质量第三方评测。
