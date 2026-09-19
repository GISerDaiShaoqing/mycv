# Shaoqing Dai（戴劭勍）个人学术主页

基于 **Hugo + Academic 主题**的个人学术主页源文件，线上地址：[https://gisersqdai.top/mycv/](https://gisersqdai.top/mycv/)。

## 技术栈

- Hugo v0.59.1 extended（勿升级，主题模板依赖此版本语法）
- Academic 主题（git submodule：`themes/academic`，**不要改动主题内文件**）
- 部署：构建产物在 `public/`，站点部署于 `/mycv/` 子路径

## 常用命令

```bash
hugo --gc --minify   # 构建验证（必须无 ERROR）
./view.sh            # 本地预览（hugo server）
```

## 双仓库架构（核心）

本站点由**两个 git 仓库**配合维护：

| 仓库 | 托管 | 内容 |
|---|---|---|
| **源码仓库**（本仓库，Gitee：`hugocv-source-document`） | Gitee | 整个工程的源码：content、配置、主题子模块、static 等 |
| **发布仓库**（GitHub：[GISerDaiShaoqing/mycv](https://github.com/GISerDaiShaoqing/mycv)） | GitHub Pages | `public/` 生成的**纯 HTML 静态站点**（`public/` 是独立 git 仓库） |

## 部署（关键命令）

第 1 步，生成 public 文件夹（baseUrl 指向线上子路径）：

```bash
hugo --theme=academic --baseUrl="https://gisersqdai.top/mycv/"
```

第 2 步，push 发布仓库到 GitHub（**日常更新**，在 `public/` 内）：

```bash
cd public
git add -A
git commit -m "message"
git push origin master
```

首次使用需初始化发布仓库：

```bash
cd public
git init
git remote add origin https://github.com/GISerDaiShaoqing/mycv.git
git add -A
git commit -m "message"
git push -u origin master
```

## 源码同步（Gitee）

源码改动同步到 Gitee 源码仓库：

```bash
git add -A
git commit -m "Update YYYYMMDD"
git push gitee master
```

## 目录结构

| 位置 | 内容 |
|---|---|
| `content/publication/` | 论文（每篇一个目录：`index.md` + `cite.bib` + `featured.jpg`） |
| `content/talk/` | 学术报告（可含照片与幻灯片挂链） |
| `content/project/` | 研究项目（围绕已发表论文组织） |
| `content/news.md` | News 全量页 |
| `content/home/` | 首页各 widget 区块（news 摘要、services、awards、projects 过滤按钮等） |
| `content/authors/admin/` | 个人档案 |
| `static/` | 论文/报告 PDF（按"年份+当年顺序号"命名，如 `202601.pdf`）与 CV |
| `config/_default/` | 站点配置（config / languages / menus / params） |

## 内容维护规范（重要）

**所有内容维护规则见 [AGENTS.md](AGENTS.md)**——它是本仓库的权威维护规范，AI 助手（ZCode 等）会自动读取并遵循。要点速览：

- **论文**：目录命名（期刊缩写-主题词）、`date` 用在线发表日期、通讯作者 `(corresponding)` 标注、微信推送与代码链接写法、关键图与 `summary` 必填、featured 论文重点展示框架
- **News**：全量页默认必更新（收录论文的固定动作）；首页 news 由站主挑选（时间窗一年、4–6 条、优先级：个人里程碑 > 荣誉获奖 > 代表性活动）
- **Talks**：目录命名、幻灯片 PDF 命名、个人微博 Follow 链接统一保留、照片以 page bundle 方式组织
- **Projects**：围绕已发表论文组织，写作模板见 `content/project/urban-visual-intelligence/`；论文与项目的关联由站主确认
- **Services / Awards**：blank widget 只改正文；期刊审稿按主题分组、组内按影响力排序
- **CV**：`static/cv.html`、`cv.pdf`、`template-zh.html`、`template-zh.pdf` 从另一个 CV 项目拷贝而来，**本仓库不维护、不改动**
- **渲染陷阱**：不用 LaTeX 行内公式（用 CO₂、PM2.5、R² 等 Unicode 写法）、避免"数字/数字"、YAML 值含冒号加引号、publishDate 不能是未来日期
- **图片处理**：外链图尽量本地化、语义化命名；出版商反爬图片从本地论文 PDF 提取；拼接用 ImageMagick

## License

主题部分遵循 [MIT](LICENSE.md)（Academic Kickstart）。

![](https://github.com/GISerDaiShaoqing/Curriculum-Vitae-Latex/blob/master/cn/qrcodeCV.png)

