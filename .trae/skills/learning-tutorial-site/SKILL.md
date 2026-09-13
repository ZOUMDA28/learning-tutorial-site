---
name: "learning-tutorial-site"
description: "基于 Jupyter Notebook 的交互式学习教程网站生成器。支持两种模式：(1) 基于ipynb笔记本的教程网站，(2) 基于大学课程（斯坦福/MIT等）研究方向生成学术级教程网站，支持多课程交叉对照改写。当用户需要创建在线教程/课程网站时调用。"
---

# Learning Tutorial Site Generator

基于 Jupyter Notebook 的交互式学习教程网站生成器，参考 modern-llm-notebook 项目模板构建。支持从自有 Notebook 或大学公开课程生成学术级教程网站。

## 两种工作模式

### 模式一：自有 Notebook 教程
用户提供 .ipynb 笔记本，生成可浏览的网页教程。适用于已有教学材料的情况。

### 模式二：大学课程学术教程
**用户给定研究方向和学习内容，自动生成基于大学公开课程的学术级教程网站。**

核心要求：
- **优先斯坦福**课程资源，其次 MIT、CMU、UC Berkeley 等顶尖美国大学
- **学术严谨**：每个知识点必须有论文出处，引用最新相关论文
- **代码作业**：不只理论，每篇 Notebook 必须有可运行的代码实现和作业练习
- **图片可视化**：每个核心概念配 matplotlib 生成的图表或论文配图
- **模板一致**：使用与模式一相同的 React + Vite 前端模板
- **多课程交叉对照改写**：以斯坦福为主线，对照 MIT/CMU/Berkeley 同类课程，取长补短进一步改写优化

#### 多课程交叉对照改写流程

模式二不仅从单一课程生成，还支持**对照多个国外大学同类课程进行改写优化**：

**Step 1：主线课程选取（斯坦福优先）**
1. 根据用户研究方向，搜索斯坦福相关课程（如 CS329A、CS224N、CS231N、CS330、CS25 等）
2. 获取课程大纲、阅读清单、讲座安排、作业
3. 以斯坦福课程结构为教程主线

**Step 2：对照课程收集**
1. 搜索 MIT 同类课程（如 6.S191、6.5940、6.867 等）
2. 搜索 CMU 同类课程（如 10-701、10-708、16-824 等）
3. 搜索 UC Berkeley 同类课程（如 CS285、CS294 等）
4. 收集每门课的教学大纲、重点主题、独特视角

**Step 3：交叉对照分析**
1. 将斯坦福主线与对照课程逐章节对比
2. 识别各课程的独特优势和互补内容：
   - 斯坦福偏重：系统性、前沿性、工程实践
   - MIT 偏重：理论基础、数学推导、简洁实现
   - CMU 偏重：统计视角、理论深度、研究方法
   - Berkeley 偏重：强化学习、机器人、实验驱动
3. 标记可以融合改进的知识点

**Step 4：改写优化**
1. 以斯坦福课程为主线骨架
2. 在对应章节融入对照课程的独特视角和补充内容
3. 如果某主题 MIT/CMU 讲得更深入，则对照改写该章节
4. 如果某实验 Berkeley 做得更直观，则参照其实验设计
5. 在 Notebook 中标注内容来源（如「本节视角参考 MIT 6.S191 Lecture 5」）

**Step 5：前沿论文增强**
1. 搜索各课程最新学期的阅读清单
2. 搜索 arXiv 最新相关论文
3. 将最新论文融入对应章节
4. 在 papers/ 目录中记录研读笔记

**交叉对照示例：**

| 主题 | 斯坦福主线 | MIT 对照 | CMU 对照 | Berkeley 对照 | 改写策略 |
|------|-----------|---------|---------|--------------|----------|
| Transformer | CS224N L5 | 6.S191 L3 | 11-711 | - | 以CS224N为主线，融入MIT的简洁实现 |
| RL基础 | CS234 | - | 10-703 | CS285 | 以CS234为主线，融入CS285的实验设计 |
| World Models | CS329A | 6.S191 | - | CS285 | 以CS329A为主线，融入CMU的理论推导 |

#### 课程资源检索方法

**搜索策略：**
1. 使用 WebSearch 搜索 `{大学名} {课程号} {研究方向} course syllabus site:edu`
2. 使用 WebSearch 搜索 `{大学名} {课程号} {年份} lecture notes`
3. 查看课程官网获取 reading list 和 schedule
4. 搜索 `{大学名} {课程号} github` 查找课程代码仓库
5. 搜索 `site:scholar.google.com {研究方向} survey 2024 2025` 找最新综述

**常用课程入口：**
- 斯坦福：`cs.stanford.edu/academicprograms/courses`，或直接搜 `stanford.edu/cs{课程号}`
- MIT：`ocw.mit.edu`，或 `mit.edu/6.{课程号}`
- CMU：`cs.cmu.edu/~{课程号}`
- Berkeley：`cs.berkeley.edu/~{教授名}/{课程号}`

---

## 何时使用

当用户需要：
- 创建一个基于 Jupyter Notebook 的在线教程/课程网站
- 将 .ipynb 笔记本转化为可浏览的网页教程
- 需要中英双语支持的学习平台
- 构建带有代码高亮、数学公式、笔记功能的教程网站
- **基于研究方向生成大学课程级别的学术教程**（模式二）
- **复刻斯坦福等大学公开课程为交互式教程**（模式二）
- **对照多所大学同类课程进行交叉改写优化**（模式二）

## 技术栈

- **前端框架**: React 19 + Vite
- **样式**: Tailwind CSS 4
- **数学公式**: KaTeX
- **图标**: Lucide React
- **核心功能**: 直接渲染 .ipynb 文件，无需后端

## 项目结构

### 模式一结构（自有 Notebook）

```
tutorial-site/
├── notebooks/           # 教程笔记本
│   ├── part1-image-processing/
│   │   ├── 数字图像的获取和表示/
│   │   │   ├── practice.ipynb
│   │   │   └── lena.jpeg
│   │   └── 几何变换/
│   │       └── practice.ipynb
│   └── part2-optimization-3d/
├── web/                 # React/Vite 前端
├── .github/workflows/
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
├── llm_client.py             # LLM 客户端（如需要）
├── .github/workflows/
│   └── deploy.yml
└── README.md
```

---

## 模式二：大学课程学术教程生成指南

### 1. 课程调研与结构设计

**优先级排序：**
1. **斯坦福大学**（Stanford）- CS 系列、AI/ML/RL/Robotics 课程
2. **MIT**（Massachusetts Institute of Technology）- 6.S 系列
3. **CMU**（Carnegie Mellon University）- 10/15/16 系列
4. **UC Berkeley**（University of California, Berkeley）- CS 系列
5. 其他顶尖美国大学的公开课程

**调研步骤：**
1. 根据用户给定的研究方向，搜索相关大学课程
2. 找到课程大纲（syllabus）、课程计划（schedule）、阅读清单（reading list）
3. 梳理课程的知识主线，确定章节划分
4. 收集每节课对应的论文（优先 arXiv 链接）
5. 设计 Notebook 结构，确保代码+理论平衡
6. **对照其他大学同类课程，标记可融合改进的知识点**

**课程结构设计原则：**
- 按课程大纲分为 3-5 个部分（Part）
- 每个部分包含 3-6 节课（Lecture）
- 每节课对应 1 个 practice.ipynb（主练习）
- 可选 1 个 practice_extra.ipynb（拓展练习）
- 总计 10-20 个 Notebook

### 2. 教学契约（Teaching Contract）

**每篇 Notebook 必须遵循四步教学路径：**

```
直觉理解 → 手算验证 → 代码实现 → 实验观察
```

1. **直觉理解**（Markdown）：用通俗语言和类比解释核心概念
2. **手算验证**（Markdown + Code）：用小规模数据手动推导，再用代码验证
3. **代码实现**（Code Cell）：从零实现核心算法，不依赖封装库
4. **实验观察**（Code + Output）：用真实或合成数据运行实验，可视化结果

**每个 Notebook 必须包含：**
- 课程标题和章节号（第一个 markdown cell 为 `# 标题`）
- 导读段落（介绍本节学习目标）
- 核心概念的数学公式（LaTeX）
- 可运行的 Python 代码块
- matplotlib 或其他可视化图表
- 2-3 道作业练习（含 assert 验证）
- 参考文献列表（含 arXiv 链接）
- **内容来源标注**（如「本节视角参考 MIT 6.S191 L5」）

### 3. 学术引用规范

**论文引用格式（三种方式）：**

方式一（行内引用，适用于正文中提及）：
```markdown
[[Ha & Schmidhuber, 2018]](https://arxiv.org/abs/1803.10122)
```

方式二（图注引用，适用于图片出处）：
```markdown
_图 1.1-1：DRQN 的连续帧卷积响应。出处：Hausknecht & Stone，[Deep Recurrent Q-Learning](https://arxiv.org/abs/1507.06527)（2015），Figure 3。_
```

方式三（参考文献列表，放在 Notebook 末尾）：
```markdown
## 参考文献

1. **ReAct: Synergizing Reasoning and Acting in Language Models** - Yao et al., 2022. [arXiv:2210.03629](https://arxiv.org/abs/2210.03629)
2. **STaR: Self-Taught Reasoner** - Zelikman et al., 2022. [arXiv:2203.14465](https://arxiv.org/abs/2203.14465)
```

**论文研读笔记（papers/ 目录）：**
- 每节课创建 `papers/lecture-XX/` 子目录
- 包含 `NOTES.md`（中文研读笔记）和可选的 `NOTES.en.md`（英文）
- 研读笔记内容：论文核心思想、关键公式/算法、实验数字、教学主线、代码演示切入点
- 可选：`scripts/download_papers.py` 脚本按 arXiv ID 下载论文 PDF

### 4. 代码要求

**零依赖原则：**
- 核心算法从零实现，不使用 LangChain、transformers 等封装库
- 仅依赖基础库：numpy, matplotlib, torch（如需深度学习）
- 每个代码块短小精炼（<50行），添加充分中文注释

**作业与验证：**
- 每篇 Notebook 末尾含 2-3 道作业练习
- 代码含 `assert` 语句保证正确性
- 作业应覆盖：概念理解、代码实现、实验分析

**实验可复现：**
- 使用固定随机种子（`np.random.seed(42)`, `torch.manual_seed(42)`）
- 每个 Notebook 独立可运行，不依赖前面的状态

### 5. 图片与可视化要求

**图表类型：**
- matplotlib 生成的图表（曲线图、柱状图、散点图、热力图）
- 算法流程图（可用 matplotlib 或文本图示）
- 架构示意图（可用 matplotlib patches 或 PIL 绘制）
- 实验结果对比图

**图片规范：**
- 图片放在 Notebook 同目录下
- 使用相对路径引用（`![描述](figure.png)`）
- 每张图片必须有中文图注说明
- 引用论文图片时标注出处

**matplotlib 示例：**
```python
import matplotlib.pyplot as plt
import matplotlib
matplotlib.rcParams['font.sans-serif'] = ['SimHei', 'DejaVu Sans']
matplotlib.rcParams['axes.unicode_minus'] = False

fig, ax = plt.subplots(1, 1, figsize=(8, 4))
ax.plot(x, y, label='训练奖励', color='#ea580c', linewidth=2)
ax.set_xlabel('训练步数')
ax.set_ylabel('奖励')
ax.set_title('PPO 训练曲线')
ax.legend()
plt.tight_layout()
plt.savefig('training_curve.png', dpi=150, bbox_inches='tight')
plt.show()
```

### 6. 前沿论文集成

**如何找到最新相关论文：**
1. 搜索 arXiv（使用 WebSearch 工具）
2. 查看顶会论文（NeurIPS, ICML, ICLR, CVPR, RSS, CoRL）
3. 参考大学课程的阅读清单
4. 关注 Google Scholar 引用网络

**论文到 Notebook 的转化流程：**
1. 阅读论文，提取核心算法/公式/实验
2. 在 `papers/lecture-XX/NOTES.md` 中记录研读笔记
3. 将论文核心算法转化为可运行的 Python 代码
4. 用小规模数据验证算法正确性
5. 可视化关键结果
6. 在 Notebook 末尾列出参考文献

### 7. 参考模板

以下三个仓库是本模式的标杆参考：

| 仓库 | 特点 | 课程来源 |
|------|------|---------|
| [self-improving-agent-notebook](https://github.com/walkinglabs/self-improving-agent-notebook) | 17个可运行Notebook，论文研读笔记，零依赖实现 | Stanford CS329A |
| [hands-on-world-models](https://github.com/walkinglabs/hands-on-world-models) | 10章57页，LaTeX图注，arXiv引用 | 原创 |
| [hands-on-modern-rl](https://github.com/walkinglabs/hands-on-modern-rl) | 26章，VitePress，GIF/SVG图，PDF构建 | 原创 |

**从这些模板中学习的关键模式：**
- CS329A 模板：论文→代码的转化流程，papers/ 目录，研读笔记
- World Models 模板：`[[作者, 年份]](链接)` 引用格式，图注出处
- Modern RL 模板：章节+附录结构，GIF/SVG/WEBP 多媒体，TikZ→SVG

---

## 嵌套目录支持

Notebook 可以放在任意深度的子目录中。系统会自动：
- 递归扫描所有 .ipynb 文件
- 使用 `{子目录路径}-{文件名}` 作为唯一 ID（路径分隔符替换为 -）
- 从 Notebook 的第一个 markdown 一级标题提取标题
- 重写 Markdown 中的相对图片路径，指向正确的资源位置

例如 `part1-foundation/lecture-01-intro/practice.ipynb` 的 ID 为 `lecture-01-intro-practice`。

## 图片资源处理

Markdown 单元格中的相对图片路径会被自动重写：
- 原始路径 `![训练曲线](training_curve.png)`
- 重写为 `<img src="./notebooks/part1-foundation/lecture-01-intro/training_curve.png">`
- 绝对 URL 和 data: URI 不受影响

部署时需要将 notebooks 目录复制到构建输出目录（docs/notebooks/），使图片可访问。

## 章节排序与编号

侧边栏中的章节按正序（1, 2, 3... N）连续排列，不按部分重置。

### 排序机制

在 `notebooks.js` 中定义 `CHAPTER_ORDER` 映射表，使用连续编号：

```javascript
const CHAPTER_ORDER = {
  // 第一部分（第1-5章）
  'lecture-01-intro': 1,
  'lecture-02-core': 2,
  ...
  // 第二部分（第6-10章，连续编号不重置）
  'lecture-06-advanced': 6,
  ...
}
```

排序逻辑（三级排序）：
1. 按 `PARTS` 顺序（part1 在 part2 之前）
2. 按 `CHAPTER_ORDER[dir]`（章节号正序排列）
3. `practice.ipynb` 在 `practice_extra.ipynb` 之前

### getCatalog 必须传递 chapterOrder

`getCatalog()` 返回的对象必须包含 `chapterOrder` 字段：

```javascript
export function getCatalog() {
  return NOTEBOOKS.map((entry) => ({
    id: entry.id,
    title: entry.title,
    part,
    partDir: entry.partDir,
    chapterOrder: entry.chapterOrder,
  }))
}
```

### 侧边栏章节号显示

`Sidebar.jsx` 中的 `buildSidebarSections` 使用 `item.chapterOrder` 生成章节号徽章：
- `practice.ipynb` 显示为 `1`, `2`, `3`...
- `practice_extra.ipynb` 显示为 `4+`, `5+`...（带 + 号表示拓展）

### 添加新章节

新增章节时，在 `CHAPTER_ORDER` 映射表中添加对应目录名和序号即可（使用下一个连续数字）。未在映射表中的目录会排在最后（序号 999）。

## 核心组件

| 组件 | 功能 |
|------|------|
| `Sidebar.jsx` | 左侧边栏导航，按部分组织教程，章节号正序排列 |
| `NotebookViewer.jsx` | Notebook 渲染器，显示内容和右侧大纲 |
| `Welcome.jsx` | 首页欢迎页，展示课程概览 |
| `NotesPanel.jsx` | 笔记和书签管理面板 |
| `SettingsPanel.jsx` | 设置面板（主题、字号） |
| `ImageLightbox.jsx` | 图片灯箱查看器 |

## 核心数据模块

**data/notebooks.js**:
- 使用 `import.meta.glob('../../../notebooks/**/*.ipynb', { query: '?raw', import: 'default' })` 动态加载
- 嵌套路径正则匹配：`rootDir/([^/]+)/(.+?)\.ipynb$`
- 计算 imageBase 路径用于图片 URL 重写
- `CHAPTER_ORDER` 映射表控制章节排序（连续编号1-N，不按部分重置）
- 每个条目包含 `chapterOrder` 字段，`getCatalog()` 必须传递此字段
- 实现 Markdown 渲染（标题、列表、表格、引用、代码块）
- 实现 Python 代码高亮
- 实现 Notebook 单元格渲染（markdown cell, code cell, output）
- 支持 KaTeX 数学公式渲染
- 实现内存缓存和预取机制

**data/sidebar.js**:
- 定义学习路径（PATH_STEPS）
- 定义精选笔记本（RUNNABLE_NOTEBOOKS）
- lessonId 必须与 notebooks.js 生成的 ID 匹配

## Vite 配置

**vite.config.js** 关键配置：
- 自定义虚拟模块插件 `virtual:notebook-catalog`
- 递归扫描 notebooks 目录，生成 catalog
- 嵌套路径正则：`rel.match(/^([^/]+)\/(.+)\.ipynb$/)`
- ID 生成：`idPath.replace(/\//g, '-')`
- 从 Notebook JSON 提取第一个 `#` 标题作为显示标题
- 构建输出到 `../docs` 目录
- `base: './'` 确保相对路径

## 主题自定义

修改 `web/src/styles/index.css` 中的 CSS 变量。橙色主题示例：

```css
:root {
  --accent: #ea580c;
  --accent-soft: rgba(234, 88, 12, 0.09);
  --brand-gradient-from: rgba(234, 88, 12, 0.11);
  --brand-gradient-to: rgba(251, 146, 60, 0.12);
  --brand-accent: #ea580c;
  --code-inline-bg: #fff7ed;
  --code-inline-text: #c2410c;
  --blockquote-bg: #fff7ed;
  --blockquote-border: rgba(234, 88, 12, 0.1);
}

[data-theme="dark"] {
  --accent: #fb923c;
  --accent-soft: rgba(251, 146, 60, 0.12);
  --brand-gradient-from: rgba(251, 146, 60, 0.11);
  --brand-gradient-to: rgba(255, 180, 0, 0.12);
  --brand-accent: #fb923c;
}
```

## GitHub Actions 部署

**.github/workflows/deploy.yml** 使用双部署策略，兼容两种 Pages 源设置：

### 权限配置

```yaml
permissions:
  contents: write
  pages: write
  id-token: write
```

### 关键步骤

1. 递归复制中文目录到 notebooks 结构
2. 安装依赖（使用 npm install，非 npm ci）
3. 构建：`npm run build`
4. 复制 notebook 资源（图片等）到构建输出
5. 部署方式 A：actions/deploy-pages（Pages 源 = GitHub Actions）
6. 部署方式 B：peaceiris/actions-gh-pages（Pages 源 = gh-pages 分支）

### GitHub Pages 源设置

**选项 A（推荐）**：Source = GitHub Actions
**选项 B**：Source = Deploy from a branch → gh-pages → / (root)

## Notebook 组织规范

教程按部分组织，每个部分一个顶层文件夹：
- `part1-foundation/` - 基础
- `part2-advanced/` - 进阶
- `part3-frontiers/` - 前沿

每个部分内可以有子目录，每个子目录包含：
- `practice.ipynb` - 主练习 Notebook
- `practice_extra.ipynb` - 拓展练习（可选）
- 图片资源文件（.png, .jpg, .svg）

确保每个 Notebook 的第一个 markdown cell 是一级标题（# 标题），用于自动提取显示名称。

## 最佳实践

1. **教学契约**: 每篇 Notebook 遵循「直觉 → 手算 → 实现 → 实验」的学习路径
2. **代码可读性**: 保持代码单元格短小，添加充分的中文注释
3. **自包含**: 每篇 Notebook 应该可以独立运行，不依赖前面的状态
4. **图片资源**: 图片放在 Notebook 同目录下，使用相对路径引用
5. **固定种子**: 涉及随机实验时使用固定种子，确保可复现
6. **npm install**: 在 GitHub Actions 中使用 npm install 而非 npm ci
7. **双部署策略**: 同时使用 actions/deploy-pages 和 peaceiris/actions-gh-pages
8. **force_orphan**: gh-pages 分支使用 orphan commit，不保留构建历史
9. **连续章节编号**: CHAPTER_ORDER 使用连续编号，不按部分重置；getCatalog 必须传递 chapterOrder
10. **学术严谨**: 每个知识点必须有论文出处，使用标准引用格式
11. **代码作业**: 每篇 Notebook 必须有可运行代码和作业练习（含 assert 验证）
12. **论文研读**: papers/ 目录记录每节课的论文研读笔记，含中英双语
13. **零依赖实现**: 核心算法从零实现，不使用封装库
14. **可视化**: 每个核心概念配 matplotlib 图表或论文配图
15. **多课程交叉对照**: 以斯坦福为主线，对照 MIT/CMU/Berkeley 同类课程改写优化，标注内容来源

## 参考模板

- [modern-llm-notebook](https://github.com/walkinglabs/modern-llm-notebook) - 前端模板基础
- [self-improving-agent-notebook](https://github.com/walkinglabs/self-improving-agent-notebook) - Stanford CS329A 课程模式
- [hands-on-world-models](https://github.com/walkinglabs/hands-on-world-models) - 学术引用规范
- [hands-on-modern-rl](https://github.com/walkinglabs/hands-on-modern-rl) - 章节结构与可视化
