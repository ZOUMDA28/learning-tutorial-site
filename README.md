# Learning Tutorial Site

基于 Jupyter Notebook 的交互式学习教程网站生成器。

> 参考 [modern-llm-notebook](https://github.com/walkinglabs/modern-llm-notebook) 项目模板构建的 TRAE Skill | [English](./README.en.md)

## 强制第一步：先对照参考网站

**每次做或改网站，动手前先打开参考项目，把它当格式基准**（本地优先，其次 GitHub）：

| 参考 | 位置 | 用途 |
|------|------|------|
| modern-llm-notebook | `modern-llm-notebook/web/` | 前端组件、设计系统 CSS、构建与部署配置的唯一基准 |

必读：`web/src/styles/index.css`（完整设计系统）、`web/src/components/`（组件清单）、
`web/index.html`（字体与公式引入）、`web/vite.config.js`、`.github/workflows/`。

**不要凭印象重写样式或组件，也不要裁剪参考项目的设计系统**——这是踩过的坑：
曾把 `index.css` 裁成空壳，导致 93 个样式类与 10 个 CSS 变量缺失，页面大面积失去样式。

## 本次更新（v2）

在原有能力之上，补齐了「网站做出来但看起来不对」这一类问题的完整排查经验：

- **强制第一步**：任何网站任务先对照参考项目（见上节）
- **样式完整性对账**：脚本化检查「组件用到的类/变量 ↔ CSS 定义」，要求 0 缺失
- **KaTeX 随包内置**：不再从 CDN 加载公式样式（受限网络下会整体失效）
- **渲染器硬约束**：表格分隔行至少 3 个短横线、强调不跨行、需支持 `\*` 转义、图片路径前缀
- **页面标题去重**：渲染时剥离正文首个 H1（页面顶部已由 `meta.title` 渲染）
- **章节编号改为目录名推导**：废止「用章节标题做排序键」的做法（会集体退化成 999）
- **一个学习要点一个 Notebook**：废止 `4+` 这类拓展后缀，拓展内容进附录
- **首页卡片绑定真实 notebook id**：修复 prop 名不匹配 / id 拼错导致的点击失效
- **渲染体检（Vite SSR）**：用真实渲染器把每篇 Notebook 渲染成 HTML 做机器检查
- **复现基础设施**：`utils.py` + `requirements.txt` + 注册 ipykernel，并约定
  「先执行到临时文件、校验无 error 再覆盖源文件」
- **部署瘦身**：`rsync --exclude` 排除大体量数据集；发布前做仓库卫生清理
- **主题换色两处改**：CSS 变量 + 组件内字面颜色类（如 `blue→orange`）
- **症状 → 原因 → 处理速查表**：11 类常见故障的对症处理

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
- **样式完整性对账** - 脚本检查组件用到的样式类与 CSS 变量是否有定义（0 缺失）
- **渲染体检** - 用 Vite SSR 跑真实渲染器，机器检查表格/公式/标题是否正常
- **复现基础设施** - `utils.py` + `requirements.txt` + ipykernel 注册，支持一键重跑 Notebook
- **症状速查表** - 11 类常见前端故障的「症状 → 原因 → 处理」对照

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

章节号**从目录名的前导数字推导**（目录形如 `01-digital-image-acquisition`），因此新增章节
只要用两位数前缀命名即可，不需要维护映射表；未编号目录（如 `appendix/`）自动排到最后。
附录用 `A1`/`A2` 徽章，单独成组。

> 不要用章节标题做排序键——目录名与标题一旦不一致，编号会集体退化成 `999`。

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

改主色要**同时改两处**，否则会「改了一半」：

1. **CSS 变量**：在 `web/src/styles/index.css` 末尾追加覆盖块（后写覆盖前写），
   同时覆盖 `:root` 与 `[data-theme="dark"]`：背景、边框、`--accent`、品牌渐变、
   引用块、行内代码、表格、滚动条、Modal。
2. **组件里的字面颜色类**：`Welcome.jsx` 这类页面有 `bg-blue-600`、`from-cyan-500`、
   `from-[#dbeafe]` 等**不随变量变化**的字面颜色。整体转橙色的映射：
   `blue→orange`、`cyan→amber`、`purple/violet→rose`、`indigo→orange`，卡片渐变改为暖色阶。

校验：构建产物 CSS 应含新色值；主包 JS 中旧色类计数为 0、新色类计数 > 0。

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
16. 参考优先：任何网站任务动手前先读参考项目的样式与组件
17. 样式对账：移植或精简 CSS 后跑脚本确认「缺失类 0、缺失变量 0」
18. KaTeX 随包内置，不要 CDN
19. 表格分隔行至少 3 个短横线；强调不要跨行
20. 剥离正文首个 H1，避免标题重复
21. 章节号从目录名推导；`getCatalog()` 传 `chapterOrder` 与 `numLabel`
22. 一个学习要点一个 Notebook，拓展进附录（不用 `+` 后缀）
23. 首页卡片绑定真实 notebook id
24. 复现三件套：`utils.py` + `requirements.txt` + 注册 ipykernel
25. 执行 Notebook 走「临时文件 → 校验 → 覆盖」
26. 用 Vite SSR 渲染体检做机器验收，不要只靠肉眼
27. 部署用 `rsync --exclude` 排除大体量数据集
28. 断言写「关系」（内部区域严格相等 / A < B），不写脆弱常数
29. 换主题色改两处：CSS 变量 + 组件字面颜色类

## 参考项目

- [modern-llm-notebook](https://github.com/walkinglabs/modern-llm-notebook) - 前端模板基础
- [self-improving-agent-notebook](https://github.com/walkinglabs/self-improving-agent-notebook) - Stanford CS329A 课程模式
- [hands-on-world-models](https://github.com/walkinglabs/hands-on-world-models) - 学术引用规范
- [hands-on-modern-rl](https://github.com/walkinglabs/hands-on-modern-rl) - 章节结构与可视化

## 许可证

MIT License
