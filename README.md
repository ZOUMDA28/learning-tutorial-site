# Learning Tutorial Site

基于 Jupyter Notebook 的交互式学习教程网站生成器。

> 参考 [modern-llm-notebook](https://github.com/walkinglabs/modern-llm-notebook) 项目模板构建的 TRAE Skill | [English](./README.en.md)

## 语言策略

- **模式一**：跟随用户 Notebook 语言
- **模式二**：**默认中文**生成所有 Notebook 和研读笔记。如需英文版本，用户须显式告知，届时生成 `notebooks-en/` 和 `NOTES.en.md` 英文镜像
- **README**：项目同时提供 [中文版](./README.md) 和 [英文版](./README.en.md)

## 两种工作模式

### 模式一：自有 Notebook 教程
用户提供 .ipynb 笔记本，生成可浏览的网页教程。可选对照斯坦福等国外大学同类课程进一步改写优化。

### 模式二：大学课程学术教程
用户给定研究方向和学习内容，自动基于大学公开课程（优先斯坦福，其次 MIT/CMU/Berkeley）生成学术级教程网站。**默认中文**，英文需用户要求。

**核心要求：**
- 学术严谨：每个知识点必须有论文出处，引用最新 arXiv 论文
- 代码作业：每篇 Notebook 必须有可运行的代码实现和作业练习（含 assert 验证）
- 图片可视化：每个核心概念配 matplotlib 图表或论文配图
- 论文研读：papers/ 目录记录每节课的论文研读笔记
- 零依赖实现：核心算法从零实现，不使用封装库
- 多课程交叉对照：以斯坦福为主线，对照 MIT/CMU/Berkeley 同类课程改写优化
- **质量审计**：每篇 Notebook 遵循四步教学路径（直觉→手算→实现→实验），含质量清单审计
- **批量生成**：按模块分批生成，使用 Python 脚本程序化创建 .ipynb 文件
- **预渲染输出**：本地执行 Notebook 并嵌入输出，前端无需内核即可显示结果

**参考标杆：**
- [self-improving-agent-notebook](https://github.com/walkinglabs/self-improving-agent-notebook) - Stanford CS329A 课程模式
- [hands-on-world-models](https://github.com/walkinglabs/hands-on-world-models) - 学术引用规范
- [hands-on-modern-rl](https://github.com/walkinglabs/hands-on-modern-rl) - 章节结构与可视化

## 功能特性

- **直接渲染 .ipynb 文件** - 无需后端，前端直接解析 Notebook
- **嵌套目录支持** - Notebook 可放在任意深度的子目录中
- **图片路径自动重写** - Markdown 中的相对图片路径自动指向正确位置
- **侧边栏目录导航** - 按学习路径组织内容，章节号正序排列
- **右侧大纲导航** - 快速跳转到章节
- **代码语法高亮** - Python 代码智能高亮
- **Markdown 完整渲染** - 标题、列表、表格、引用、代码块
- **数学公式支持** - KaTeX 渲染 LaTeX 公式
- **浅色/深色主题** - 主题模式切换，支持自定义颜色
- **字号调整** - 小/中/大三档字号
- **笔记和书签** - 本地存储学习笔记
- **代码折叠展开** - 长代码默认折叠
- **输出折叠展开** - 长输出默认折叠
- **图片灯箱** - 点击放大查看
- **URL hash 路由** - 支持分享链接
- **响应式设计** - 移动端完美适配
- **预取缓存** - 智能预取提升切换速度
- **GitHub Actions 自动部署** - 推送即部署到 GitHub Pages（双部署策略）
- **质量审计清单** - 每篇 Notebook 对照 8 项质量标准审计
- **批量生成脚本** - Python 脚本程序化创建 .ipynb 文件
- **预渲染输出** - 执行 Notebook 嵌入输出，无需本地运行环境
- **walkinglabs 教学风格** - 直觉优先、小数字验证、关键观察标注

## 技术栈

| 技术 | 版本 | 用途 |
|------|------|------|
| React | 19 | 前端框架 |
| Vite | 6 | 构建工具 |
| Tailwind CSS | 4 | 样式框架 |
| KaTeX | 0.17 | 数学公式渲染 |
| Lucide React | 0.546 | 图标库 |

## Skill 安装

### 方式一：克隆到工作区

```bash
mkdir -p .trae/skills
cd .trae/skills
git clone https://github.com/ZOUMDA28/learning-tutorial-site.git
```

### 方式二：手动复制

将本仓库中的 `.trae/skills/learning-tutorial-site/` 目录复制到你的工作区 `.trae/skills/` 目录下。

## 使用方法

### 模式一：自有 Notebook

在 TRAE 对话中，当你说以下内容时，该 skill 会被自动调用：

- 「创建一个学习教程网站」
- 「生成 Notebook 教程网站」
- 「做一个在线课程网站」
- 「把 ipynb 转成网页」

### 模式二：大学课程学术教程

当你说以下内容时，会进入大学课程模式：

- 「帮我做一个关于 XXX 研究方向的教程网站，参考斯坦福的课程」
- 「把 MIT 的 XXX 课程做成交互式教程」
- 「我要一个关于 XXX 的学术教程，要有论文引用和代码作业」

模式二默认生成中文内容。如需英文版本，请显式告知（如「也要英文版」），将自动生成 `notebooks-en/` 和 `NOTES.en.md` 英文镜像。

## 项目结构

### 模式一结构

```
tutorial-site/
├── notebooks/                    # 教程笔记本
│   ├── part1-image-processing/
│   │   ├── 数字图像的获取和表示/
│   │   │   ├── practice.ipynb
│   │   │   └── lena.jpeg
│   │   └── 几何变换/
│   │       └── practice.ipynb
│   └── part2-optimization-3d/
├── web/                          # React/Vite 前端网站
├── .github/workflows/
├── README.md                    # 中文文档
├── README.en.md                 # 英文文档
└── LICENSE
```

### 模式二结构（大学课程）

```
university-course-site/
├── notebooks/               # 中文教程笔记本（默认）
│   ├── part1-foundation/
│   │   ├── lecture-01-intro/
│   │   │   ├── practice.ipynb
│   │   │   └── *.png
│   │   └── lecture-02-core/
│   │       └── practice.ipynb
│   ├── part2-advanced/
│   └── part3-frontiers/
├── notebooks-en/            # 英文镜像（仅在用户要求时生成）
├── papers/
│   ├── lecture-01/
│   │   ├── NOTES.md          # 中文研读笔记（默认）
│   │   └── NOTES.en.md       # 英文研读笔记（仅在用户要求时生成）
│   └── lecture-02/
├── web/
├── scripts/
├── .github/workflows/
├── README.md
├── README.en.md
└── LICENSE
```

## 教学契约

每篇 Notebook 必须遵循四步教学路径：

```
直觉理解 → 手算验证 → 代码实现 → 实验观察
```

每个 Notebook 必须包含：课程标题、导读、数学公式、可运行代码、matplotlib 图表、2-3 道作业（含 assert）、参考文献列表。

## 学术引用规范

三种引用方式：行内引用 `[[作者, 年份]](arXiv链接)`、图注出处、参考文献列表。

## 嵌套目录支持

Notebook 可以放在任意深度的子目录中。系统会自动递归扫描、生成唯一 ID、提取标题、重写图片路径。

## 章节排序

侧边栏章节按正序（1, 2, 3... N）连续排列，不按部分重置。

## 快速开始

### 1. 初始化项目

在 TRAE 中说「创建一个学习教程网站」，skill 会自动帮你生成完整的项目结构。

### 2. 添加教程内容

将你的 `.ipynb` 文件放入 `notebooks/` 对应的部分目录中。

### 3. 启动开发服务器

```bash
cd web
npm install
npm run dev
```

### 4. 部署到 GitHub Pages

配置 `.github/workflows/deploy.yml`，推送到 main 分支即可自动部署。

## 自定义主题色

修改 `web/src/styles/index.css` 中的 CSS 变量。

## 最佳实践

1. 教学契约：直觉 → 手算 → 实现 → 实验
2. 代码可读性：短小精炼，充分中文注释
3. 自包含：每篇 Notebook 独立可运行
4. 图片资源：同目录，相对路径引用
5. 固定种子：确保可复现
6. npm install：GitHub Actions 中使用 npm install
7. 学术严谨：每个知识点有论文出处
8. 代码作业：每篇 Notebook 有代码和作业
9. 连续章节编号：不按部分重置
10. 多课程交叉对照：斯坦福优先，对照 MIT/CMU/Berkeley 改写
11. 语言策略：模式二默认中文，英文需显式要求
12. 质量审计：每批 Notebook 对照质量清单审计
13. 批量生成：按模块分批，使用 Python 脚本
14. 预渲染输出：执行 Notebook 嵌入结果
15. 增量推送：每批完成后立即推送部署

## 参考项目

- [modern-llm-notebook](https://github.com/walkinglabs/modern-llm-notebook) - 前端模板基础
- [self-improving-agent-notebook](https://github.com/walkinglabs/self-improving-agent-notebook) - Stanford CS329A 课程模式
- [hands-on-world-models](https://github.com/walkinglabs/hands-on-world-models) - 学术引用规范
- [hands-on-modern-rl](https://github.com/walkinglabs/hands-on-modern-rl) - 章节结构与可视化

## 许可证

MIT License
