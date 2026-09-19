# AGENTS.md — 站点内容维护规范

本仓库是 **Shaoqing Dai（戴劭勍）的 Hugo Academic 个人学术主页**源文件。AI 助手在本仓库的主要任务是：**接收用户提供的内容，按本规范生成或修改对应的内容文件**。所有新文件必须与现有条目格式保持一致。

## 站点概况

- Hugo v0.59.1 extended + Academic 主题（git submodule：`themes/academic`，**不要改动主题内文件**）。
- 站点配置在 `config/_default/`（config.toml / languages.toml / menus.toml / params.toml）。
- 页面内容为英文（`hasCJKLanguage = false`）；`cite.bib` 中中文文献的条目保留中文（已有先例）。
- 构建验证：`hugo --gc --minify`；本地预览：`./view.sh`。
- **部署**：`hugo --theme=academic --baseUrl="https://gisersqdai.top/mycv/"` 生成 `public/`；`public/` 是独立 git 仓库，推送到 GitHub Pages（`GISerDaiShaoqing/mycv`）后站点上线。源码仓库推 Gitee。README 与 AGENTS.md 需复制一份进 `public/`（随发布仓库上 GitHub），文档更新后记得同步。
- 站点部署在 `/mycv/` 子路径下：news 等页面中指向站内的相对链接沿用现有写法（如 `../mycv/publication/<slug>`）。
- Git 提交信息惯例：`Update YYYYMMDD`（如 `Update 20260128`）。**不要主动 commit/push**，除非用户明确要求。

## 内容类型速查

| 要更新什么 | 文件位置 |
|---|---|
| 论文 | `content/publication/<slug>/index.md` + `cite.bib` |
| 学术报告 | `content/talk/<slug>/index.md`（可含 `featured.jpg`） |
| News | **同时改** `content/news.md`（全量页）和 `content/home/news.md`（首页摘要） |
| 首页列表（Services / Awards / Teaching / Resources） | `content/home/services.md`、`awards.md` 等 blank widget 的正文 |
| 研究项目 | `content/project/<slug>/index.md` + `featured.jpg` |
| 个人档案 | `content/authors/admin/_index.md` |
| CV | `static/cv.html` + `static/cv.pdf`；中文版 `static/template-zh.html` + `static/template-zh.pdf` |
| 首页栏目开关/排序 | `content/home/*.md` 的 `active` / `weight`；导航在 `config/_default/menus.toml` |

---

## 1. 论文（publication）

**目录命名**：小写 kebab-case，惯例为 `期刊缩写-主题词`（如 `japg-cgwnn-population`、`ecoi-landscape-flooding`），不得与现有目录重名。

`index.md` 模板（YAML front matter，正文留空）：

```yaml
---
title: "论文英文标题"
authors: ["First Author", "**Shaoqing Dai**", "Last Author"]
date: "2025-09-02T00:00:00Z"
doi: "10.xxxx/xxxxx"

# Schedule page publish date (NOT publication's date).
publishDate: "2025-09-02T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["2"]

# Publication name and optional abbreviated publication name.
publication: In *Journal Full Name*
publication_short: ""

abstract: |
  论文摘要原文……

summary: ""
tags: []

featured: false

url_pdf: "202601.pdf"
url_code: ""
url_dataset: ""
url_poster: ""
url_project: ""
url_slides: ""
url_source: ""
url_video: ""

links: []

image:
  caption: ""
  focal_point: ""
  preview_only: true

projects: []
slides: ""
---
```

规则：

- `authors`：每位作者一个字符串，**不要把逗号写进字符串**（现有个别条目有 `", Feng Lu"` 这类笔误，不要模仿）；自己的名字加粗为 `"**Shaoqing Dai**"`，放在实际作者顺位。
- `date` 用论文的**在线发表日期**（online publication date，见 DOI 页或 PDF 页脚的 Published/Available online；卷期月份晚于在线日期时仍以在线日期为准，卷期信息写进 `publication` 和 `cite.bib`）；`publishDate` 直接设为与 `date` 相同（现有部分旧条目此字段是错的，不要照抄）。
- **通讯作者标注**：自己是（共同）通讯作者的论文，在 `authors` 中把自己的名字写为 `"**Shaoqing Dai(corresponding)**"`（沿用现有条目写法，如 HabitatI-SVI-3DHPM）；不确定时查 PDF 首页脚注的 Corresponding author 信息。
- **微信推送**：论文有微信公众号推送时，在 `links:` 块加 `- name: "Wechat article"` + 推送链接（沿用 NC-MetS-score-calculator 等现有写法）。
- **代码链接**：论文有公开代码仓库（GitHub/R 包）时填入 `url_code`（如 tEDM → `https://github.com/stscl/tEDM`）。
- `publication_types`：期刊论文 `["2"]`，按上面图例选择。
- `featured` 默认 `false`；**是否 featured 由用户指定**（首页 Featured Publications widget 取最近的 2 篇 featured 条目，Card 视图展示）。
- `url_pdf`：论文 PDF 放 `static/` 根目录，**按"年份 + 当年时间顺序号"命名**——顺序号按**在线发表时间**先后排列（先在线者序号小），论文与报告共用同一序列，每年从 01 重新计数（如 2026 年第 1 份材料为 `202601.pdf`，其幻灯片为 `202601slides.pdf`；现有文件已排到 `202508.pdf`）。字段内只填文件名；没有 PDF 则留空并列入待补清单。
- `summary`：填一句话概括论文（列表卡片显示用），不要留空。
- **网页图编号**：论文页不沿用论文原图号——**featured 论文按图（表）在网页中出现的顺序重新编号**（Figure 1、2、3…，Table 1、2…），**图与表合计不超过 6 个**；**非 featured 论文通常只有一张图，统一编号 Figure 1**（多图拼接也计一张）。图名仍用论文原图的完整图名。
- `image.preview_only: true` 保持不变。

`cite.bib`：英文论文用英文条目（key 惯例：第一作者姓 + 年份）；中文论文条目可用中文（已有先例，如 `林玥希2025`）。

### 每篇论文的页面内容要求

- `abstract` 不得为空：优先逐字采用论文摘要原文；拿不到原文时，写一段忠实概括论文主要内容的短文（约 100–150 词），并在汇报中注明是整理稿。
- **关键图**：每篇论文至少一张，从论文 PDF 提取（本机有 `pdfimages` / `pdftoppm` / ImageMagick，可直接导出），存为该目录的 `featured.jpg`；在正文嵌入 `![](featured.jpg)`，其下紧跟粗体图名，格式 `**Figure N. 完整图名.**`（沿用现有条目写法）。
- 新论文暂无 PDF 时，把"关键图 + PDF 挂链"列入待补清单，等用户提供 PDF 后一次补齐。

### Featured 论文重点展示框架

以现有 11 篇 `featured: true` 条目（如 TREE-COVID-19-2、JAG-Bikeability-multisource）为基准，featured 论文需材料齐全：

1. front matter：`featured: true`，且 `abstract`、`tags`、`summary`、`url_pdf` 完整。
2. 目录含 `featured.jpg`（论文关键图）。
3. 正文按论文自身结构写 3–5 个 `#### 小标题` 小节，每节一段话概括该部分要点（用自己的话概括，不逐字照抄）。
4. 正文嵌入关键图：`![](featured.jpg)` + `**Figure N. 完整图名.**`。
5. 用户说"重点展示这篇"时，按上述清单补齐全部材料后再设 `featured: true`。

**信息缺失时**：优先通过 DOI（Crossref）或期刊页面联网补全 title / authors / abstract / date；仍缺的字段列成清单问用户，不要编造。

## 2. 学术报告（talk）

**目录命名**：`会议缩写+年份+talks`（如 `AGSCF2025talks`、`HGYF2024talks`）或描述性 kebab-case。

`index.md` 模板：

```yaml
---
title: 报告标题
event: 会议/活动全名
event_url: ""

location: 地点名称
address:
  street: ""
  city: Wuhan
  postcode: ""
  country: China

summary: 一句话说明
abstract: ""

date: "2025-05-24T16:20:00Z"
date_end: "2025-05-24T16:40:00Z"
all_day: false

publishDate: ""

authors: []
tags: []
featured: false

image:
  caption: ""
  focal_point: Right

# 个人微博 Follow 链接：所有 talk 条目统一保留（见下方规则）。
links:
- icon: weibo
  icon_pack: fab
  name: Follow
  url: https://weibo.com/2884386252/profile?rightmod=1&wvr=6&mod=personinfo&ssl_rnd=1509588674.6099&sudaref=localhost&display=0&retcode=6102
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

slides: ""
projects: []
math: true
---
```

规则：

- `date` / `date_end` 为报告起止时间（带时区 Z）；全天活动用 `all_day: true`。
- 幻灯片 PDF 放 `static/` 根目录，命名与论文共用"年份 + 顺序号"序列并加 `slides` 后缀（如 `202410slides.pdf`），填入 `url_slides`。
- 可选放 `featured.jpg`；`projects:` 可关联现有项目 slug：`spatial-lifecourse-health`、`urban-carbon-cycle`、`air-pollution-map`、`spatial-temporal-gis-theory`、`virtual-geographical-environment`、`urban-blue-and-green-space`。
- `links` 中的微博 Follow 链接是**站主个人微博**，所有 talk 条目统一保留（直接用模板中的链接块）。

## 3. News（全量页默认必更新；首页由站主挑选）

新增**论文类** news 时（收录论文的固定动作）：

1. **只更新完整版 `content/news.md`**：在列表**最顶部**按时间倒序插入，日期用论文的**在线发表日期**。
2. **首页 `content/home/news.md` 默认不动**：站主会自行挑选上首页的条目并补充其他 news。只有当用户明确指定哪些条目上首页时，才同步到 `content/home/news.md`，并将该文件只保留用户选定的最近约 6 条（多余的从首页删除，全量页保留）。

**首页 news 的选择依据（站主已确认）**：

- **时间窗**：一年以内；条目数量保持 4–6 条（widget 即 "Recent Key News" 摘要，More News 按钮导流全量页）。
- **常规论文发表不上首页**：首页 Publications 区已滚动展示最新论文，避免重复；**论文的事件性节点除外**（如入选教材、ESI 高被引等荣誉/影响事件）。
- **优先级**：① 个人里程碑（入职/答辩/毕业/出书/人才计划入选）> ② 荣誉获奖（专利授权、竞赛奖、ESI 高被引、编委聘任）> ③ 代表性活动（邀请报告、重要年会参会、授课）；日常参会与常规服务任职（客座编辑等）默认不上，用户点名除外。
- **同步原则**：从全量页原文照搬（含链接），只做删减不做改写。

**条目中文标准（模仿现有句式）**：

```markdown
-   **Aug. 14, 2026:**
    We published a paper entitled '论文英文标题'. [This paper](../mycv/publication/<slug>) is now available on [期刊名](https://doi.org/<doi>).
```

- **常规论文句式**：`We published a paper entitled '<title>'. [This paper](../mycv/publication/<slug>) is now available on [<Journal>](<DOI 链接>).`
- **社论句式**：`Our editorial entitled '<title>' has been published in [<Journal>](<DOI 链接>). [This editorial](../mycv/publication/<slug>) introduces ...`
- **可选补充句**（接在首句后）：代码/数据开放（如 `The open-source [xx R package](url) is available on CRAN.`）、入选、获奖、被报道等。
- **研究概述句**：论文类 news 可在首句后加 `In this study, we ...` 一句概述方法与结果；**featured/重点论文按现有 HabitatI-SVI-3DHPM、ISPRS-street-view-generation 条目风格详写主要发现**，普通论文一句带过。
- **微信推送**：论文有微信推送时，在条目末尾加 `The related [Wechat article](url) introduces this study.`
- **措辞惯例**：项目/基金主持人写 `principal investigator`（不用 project leader）；期刊/实验室等机构名以官方英文名为准。
- 日期：英文三字母月份缩写 + 两位日 + 年（`Aug. 14, 2026`；五月写作 `May. 21`）。
- 站内链接用 `../mycv/publication/<slug>`（slug 一律小写；线上部署在 `https://gisersqdai.top/mycv/`，news 页相对解析单层 `../` 即为 `/mycv/`，已线上验证；条目间空行；正文换行后缩进 4 个空格）。
- 其他类型 news（参会、报告、获奖、服务等）仍由站主提供内容或按现有条目风格撰写。

## 4. 首页列表 widget（Services / Awards / Teaching / Resources）

这些是 blank widget（`widget = "blank"`），**front matter 不要动，只改正文**。条目用 Markdown 表格、按年份倒序插到对应分组顶部。

Awards 表格写法（见 `content/home/awards.md`）：

```markdown
## Award & Honors

Year|Award & Honors
----|-------------
2025|**奖项名称**，颁发机构，[机构链接](URL)。
```

Services 按 `## 分组标题`（Organizational Roles / Science & Technology Service 等）+ 表格组织，分组行用 `| **Editors** | |` 这类形式（见 `content/home/services.md`）。

## 5. CV（**不在本仓库维护，不要改动**）

`static/cv.html`、`static/template-zh.html`、`static/cv.pdf`、`static/template-zh.pdf` 是从**另一个 CV 项目**直接拷贝过来的成品，其源文件与维护都在那个项目里进行，站主会在那边改动后拷回。**AI 助手不得修改这四个文件**（包括拼写修正）；即使发现其中的笔误（如 cv.html 软著条目的历史拼写），也只向站主报告，不代改。

## 6. 研究项目（project）

项目围绕**已发表的论文及相关研究内容**组织拓展（现有 6 个项目：`urban-carbon-cycle`、`air-pollution-map`、`spatial-lifecourse-health`、`spatial-temporal-gis-theory`、`virtual-geographical-environment`、`urban-blue-and-green-space`）。

- **论文与项目的关联由用户确认分类**：新增论文时可以建议归属哪个项目（按主题判断），但 `projects:` 字段和项目页新增小节都要经用户确认后再合入，不要擅自关联。
- **项目页写作模式**（照 `content/project/urban-carbon-cycle/index.md` 等现有条目）：
  - front matter：`summary` 一句话概述；`tags` 从词表选、与关联论文呼应；`image.focal_point: Smart`；配 `featured.jpg`。
  - 正文：开头一段研究背景与问题 → 按子课题分节（`# 1 子课题名`、`# 2 ...`），每节围绕若干已发表论文展开：一段方法/结果说明 + 论文关键图（`**Figure N. 图名**`）+ 相关微信推送或详情页链接。
- 用户说"完善某个项目"时，按上述模式起草新增小节（从关联论文提取关键图、概括内容），经确认后合入。

## 7. 其他

- `content/authors/admin/_index.md`：个人档案（role、organizations、education、social、bio）。用户说"更新简介/经历/头像"时改这里。
- 首页各栏目的显示与顺序由 `content/home/*.md` 的 `active` / `weight` 控制。

## 8. Markdown 渲染陷阱（Blackfriday，写入前规避）

- **不用 LaTeX 行内公式**：正文/摘要里的 `$CO_2$`、`PM$_{2.5}$`、`$R^2$` 等不会被渲染（页面默认无 MathJax），且成对的 `_` 会触发斜体连体。一律写 Unicode 纯文本：CO₂、PM2.5、R²、km²、10⁶、L·S⁻¹·ha⁻¹、g⁻¹、AF(+)。
- **避免"数字/数字"**：`0.427/0.537` 会被 smartypants 转成上下分数，改写为 "0.427 and 0.537"（DOI、日期、路径中的斜杠不受影响）。
- **YAML 值含冒号必须加引号**：`abstract: "..."`、`title: "..."`，否则构建报 "mapping values are not allowed"。
- **publishDate 不能晚于当前时间**：未来日期的页面 Hugo 直接不渲染；卷期晚于在线日期时 date/publishDate 都用在线日期。

## 9. 图片处理惯例

- **工具链**：`pdfimages -all`（从论文 PDF 提取位图）、`pdftoppm`（渲染页面后裁剪，适合矢量图）、ImageMagick（`+append` 横拼、`-append` 竖拼、`montage -tile 2x2` 网格；输出统一 `-strip -quality 85~88`，宽约 1400–1600px）。透明通道（smask）需 `-background white -alpha remove -alpha off` 压平。
- **项目封面 featured.jpg**：同时是项目头图与首页卡片图，可用已有论文图/素材图拼合制作（裁掉文字框与水印、统一宽度后拼接）；正文首图与封面解耦（正文引用独立文件名），换封面不影响正文。
- **外链图本地化**：项目页/论文页的图尽量存入本目录、语义化命名（如 `bikeability-daily.jpg`）。MDPI 等出版商图片有反爬（403，换 Referer/UA 无效），改从本地论文 PDF 提取。跨 bundle 复用图片必须复制文件（Hugo page bundle 不共享）。
- **Talks 照片**：照片放对应 talk 目录，`featured.jpg` 作列表卡片图，正文末加 `#### Photos` 节：`![](xxx.jpg)` + 一行英文图名（活动、地点、时间）。
- **Projects 过滤按钮**：首页 Projects 区的过滤按钮在 `content/home/projects.md` 硬编码（`[[content.filter_button]]` 的 name + tag）。新建项目需手动补按钮，tag 必须与项目页 tags 中的词完全一致。

---

## 现有 tags 词表（新增内容优先复用，不随意造新词）

GeoAI, Health Geography, Urban Visual Intelligence, Spatial Lifecourse Health, Spatial Statistics, Spatial-Temporal Big Data, Urban Carbon Cycle, Air Pollution Map, Street View Images, Obesity, Landscape Ecology, EO & RS, Ecological Modeling, SDG

## 每次收尾的固定动作

1. **拼写自查**：英文标题/摘要逐词检查（现有条目曾有 "Agreicultural"、"oritical"、"solftware" 这类笔误，不要新增同类问题）。
2. **校对**：DOI 可访问、作者顺序与原文一致、日期正确、slug 不与现有目录重复。
3. **论文条目三件套**：`abstract` 摘要段、关键图（`featured.jpg` + `**Figure N.**` 图名）、`summary` 一句话是否齐全；featured 论文再按"重点展示框架"逐项核查。
4. **构建**：`hugo --gc --minify` 必须无 ERROR 通过。
5. **汇报**：向用户列出新增/修改了哪些文件、哪些字段待补、PDF 是否已同步。
