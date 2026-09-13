# Learning Tutorial Site

基于 Jupyter Notebook 的交互式学习教程网站生成器。

> 参考 [modern-llm-notebook](https://github.com/walkinglabs/modern-llm-notebook) 项目模板构建的 TRAE Skill

## 两种工作模式

### 模式一：自有 Notebook 教程
用户提供 .ipynb 笔记本，生成可浏览的网页教程。

### 模式二：大学课程学术教程
用户给定研究方向和学习内容，自动基于大学公开课程（优先斯坦福，其次 MIT/CMU/Berkeley）生成学术级教程网站。

**核心要求：**
- 学术严谨：每个知识点必须有论文出处，引用最新 arXiv 论文
- 代码作业：每篇 Notebook 必须有可运行的代码实现和作业练习（含 assert 验证）
- 图片可视化：每个核心概念配 matplotlib 图表或论文配图
- 论文研读：papers/ 目录记录每节课的论文研读笔记
- 零依赖实现：核心算法从零实现，不使用封装库

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

## 项目结构

### 模式一结构

```
tutorial-site/
├── notebooks/                    # 教程笔记本
│   ├── part1-image-processing/   # 第一部分
│   │   ├── 数字图像的获取和表示/  # 子目录（支持嵌套）
│   │   │   ├── practice.ipynb
│   │   │   └── lena.jpeg         # 图片资源
│   │   └── 几何变换/
│   │       └── practice.ipynb
│   └── part2-optimization-3d/    # 第二部分
├── web/                          # React/Vite 前端网站
│   ├── src/
│   │   ├── components/           # React 组件
│   │   ├── context/              # Context 状态管理
│   │   ├── data/                 # 数据配置
│   │   │   ├── notebooks.js      # Notebook 加载与渲染
│   │   │   └── sidebar.js        # 侧边栏数据
│   │   ├── hooks/                # 自定义 Hooks
│   │   ├── styles/               # 全局样式
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── index.html
│   ├── package.json
│   └── vite.config.js
├── .github/workflows/           # GitHub Actions
│   └── deploy.yml               # 自动部署工作流
└── README.md
```

### 模式二结构（大学课程）

```
university-course-site/
├── notebooks/               # 教程笔记本（按课程结构组织）
│   ├── part1-foundation/     # 第一部分：基础
│   │   ├── lecture-01-intro/
│   │   │   ├── practice.ipynb
│   │   │   └── *.png         # matplotlib 图表
│   │   └── lecture-02-core/
│   │       └── practice.ipynb
│   ├── part2-advanced/       # 第二部分：进阶
│   └── part3-frontiers/      # 第三部分：前沿
├── papers/                   # 论文研读笔记
│   ├── lecture-01/
│   │   ├── NOTES.md          # 中文研读笔记
│   │   └── NOTES.en.md       # 英文研读笔记
│   └── lecture-02/
│       └── ...
├── web/                      # React/Vite 前端（与模式一相同）
├── scripts/
│   └── download_papers.py    # arXiv 论文下载脚本
├── .github/workflows/
│   └── deploy.yml
└── README.md
```

## 教学契约

每篇 Notebook 必须遵循四步教学路径：

```
直觉理解 → 手算验证 → 代码实现 → 实验观察
```

1. **直觉理解**：用通俗语言和类比解释核心概念
2. **手算验证**：用小规模数据手动推导，再用代码验证
3. **代码实现**：从零实现核心算法，不依赖封装库
4. **实验观察**：用真实或合成数据运行实验，可视化结果

每个 Notebook 必须包含：课程标题、导读、数学公式、可运行代码、matplotlib 图表、2-3 道作业（含 assert）、参考文献列表。

## 学术引用规范

三种引用方式：

```markdown
<!-- 行内引用 -->
[[Ha & Schmidhuber, 2018]](https://arxiv.org/abs/1803.10122)

<!-- 图注引用 -->
_图 1.1-1：DRQN 的连续帧卷积响应。出处：Hausknecht & Stone，[论文](链接)（2015），Figure 3。_

<!-- 参考文献列表 -->
## 参考文献
1. **论文名** - 作者, 年份. [arXiv:ID](链接)
```

## 嵌套目录支持

Notebook 可以放在任意深度的子目录中。系统会自动：
- 递归扫描所有 .ipynb 文件
- 使用 `{子目录路径}-{文件名}` 作为唯一 ID
- 从 Notebook 的第一个 markdown 一级标题提取标题
- 重写 Markdown 中的相对图片路径

## 章节排序

侧边栏章节按正序（1, 2, 3... N）连续排列，不按部分重置。在 `notebooks.js` 中定义 `CHAPTER_ORDER` 映射表控制顺序。

## 图片资源处理

Markdown 中的相对图片路径会被自动重写为正确的绝对路径。部署时需将 notebooks 目录复制到构建输出目录。

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

修改 `web/src/styles/index.css` 中的 CSS 变量：

```css
:root {
  --accent: #ea580c;           /* 主色调（橙色示例） */
  --brand-gradient-from: ...;  /* 渐变起始色 */
  --brand-gradient-to: ...;    /* 渐变结束色 */
}
```

## 最佳实践

1. **教学契约**: 每篇 Notebook 遵循「直觉 → 手算 → 实现 → 实验」的学习路径
2. **代码可读性**: 保持代码单元格短小，添加充分的中文注释
3. **自包含**: 每篇 Notebook 应该可以独立运行
4. **图片资源**: 图片放在 Notebook 同目录下，使用相对路径引用
5. **固定种子**: 涉及随机实验时使用固定种子
6. **npm install**: 在 GitHub Actions 中使用 npm install 而非 npm ci
7. **学术严谨**: 每个知识点必须有论文出处，使用标准引用格式
8. **代码作业**: 每篇 Notebook 必须有可运行代码和作业练习
9. **连续章节编号**: CHAPTER_ORDER 使用连续编号，不按部分重置

## 参考项目

- [modern-llm-notebook](https://github.com/walkinglabs/modern-llm-notebook) - 前端模板基础
- [self-improving-agent-notebook](https://github.com/walkinglabs/self-improving-agent-notebook) - Stanford CS329A 课程模式
- [hands-on-world-models](https://github.com/walkinglabs/hands-on-world-models) - 学术引用规范
- [hands-on-modern-rl](https://github.com/walkinglabs/hands-on-modern-rl) - 章节结构与可视化

## 许可证

MIT License
