---
name: "learning-tutorial-site"
description: "基于 Jupyter Notebook 的交互式学习教程网站生成器（React+Vite+KaTeX），含前端样式对账与渲染体检。支持笔记高亮、书签、搜索筛选、代码折叠、图片灯箱、大纲导航等完整功能。两种模式：(1) 自有 ipynb 教程网站；(2) 按研究方向生成大学课程级学术教程（2026 更新：引用核验、现代工具链 uv、CI 校验、开源合规）。默认中文，英文需告知。当用户要创建/修改在线教程或课程网站时调用。"
---

# Learning Tutorial Site Generator

基于 Jupyter Notebook 的交互式学习教程网站生成器，参考 modern-llm-notebook 项目模板构建。支持从自有 Notebook 或大学公开课程生成学术级教程网站。

## 强制第一步：先对照参考网站（每次做/改网站都必须执行）

**任何"做网站 / 改网站"的任务，动手之前必须先打开参考项目，把它当作格式基准。**

| 参考 | 位置 | 用途 |
|------|------|------|
| modern-llm-notebook | `modern-llm-notebook/web/`（本地优先，其次 GitHub） | 前端组件、设计系统 CSS、构建与部署配置的唯一基准 |
| Hands-On-Computer-Vision | 本地项目或 GitHub `ZOUMDA28/Hands-On-Computer-Vision` | 笔记高亮、书签、搜索筛选、Welcome 页完整结构、橙色主题等增强功能的参考实现 |

**动手前的必读清单（逐项打开确认）**：

1. `web/src/styles/index.css` —— 完整设计系统（CSS 变量 + 全部自定义类 + KaTeX 与字体 `@import`）；
2. `web/src/components/` —— 组件清单（注意 NotesPanel / SettingsPanel / ImageLightbox 这类容易漏掉的组件）；
3. `web/src/data/notebooks.js` —— Notebook 渲染器、Markdown 解析、代码高亮、缓存与预取；
4. `web/src/data/sidebar.js` —— 学习路径定义、精选 Notebook 卡片数据；
5. `web/index.html` —— 字体的 preconnect 与样式表引入方式；
6. `web/vite.config.js` —— 虚拟模块插件、`base: './'`、构建输出目录；
7. `.github/workflows/` —— 部署方式与 GitHub Pages 源设置。

**规则**：

- **不得凭印象重写样式或组件**。样式一律以参考项目的 `index.css` 为准；需要换主题色时只做
  「CSS 变量覆盖 + 组件内字面颜色类映射」，不要重写设计系统。
- 参考项目已有的组件与样式**不要自行裁剪**；确需删减时，必须跑「样式完整性对账」脚本确认 0 缺失。
- 每次接到网站任务，先明确说出"我参考的是哪个项目、读了哪几个文件"，再动手。
- 参考项目不可得时，先向用户确认模板来源，**不要自行发明结构**。

> 这条规则来自真实事故：曾因把 `index.css` 裁成空壳，造成 93 个样式类与 10 个 CSS 变量缺失，
> 页面大面积失去样式并反复返工。

## 语言策略

- **模式一**：跟随用户 Notebook 语言
- **模式二**：**默认中文**生成所有 Notebook 和研读笔记。如需英文版本，用户须显式告知，届时生成 `notebooks-en/` 和 `NOTES.en.md` 英文镜像
- **README**：项目同时提供中文 README.md 和英文 README.en.md 两个版本

## 两种工作模式

### 模式一：自有 Notebook 教程
用户提供 .ipynb 笔记本，生成可浏览的网页教程。适用于已有教学材料的情况。

**可选：对照国外课程进一步改写**

用户如有需要，可以对照斯坦福、MIT、CMU、UC Berkeley 等国外大学同类课程的公开内容，对自有 Notebook 进行进一步改写优化：

1. **对照课程选取**：根据 Notebook 主题，搜索斯坦福等大学的同类课程（如 CS224N、CS231N、CS234、CS329A 等），获取课程大纲、讲义、阅读清单
2. **逐篇对照分析**：将用户 Notebook 与课程对应章节对比，识别可补充和改进的知识点
3. **改写优化**：
   - 补充课程中更深入的理论推导（如 MIT 的数学推导风格）
   - 融入课程中更直观的实验设计（如 Berkeley 的实验驱动风格）
   - 增加课程中涉及但用户 Notebook 缺少的前沿论文引用
   - 标注内容来源（如「本节理论推导参考 Stanford CS224N Lecture 5」）
4. **保持用户内容主体**：改写以用户原有 Notebook 为主线，对照课程为补充和优化，不喧宾夺主
5. **斯坦福优先**：对照课程选择优先级为 斯坦福 > MIT > CMU > UC Berkeley

### 模式二：大学课程学术教程
**用户给定研究方向和学习内容，自动生成基于大学公开课程的学术级教程网站。**

**默认中文**：所有 Notebook、研读笔记、侧边栏、欢迎页均以中文生成。如需英文版本，用户须显式告知（如「也要英文版」），届时生成 `notebooks-en/` 英文镜像目录和 `NOTES.en.md` 英文研读笔记。

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

**引用真实性硬规则（防幻觉）**：
- 每条引用写入 Notebook 前，**必须用 WebSearch/WebFetch 打开 arXiv 页面核实**：标题、作者、年份、arXiv ID 四项一致才可写入；
- **严禁凭记忆编造论文标题、作者或 arXiv ID**——无法核实的引用直接删除，宁缺毋假；
- 课程编号同样要核实（课程号每年会调整，如 MIT 6.5940 曾为 6.S193/6.5940 演变），以课程官网当年页面为准。

**Step 5：前沿论文增强（2025–2026 科研需要）**
1. 搜索各课程最新学期的阅读清单
2. 搜索 arXiv 最新相关论文
3. 将最新论文融入对应章节
4. 在 papers/ 目录中记录研读笔记

**交叉对照示例：**

| 主题 | 斯坦福主线 | MIT 对照 | CMU 对照 | Berkeley 对照 | 改写策略 |
|------|-----------|---------|---------|--------------|------------|
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

**常用课程入口（含 2025–2026 热门课，检索后仍须核实当年课程号）：**
- 斯坦福：`cs.stanford.edu/academicprograms/courses`，或直接搜 `stanford.edu/cs{课程号}`；热门：CS224N（NLP/LLM）、CS231N（CV）、CS336（Language Modeling from Scratch）、CS330（多任务与元学习）、CS25（Transformers United）
- MIT：`ocw.mit.edu`，或 `mit.edu/6.{课程号}`；热门：6.S191（深度学习导论，每年更新 LLM 内容）、6.5940（TinyML 高效深度学习）、6.867（ML 基础）
- CMU：cs.cmu.edu/~{课程号}；热门：10-701/10-708（ML/深度学习）、11-711（NLP）
- Berkeley：`cs.berkeley.edu/~{教授名}/{课程号}`；热门：CS285（深度 RL）、CS294
- 其他可参考：Princeton COS 597G（理解大语言模型）、国内高校公开课（可选补充，须核实公开许可）

---

## 何时使用

当用户需要：
- 创建一个基于 Jupyter Notebook 的在线教程/课程网站
- 将 .ipynb 笔记本转化为可浏览的网页教程
- 需要中英双语支持的学习平台
- 构建带有代码高亮、数学公式、笔记功能的教程网站
- **基于研究方向生成大学课程级别的学术教程**（模式二）
- **复刻斯坦福等大学公开课程为交互式教程**（模式二）
- **对照多所大学同类课程进行交叉改写优化**（模式一/二均支持）
- 需要笔记高亮、书签、搜索筛选、大纲导航等完整学习体验

## 技术栈

- **前端框架**: React 19 + Vite
- **样式**: Tailwind CSS 4
- **数学公式**: KaTeX
- **图标**: Lucide React
- **核心功能**: 直接渲染 .ipynb 文件，无需后端
- **增强功能**: 笔记高亮、书签、搜索筛选、代码折叠、图片灯箱、大纲导航、预取缓存

## 项目结构

### 模式一结构（自有 Notebook）

```
tutorial-site/
├── notebooks/           # 教程笔记本
│   ├── part1-image-processing/
│   │   ├── 01-digital-image-acquisition/
│   │   │   ├── practice.ipynb
│   │   │   └── lena.jpeg
│   │   └── 06-geometric-transformation/
│   │       └── practice.ipynb
│   ├── part2-optimization-3d/
│   └── appendix/        # 附录：拓展主题
├── web/                 # React/Vite 前端
│   └── src/
│       ├── components/  # React 组件
│       ├── data/        # 数据配置与渲染逻辑
│       ├── hooks/       # 自定义 Hooks
│       ├── context/     # React Context
│       ├── styles/      # 全局样式
│       ├── App.jsx
│       ├── config.js
│       └── main.jsx
├── .github/workflows/
├── utils.py             # 基础设施（中文路径、随机种子、中文字体）
├── requirements.txt
├── README.md            # 中文文档
├── README.en.md         # 英文文档
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
├── papers/                   # 论文研读笔记
│   └── lecture-XX/
│       ├── NOTES.md          # 中文研读笔记（默认）
│       └── NOTES.en.md       # 英文研读笔记（仅在用户要求时生成）
├── web/                      # React/Vite 前端（与模式一相同）
├── scripts/
│   └── download_papers.py
├── .github/workflows/
├── README.md / README.en.md
└── LICENSE
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
- 可选 1 个 practice_extra.ipynb（拓展练习）—— **拓展内容优先收入附录**
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

### 3. Notebook 质量标准

#### 四步教学路径（强制执行）

每篇 Notebook **必须** 遵循以下结构，顺序不可调换：

1. **Intuition（直觉）** — 在公式之前建立直觉。使用类比、真实世界例子、「为什么这很重要？」
2. **Manual Calculation（手算验证）** — 小数字例子，读者可以手工验证（例如 InfoNCE 的 batch_size=2，双边滤波的 3×3 邻域）
3. **Code Implementation（代码实现）** — 零依赖实现，含 assert 验证，固定随机种子
4. **Experiment & Observation（实验与观察）** — 可视化，带 `★ Key Observation` 标注，将输出与概念联系起来

#### 教学风格参考（walkinglabs.github.io）

- **直觉优先**：先讲「为什么重要」，再给公式
- **小数字**：使用 2-4 元素向量、3×3 矩阵、batch_size=2 — 读者可手工验证
- **表格胜过长文**：用表格比较方法/概念
- **关键观察**：每节末尾用 `> ★ **Key Observation**: ...` 总结
- **具体例子**：用「猫/狗」而非「x₁/x₂」，用「(3,2,5)」而非「(X,Y,Z) 抽象」
- **分步推导**：展示中间计算步骤，不只是最终结果

#### 质量审计清单

每篇 Notebook 生成后，必须对照以下清单进行审计：

| 检查项 | 验证方法 |
|--------|----------|
| 直觉先于公式 | 每个概念的首次提及都有非数学解释 |
| 手算验证 | 至少一个可手工追踪的小例子 |
| 代码来源标注 | Notebook 末尾列出 GitHub 仓库 URL 和作业/讲座来源 |
| 文字/公式比例 | ≥ 20% 文字解释（不是公式堆砌） |
| 关键观察 | 关键代码输出后有 `★` 标注 |
| 固定种子 | `set_random_seed(42)` 或等效设置 |
| 独立单元格 | 每个代码单元格无需前面单元格即可运行（理想情况） |
| 中文语言 | 默认中文；仅在明确要求时使用英文 |

#### 常见质量问题与修复

| 问题 | 修复方法 |
|------|----------|
| 公式堆砌无解释 | 公式前增加直觉段落，公式后增加小例子 |
| 函数名不一致 | 理论和实践部分的函数名对齐 |
| 缺少手算验证 | 插入 2-3 元素向量的分步示例 |
| 只有抽象符号 | 将 `X, Y, Z` 替换为具体的 `(3, 2, 5)` |
| 无关键观察 | 每个代码输出块后添加 `> ★ **Key Observation**: ...` |

### 4. 代码来源规则

向 Notebook 添加代码时：

1. **优先级**：GitHub 热门仓库（★ > 1000）优先 → 最新课程作业 → 论文
2. **标注来源**：在 Notebook 末尾，列出 GitHub 仓库 URL 和作业/讲座来源
3. **改编而非复制**：将代码适配到 Notebook 风格，添加中文注释，使用固定种子
4. **分层依赖**：教学核心层手动实现（仅 numpy/torch 基础 API）；实际应用层直接用现代生态库（transformers/trl/peft 等），注明版本

### 5. 学术引用规范

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
- 默认生成 `NOTES.md`（中文研读笔记）
- 仅在用户要求英文版本时生成 `NOTES.en.md`（英文）
- 研读笔记内容：论文核心思想、关键公式/算法、实验数字、教学主线、代码演示切入点
- 可选：`scripts/download_papers.py` 脚本按 arXiv ID 下载论文 PDF

### 6. 代码要求（2026 科研与实际应用标准）

**双层代码策略（教学核心从零实现 + 实际应用用现代生态）：**

| 层次 | 适用场景 | 依赖策略 |
|------|----------|----------|
| **教学核心层** | 算法原理讲解（如注意力机制、梯度更新、采样循环） | 从零实现，只用 numpy / torch 基础 API，让读者看清数学本质 |
| **实际应用层** | 与论文 SOTA 对齐的实验、贴近生产环境的 demo | **直接使用现代科研生态**，不要重复造轮子 |

- **教学核心层**：核心算法从零实现，每个代码块短小精炼（<50行），添加充分中文注释
- **实际应用层**：优先使用经过验证的现代库，并给出版本与官方文档链接：
  - LLM/NLP：`transformers`、`trl`（SFT/DPO/GRPO）、`peft`（LoRA/QLoRA）、`accelerate`、`datasets`
  - 推理部署：`vllm`、`ollama`、`llama.cpp`（至少给一种本地推理示例）
  - 视觉/多模态：`timm`、`diffusers`
  - 实验管理：`wandb` 或最小化的 JSON/CSV 日志（二选一，注明为什么）
- **禁用标准**：不用 LangChain 这类抽象层级过高、API 变动剧烈的封装框架做教学主线；Agent 教学直接用 `openai`/`anthropic` SDK + 手写工具调用循环，反而更清晰

**代码工程质量（实际应用要求）：**
- 函数签名带**类型注解**，关键函数附 docstring
- 训练类代码必须包含：混合精度（`torch.autocast`/`accelerate`）、梯度累积说明、checkpoint 保存/恢复的最小示例
- 代码风格参考主流开源仓库（ruff 格式化可选项），变量命名与论文符号对应（如 `logits`、`d_model`）
- 不再手写"读配置 magic number"——超参集中在 Notebook 顶部一个 dict/dataclass 中

**作业与验证：**
- 每篇 Notebook 末尾含 2-3 道作业练习
- 代码含 `assert` 语句保证正确性
- 作业应覆盖：概念理解、代码实现、实验分析；实际应用层可再增 1 道"工程迁移题"（如把手写实现换成 `trl` 版本并对比结果）

**实验可复现：**
- 使用固定随机种子（`np.random.seed(42)`, `torch.manual_seed(42)`；实际应用层用 `transformers.set_seed(42)`）
- 每个 Notebook 独立可运行，不依赖前面的状态
- 关键实验给出运行硬件与耗时报表（如「RTX 4060 8GB，约 12 分钟」）

**代码风格参考（easy-llm-master 规范，本地 `D:/CODE/easy-llm-master`）：**

写训练脚本和模型实现时以该仓库为风格基准，其可复用的做法：

1. **文件头 docstring 自带使用说明**：说明脚本结构、支持的功能开关、可直接复制运行的命令行示例（Windows/Linux 通用）
2. **中文 docstring 三段式**：功能一句话 → 参数列表（`参数名: 类型 说明`）→ 返回形状；张量形状始终标注（如 `[..., d_model]`、`(B, m, vocab_size)`）
3. **注释解释「为什么」而不是「是什么」**：如「裁剪前必须 `scaler.unscale_`，否则阈值是对放大后的梯度生效的」
4. **行内注释带形状变化**：`# (batch_size, seq_len) -> (batch_size, seq_len, embed_dim)`
5. **assert 前置条件**：如 `assert d_k % 2 == 0, "RoPE 要求向量维度 d_k 必须是偶数"`，在错误发生处就拦截
6. **消融开关设计**：组件自带开关（如 `remove: bool = False`、`--no_rmsnorm`），方便对照实验而不需改代码
7. **用 einops/einsum 显式表达维度语义**：`einsum(x, w, "... d_in, d_out d_in -> ... d_out")` 替代裸 matmul
8. **超参集中 + 实验命名规范**：`--run_name` 按「类型-lr峰值_谷值-bs-种子」拼（如 `lr_sweep-lr3e-4_3e-5-bs64-s2026`），wandb `config=vars(args)` 全量存档
9. **checkpoint 分层保存**：`{save_dir}/{数据集}/{实验名}/iter{步数:06d}.pt`，换数据集自动分开
10. **大数据集用 `np.load(..., mmap_mode="r")`** 惰性加载
11. **作业驱动注释**：组件上标 `# Problem N: 实现 XX 模块 (n 分)`，与课程作业直接对应

### 7. 图片与可视化要求

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

**matplotlib 中文支持：**
```python
import matplotlib.pyplot as plt
import matplotlib
matplotlib.rcParams['font.sans-serif'] = ['SimHei', 'Noto Sans CJK SC', 'WenQuanyi Micro Hei', 'DejaVu Sans']
matplotlib.rcParams['axes.unicode_minus'] = False
```

### 8. 前沿论文集成

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

### 9. 批量生成策略

当需要生成大量 Notebook（例如 19 个 CS231n Notebook）时：

1. **按模块分批**：每批 3-6 个 Notebook
2. **生成脚本**：使用 Python 脚本（`gen_batchN.py`）以编程方式创建 .ipynb 文件
3. **每批后审计**：检查文字/公式比例、直觉优先、手算验证
4. **修复后再下一批**：不要累积质量债务
5. **每批后推送**：增量部署

#### Notebook 生成脚本模板

```python
import json
from pathlib import Path

def md_cell(source):
    if isinstance(source, str):
        source = source.split('\n')
        source = [s + '\n' for s in source]
        if source:
            source[-1] = source[-1].rstrip('\n')
    return {"cell_type": "markdown", "metadata": {}, "source": source}

def code_cell(source):
    cell = md_cell(source)
    cell["cell_type"] = "code"
    cell["execution_count"] = None
    cell["outputs"] = []
    return cell

def build_notebook(cells, kernel="python3"):
    return {
        "cells": cells,
        "metadata": {
            "kernelspec": {"display_name": "Python 3", "language": "python", "name": kernel},
            "language_info": {"name": "python", "version": "3.8"}
        },
        "nbformat": 4,
        "nbformat_minor": 5
    }
```

### 10. 预渲染输出

为了让 Notebook 无需运行即可显示结果：

1. 本地执行 Notebook：`jupyter nbconvert --to notebook --execute --inplace`
2. 输出（base64 PNG 图片、文本、HTML）嵌入到 .ipynb JSON 中
3. 前端从 `cell.outputs` 渲染 — 运行时无需内核
4. 使用 `filterDisplayWarnings()` 抑制 matplotlib 字体警告

### 11. 参考模板

以下仓库是本模式的标杆参考：

| 仓库 | 特点 | 课程来源 |
|------|------|----------|
| [self-improving-agent-notebook](https://github.com/walkinglabs/self-improving-agent-notebook) | 17个可运行Notebook，论文研读笔记，零依赖实现 | Stanford CS329A |
| [hands-on-world-models](https://github.com/walkinglabs/hands-on-world-models) | 10章57页，LaTeX图注，arXiv引用 | 原创 |
| [hands-on-modern-rl](https://github.com/walkinglabs/hands-on-modern-rl) | 26章，VitePress，GIF/SVG图，PDF构建 | 原创 |
| [Hands-On-Computer-Vision](https://github.com/ZOUMDA28/Hands-On-Computer-Vision) | 笔记高亮、书签、搜索、橙色主题、完整 Welcome 页 | 华中科技大学内部教程 |

---

## 前端开发指南

### 核心组件

| 组件 | 功能 |
|------|------|
| `Sidebar.jsx` | 左侧边栏导航，搜索、筛选（书签/笔记）、自适应 footer |
| `NotebookViewer.jsx` | Notebook 渲染器，右侧大纲(TOC)、上一篇/下一篇导航、文本高亮、笔记批注、代码复制 |
| `Welcome.jsx` | 首页欢迎页，Hero banner、统计栏、特性条、学习路径、精选 Notebook 卡片网格 |
| `NotesPanel.jsx` | 笔记和书签管理面板，按章节分组，支持导出导入 |
| `SettingsPanel.jsx` | 设置面板（主题切换、字号调整） |
| `ImageLightbox.jsx` | 图片灯箱查看器，点击放大 |
| `ImageLightbox` (hook) | `useImagePreview` 自定义 Hook，管理图片预览状态 |

### 核心 Hooks

| Hook | 功能 |
|------|------|
| `useNotesAndBookmarks.js` | 笔记与书签的增删改查、localStorage 持久化、导入导出 |
| `useSettings.js` | 设置项（主题、字号）的读写与持久化 |
| `useTheme.js` | 主题切换逻辑，系统主题检测 |

### 核心 Context

| Context | 功能 |
|---------|------|
| `SettingsContext.jsx` | 全局设置与主题状态 Provider |

### 核心数据模块

**data/notebooks.js** — 渲染引擎核心：
- 使用 `import.meta.glob('../../../notebooks/**/*.ipynb', { query: '?raw', import: 'default' })` 动态加载
- 嵌套路径正则匹配：`rootDir/([^/]+)/(.+?)\.ipynb$`
- 计算 imageBase 路径用于图片 URL 重写
- `getChapterOrder(dir)` 从目录名前导数字推导排序（零维护，避免 999 事故）
- `getChapterLabel(dir)` 提取章节编号标签（`1`, `A1` 等）
- 实现完整 Markdown 渲染（标题、列表、表格、引用、代码块、图片、链接）
- 实现 Python 代码语法高亮（自定义解析器，非 highlight.js 依赖）
- 实现 Notebook 单元格渲染（markdown cell, code cell, output）
- 支持 KaTeX 数学公式渲染（行内 `$...$` 与块级 `$$...$$`）
- 实现内存缓存（`notebookCache` Map）和空闲预取（`prefetchNotebook`）
- 代码折叠：`CODE_PREVIEW_LINES = 28`，超过则显示"展开全部"按钮
- 输出折叠：超过 28 行输出显示"展开全部输出"按钮
- 代码复制按钮：VS Code 风格红黄绿圆点 + 复制/勾选动画
- 过滤 matplotlib 字体警告：`filterDisplayWarnings()`

**data/sidebar.js** — 首页卡片数据：
- `PATH_STEPS`：学习路径定义（编号、标题、描述、section 标识）
- `RUNNABLE_NOTEBOOKS`：精选 Notebook 卡片数据（id、notebookId、标题、描述、section、时长）
- `notebookId` 必须与 `notebooks.js` 生成的真实 ID 匹配

### config.js（仓库配置与启动按钮）

**config.js** 集中管理 GitHub 仓库信息和外部平台链接。

```javascript
export const GITHUB_OWNER = 'ZOUMDA28'
export const GITHUB_REPO = 'Hands-On-Computer-Vision'
export const GITHUB_BRANCH = 'main'
```

**启动按钮（Launch Links）**：`getNotebookLaunchLinks(meta, notebookId)` 返回一个数组，控制 Notebook 顶部显示的外部平台按钮。

- **保留按钮**：函数返回 `[{ id: 'github', ... }, { id: 'colab', ... }]`
- **去掉按钮**：将函数改为 `return []`，按钮区域不渲染
- **去掉后必须同步文案**：`Welcome.jsx` 中凡提及相关平台的文字都要改掉

### Vite 配置

**vite.config.js** 关键配置：
- 自定义虚拟模块插件 `notebookCatalogPlugin()`
- 虚拟模块 ID：`virtual:notebook-catalog`
- 递归扫描 notebooks 目录，生成 catalog
- 嵌套路径正则：`rel.match(/^([^/]+)\/(.+)\.ipynb$/)`
- ID 生成：`idPath.replace(/\//g, '-')`
- 从 Notebook JSON 提取第一个 `#` 标题作为显示标题
- 开发模式：监听 notebooks 目录变化，新增/修改/删除 .ipynb 时自动热重载
- 构建输出到 `../docs` 目录
- `base: './'` 确保相对路径（GitHub Pages 子路径兼容）
- `server.fs.allow` 允许访问 repo 根目录（读取 notebooks 用）

### Welcome 页面结构

Welcome 页是用户进入网站的第一印象，包含以下区块（从上到下）：

1. **Hero Banner** — 大标题、副标题、CTA 按钮（开始学习 / GitHub / 浏览学习路径）、特性 checklist、代码窗口装饰图
2. **Stats Bar** — 4 个统计卡片（Notebook 数、学习路径数、核心算法数、GitHub Stars）
3. **Features Strip** — 4 个特性图标卡片（可运行 Notebook / 代码级图解 / 循序渐进 / 实践导向）
4. **Learning Path** — 学习路径大卡片（2 个路径，带图标、描述、Notebook 数量）
5. **Runnable Notebooks Grid** — 精选 Notebook 卡片网格（每个卡片：SVG 图标 + 标题 + 描述 + 标签 + 时长）
6. **Footer** — 引用语

**自定义 SVG 图标**：每个 Notebook 卡片配一个专属 SVG 图标，放在 `NOTEBOOK_SVGS` 对象中，用 `nb-1`、`nb-2` 等 id 索引。图标风格：60×60 视口，主题相关的抽象图形，带 opacity 渐变。

### 笔记与高亮功能

这是学习教程网站的核心增强功能，支持读者在阅读时做笔记和高亮。

**核心概念：**

| 概念 | 说明 |
|------|------|
| **Bookmark（书签）** | 收藏整 Notebook，侧边栏可筛选 |
| **Highlight（高亮）** | 选中文本 → 高亮，黄色背景，可删除 |
| **Note（笔记）** | 选中文本 → 添加笔记，蓝色虚线下划线锚点，点击弹出编辑器 |
| **Section（章节）** | 笔记关联的 H2/H3/H4 标题，用于笔记分组 |

**数据结构：**

```javascript
// notes: { [notebookId]: [ { id, sectionId, sectionTitle, quote, text, anchor, updatedAt } ] }
// bookmarks: { [notebookId]: title }
```

**锚定机制（text-offset-v1）：**
- 记录文本块索引 `blockIndex` 和字符偏移 `startOffset` / `endOffset`
- 优先用锚定定位，失败则回退到文本搜索（quote 匹配）
- 高亮和笔记使用不同的 mark 样式：`.user-highlight`（黄底）vs `.note-anchor`（蓝色虚线下划线）

**选择工具栏（Selection Toolbar）：**
- 选中文本后在上方弹出浮动工具栏
- 三个按钮：复制 / 笔记 / 高亮
- 高亮按钮在不可高亮区域（代码、公式、图片等）时禁用

**笔记编辑器（Note Editor Popup）：**
- 弹窗式编辑器，显示引用文本、章节标题、笔记文本域
- 保存 / 取消 / 删除按钮
- 小屏（≤640px）自动切换为 bottom-sheet 模式

**Notes Panel：**
- 独立页面展示所有笔记和书签
- 按 Notebook 分组，显示每篇的笔记数量
- 支持导出为 JSON 和从文件导入
- 支持清空全部数据

### 大纲导航（TOC）

Notebook 右侧显示大纲导航：

- 从渲染后的 HTML 提取 h2/h3/h4 标题生成 TOC
- 滚动监听（scroll spy）高亮当前阅读位置
- 点击 TOC 项平滑滚动到对应标题
- TOC 项有笔记时显示蓝色小圆点提示
- 桌面端固定在右侧，移动端悬浮弹窗
- `toc-toggle` 按钮控制展开/收起

### 预取与缓存机制

为了提升切换 Notebook 的体验：

- **内存缓存**：`notebookCache` Map，已加载过的 Notebook 直接复用
- **空闲预取**：打开当前 Notebook 后，用 `requestIdleCallback` 预取下一篇
- 缓存命中时同步 set state，完全跳过 loading 状态
- 未命中时异步加载，显示 spinner

### 主题自定义

修改 `web/src/styles/index.css` 中的 CSS 变量。

**页面背景色**：改 `--bg-app` 即可，它是 `html, body` 的 `background` 值。

**完整主题覆盖示例（橙色系）** — 在 `index.css` 末尾追加：

```css
/* ==========================================================================
   项目主题覆盖：橙色
   ========================================================================== */

:root {
  /* 背景：暖橙色系 */
  --bg-app: #fff7ed;
  --bg-sidebar: #fffdf9;
  --bg-sidebar-footer: rgba(255, 237, 213, 0.45);
  --bg-input: #fffaf4;
  --bg-hover: rgba(255, 237, 213, 0.7);
  --bg-active: rgba(254, 215, 170, 0.55);

  /* 边框：带一点暖色 */
  --border-subtle: rgba(194, 65, 12, 0.06);
  --border-light: rgba(194, 65, 12, 0.12);
  --border-divider: rgba(194, 65, 12, 0.08);

  /* 主色 */
  --accent: #ea580c;
  --accent-soft: rgba(234, 88, 12, 0.1);

  /* 滚动条 */
  --scrollbar-thumb: #fed7aa;
  --scrollbar-thumb-hover: #fdba74;

  /* 品牌 */
  --brand-gradient-from: rgba(234, 88, 12, 0.11);
  --brand-gradient-to: rgba(251, 146, 60, 0.14);
  --brand-accent: #ea580c;

  /* 代码 / 引用 / 表格 */
  --code-inline-bg: #fff2e4;
  --code-inline-border: rgba(234, 88, 12, 0.1);
  --code-inline-text: #c2410c;
  --blockquote-bg: #fff6ec;
  --blockquote-border: rgba(234, 88, 12, 0.16);
  --table-header-bg: #fffaf3;
  --table-even-row: #fffcf8;
  --toc-hover-bg: rgba(255, 247, 237, 0.8);
}

[data-theme="dark"] {
  --bg-app: #17110c;
  --bg-card: rgba(38, 28, 21, 0.94);
  --bg-sidebar: #1d1610;
  --bg-sidebar-footer: rgba(29, 22, 16, 0.6);
  --bg-input: #251c15;
  --bg-hover: rgba(59, 42, 30, 0.6);
  --bg-active: rgba(80, 56, 36, 0.5);

  --border-subtle: rgba(255, 237, 213, 0.07);
  --border-light: rgba(255, 237, 213, 0.12);
  --border-divider: rgba(255, 237, 213, 0.07);

  --accent: #fb923c;
  --accent-soft: rgba(251, 146, 60, 0.14);

  --scrollbar-thumb: #4a3728;
  --scrollbar-thumb-hover: #6b5140;

  --brand-gradient-from: rgba(251, 146, 60, 0.12);
  --brand-gradient-to: rgba(253, 186, 116, 0.12);
  --brand-accent: #fb923c;

  --code-inline-bg: rgba(194, 65, 12, 0.18);
  --code-inline-border: rgba(251, 146, 60, 0.14);
  --code-inline-text: #fdba74;
  --blockquote-bg: rgba(234, 88, 12, 0.1);
  --blockquote-border: rgba(251, 146, 60, 0.22);
  --table-header-bg: rgba(255, 255, 255, 0.05);
  --table-even-row: rgba(255, 255, 255, 0.02);
  --toc-hover-bg: rgba(255, 255, 255, 0.05);
  --modal-bg: #251c15;
}
```

**组件内字面颜色类映射**（换主题色时必须同步改）：

| 原蓝色系 | 改为橙色系 | 说明 |
|---------|-----------|------|
| `blue-*` | `orange-*` | 主色系列 |
| `cyan-*` / `sky-*` | `amber-*` | 辅助亮色 |
| `purple-*` / `violet-*` | `rose-*` | 对比强调色 |
| `indigo-*` | `orange-*` | 深色强调 |
| `from-blue-*` | `from-orange-*` | 渐变起始色 |
| `to-cyan-*` | `to-amber-*` | 渐变结束色 |
| `text-blue-*` | `text-orange-*` | 文字色 |
| `border-blue-*` | `border-orange-*` | 边框色 |
| `bg-blue-*` | `bg-orange-*` | 背景色 |

**Welcome 页的 SECTION_STYLES**：每个学习路径 section 有独立配色对象，包含 `bg`、`tag`、`iconBg`、`pathBorder`、`gradient`、`soft` 等字段，换主题时全部同步更新。

---

## 前端样式与渲染排错（实战经验，必读）

以下每一条都来自真实项目事故，按「症状 → 根因 → 处理」给出。

### 1. 样式完整性对账（移植或精简 CSS 后必做）

**症状**：页面"看起来不对"——侧栏、右侧大纲、笔记面板、图片灯箱等元素像完全没上样式。

**根因**：组件（Sidebar / NotebookViewer / NotesPanel / Welcome …）是按参考模板的设计系统写的。
一旦 `styles/index.css` 被裁剪成空壳（只剩 `@import "tailwindcss"` 与几个变量），
组件用到的自定义类与 CSS 变量就大面积为空，元素失去全部样式。

**必须跑到 0 缺失**（缺失类 0，缺失变量 0 或都有兜底值）：

```python
import re
from pathlib import Path

REF, OUR = Path('参考模板/web/src'), Path('web/src')

def classes_in_css(p):
    return set(re.findall(r'\.([A-Za-z_][\w-]*)', p.read_text(encoding='utf-8', errors='ignore')))

def used_classes(d):
    used = set()
    for f in d.rglob('*.jsx'):
        txt = f.read_text(encoding='utf-8', errors='ignore')
        for m in re.finditer(r'className=(?:"([^"]*)"|\{`([^`]*)`\})', txt):
            for tok in re.split(r'[\s${}]+', m.group(1) or m.group(2) or ''):
                if re.fullmatch(r'[a-z][\w-]*', tok):
                    used.add(tok)
    return used

our_css, ref_css = classes_in_css(OUR / 'styles/index.css'), classes_in_css(REF / 'styles/index.css')
print('参考有、我们缺:', sorted(c for c in used_classes(OUR) if c in ref_css and c not in our_css))

def defined_vars(p):
    return set(re.findall(r'(--[\w-]+)\s*:', p.read_text(encoding='utf-8', errors='ignore')))

used_vars = set()
for f in OUR.rglob('*.jsx'):
    used_vars |= set(re.findall(r'var\((--[\w-]+)', f.read_text(encoding='utf-8', errors='ignore')))
print('变量缺失:', sorted(v for v in used_vars if v not in defined_vars(OUR / 'styles/index.css')))
```

**结论**：宁可**完整移植**参考模板的 `index.css`（通常 2500+ 行，含设计系统与 KaTeX），
也不要手工裁剪；裁剪后必须用上面的脚本对账。

### 2. KaTeX 样式必须随包内置

不要用 CDN 的 `<link>` 加载 KaTeX CSS——受限网络下公式会**整体失去样式**。
在入口 CSS 里 `@import`，Vite 会把 woff2 字体一并打进产物：

```css
/* web/src/styles/index.css */
@import "tailwindcss";
@import "katex/dist/katex.min.css";
```

校验：构建产物目录里应出现 `KaTeX_*.woff2`，且 `index.html` 中不再有 katex CDN 链接。

### 3. 自定义 Markdown 渲染器的四条硬约束

前端自行实现了 Markdown 渲染（`data/notebooks.js`），它比标准 Markdown 严格：

| 约束 | 说明 | 违反后果 |
|------|------|----------|
| 表格分隔行**至少 3 个短横线** | 判定用 `/^:?-{3,}:?$/` | 整张表格渲染失败 |
| 表格与引用**逐行**解析 | 行内语法不能跨行 | `**加粗**` 跨行断开 → 显示字面 `**` |
| 需显式支持 `\*` `\_` 转义 | 未实现时反斜杠原样输出 | 标题显示成 `CIEL\a\b\*` |
| 图片相对路径按 `BASE_URL + notebooks/<partDir>/<dir>/` 重写 | 不要写 `images/xxx.png` | 插图 404 |

### 4. 页面标题不要重复

`NotebookViewer` 顶部已渲染 `meta.title`（该标题正是从 Notebook 第一个 `# 一级标题` 提取的）。
若正文首个 markdown cell 再渲染同一个 H1，页面会**标题连出两次**。在渲染时剥离首行 H1：

```javascript
let isFirstMarkdown = true
if (cell.cell_type === 'markdown') {
  let source = normalizeSource(cell.source)
  if (isFirstMarkdown) {
    isFirstMarkdown = false
    source = source.replace(/^\s*#\s+[^\n]*(?:\n|$)/, '')   // 去掉与页面标题重复的 H1
  }
  return renderMarkdown(source, imageBase)
}
```

### 5. 首页卡片必须绑定真实 notebook id

两类典型 bug：

1. **prop 名不匹配**：父组件传 `onSelect`，子组件却解构 `onSelectNotebook` → 点击报错
2. **id 拼接错误**：子组件把 `lessonId` 拼成路径，与真实 id 不一致 → 点击无反应

正确做法：卡片数据直接存**真实 notebook id**（`notebookId` 字段），点击时 `onSelect(nb.notebookId)`；
另设一个纯视觉用的 `id`（如 `nb-1`）去索引图标与配色。

### 6. 笔记功能相关排错

| 症状 | 原因 | 处理 |
|------|------|------|
| 选中文字不弹出工具栏 | 鼠标 up 事件在非 notebook 内容区域 | 检查 `notebookContentRef.current.contains(range.commonAncestorContainer)` |
| 高亮后刷新页面消失 | 未保存到 localStorage | 检查 `saveNote` 是否被调用、`useNotesAndBookmarks` 是否正确持久化 |
| 笔记锚点定位偏移 | Notebook 内容变更后 anchor 不再匹配 | 确保有 quote 回退机制（文本搜索定位） |
| 笔记编辑器位置跑出屏幕 | 小屏下 top/left 计算导致溢出 | ≤640px 切换 bottom-sheet 模式（left:16px; right:16px; bottom:16px; width:auto; top:auto） |

---

## 渲染体检：用 Vite SSR 跑真实渲染器

写完 Notebook 后**不要只靠肉眼看网页**。用 Vite 的 SSR 直接加载前端渲染器，
把每篇 Notebook 渲染成 HTML 再机器检查，能提前发现"表格退化成裸文本""公式未渲染""标题重复"。

```javascript
// check_render.mjs —— 用 node 运行
const { createServer } = await import('file:///.../web/node_modules/vite/dist/node/index.js')
const server = await createServer({
  configFile: '<repo>/web/vite.config.js',
  root: '<repo>/web',
  server: { middlewareMode: true },
  appType: 'custom',
  logLevel: 'error',
  optimizeDeps: { noDiscovery: true, include: [] },
})
const mod = await server.ssrLoadModule('/src/data/notebooks.js')
for (const item of mod.getCatalog()) {
  const nb = await mod.getNotebook(item.id)
  const c = (re) => (nb.html.match(re) || []).length
  console.log(item.id, 'table=', c(/<table>/g), 'math=', c(/math-display/g),
              '裸表格行=', c(/<p>\|/g), '裸$$=', c(/\$\$/g))
}
await server.close()
```

判据：`裸表格行 = 0`、`裸$$ = 0`；`<table>` 数量应与源文件中的表格数一致。

---

## Notebook 执行与复现基础设施

**必须随仓库提供**，否则他人与 CI 都无法重跑 Notebook：

| 文件 / 配置 | 作用 |
|------------|------|
| `utils.py` | 中文路径读写（`cv_imread`/`cv_imwrite`）、随机种子（`set_random_seed`）、中文字体（`setup_plot_chinese`）、`show_images`、`compare_results` 等 |
| `requirements.txt` | 依赖清单（SIFT 需 `opencv-contrib-python`） |
| conda 环境注册 ipykernel | 供 nbconvert 调用 |

```bash
<env>/python.exe -m ipykernel install --user --name <env> --display-name "Python (<env>)"
<env>/python.exe -m nbconvert --to notebook --execute \
  --ExecutePreprocessor.kernel_name=<env> --ExecutePreprocessor.timeout=1800 \
  --output exec_tmp.ipynb practice.ipynb
```

**执行流程约定**：先执行到临时文件 `exec_tmp.ipynb`，校验「0 个 error cell、代码格都有输出」
后再覆盖 `practice.ipynb`——执行失败时不会破坏源文件。

---

## 症状 → 原因 → 处理 速查表

| 症状 | 最可能的原因 | 处理 |
|------|--------------|------|
| 元素大面积没样式 | `index.css` 被裁成空壳 | 完整移植设计系统 CSS，跑类名/变量对账脚本 |
| 公式无样式、排版错乱 | KaTeX CSS 走 CDN 且不可达 | 入口 CSS `@import "katex/dist/katex.min.css"` |
| 页面出现裸的 `\| a \| b \|` | 表格分隔行不足 3 个短横线 | 统一改为 `\|:---:\|` |
| 标题显示两次 | 正文 H1 与页面标题重复 | 渲染时剥离正文首个 H1 |
| 侧边栏编号显示 999 | 排序映射表用标题做键、却拿目录名去查 | 改为从目录名前导数字推导 |
| 侧边栏出现重复条目 | 同一主题同时放了 practice 与 practice_extra | 每个学习要点只留一个，拓展进附录 |
| 首页卡片点击无反应 | prop 名不匹配 / lessonId 拼出的路径不存在 | 传 `onSelect` 并用真实 notebook id |
| 正文出现 `**`、`\*` | 转义未支持或强调跨行断开 | 补齐转义、避免跨行强调 |
| 插图 404 | Markdown 写了不存在的 `images/` 前缀 | 与实际图片目录对齐 |
| 部署慢、产物巨大 | 大体量数据集被复制进站点产物 | `rsync --exclude='*.ppm' --exclude='*.3Dpoints'` |
| Notebook 跑不起来 | 缺 `utils.py` / 内核未注册 / 缺依赖 | 补齐基础设施并指定内核执行 |
| 去掉启动按钮后文案不符 | 只改了 `config.js`，未同步组件文案 | 同步修改 `Welcome.jsx` 中的相关文案 |
| 选中文本不弹工具栏 | 鼠标 up 事件在非内容区 / 选中区域含代码 | 检查 `canHighlightTextRange` 逻辑 |
| 笔记刷新后丢失 | 未持久化到 localStorage | 检查 `useNotesAndBookmarks` 初始化逻辑 |
| TOC 滚动不高亮 | scroll spy 未正确绑定 / 选择器不对 | 检查 `updateActiveHeading` 与 `requestAnimationFrame` |
| 代码复制按钮无反应 | copy 事件被拦截 / 降级路径未执行 | 确保 `execCommand('copy')` 降级路径存在 |

---

## 仓库卫生与主题定制

**发布前清理**：删除「只写不读」的生成产物（各章中间结果图），只保留输入图与被 Markdown
引用的图片；删除一次性脚本；`.gitignore` 覆盖 `docs/`、`__pycache__/`、`*.ply`、
`.ipynb_checkpoints/`、`.vscode/`、参考仓库副本目录。

**主题定制（改主色）**需同时改两处，容易漏：

1. **CSS 变量**：在 `index.css` 末尾追加覆盖块（后写覆盖前写），同时覆盖 `:root` 与
   `[data-theme="dark"]`：背景、边框、`--accent`、品牌渐变、引用块、行内代码、表格、滚动条、Modal。
2. **组件里的字面颜色类**：`Welcome.jsx` 这类页面常用 `bg-blue-600`、`from-cyan-500` 等字面颜色，
   它们不随变量变化。整体改橙色时做颜色映射（见上文「主题自定义」节）。

校验：构建产物 CSS 应含新色值；主包 JS 中旧色类计数为 0、新色类计数 > 0。

---

## 嵌套目录支持

Notebook 可以放在任意深度的子目录中。系统会自动：
- 递归扫描所有 .ipynb 文件（Vite 插件 + `import.meta.glob` 双重扫描）
- 使用 `{子目录路径}-{文件名}` 作为唯一 ID（路径分隔符替换为 `-`）
- 从 Notebook 的第一个 markdown 一级标题提取标题
- 重写 Markdown 中的相对图片路径，指向正确的资源位置

例如 `part1-image-processing/01-digital-image-acquisition/practice.ipynb` 的 ID 为 `01-digital-image-acquisition-practice`。

## 图片资源处理

Markdown 单元格中的相对图片路径会被自动重写：
- 原始路径 `![训练曲线](training_curve.png)`
- 重写为 `<img src="./notebooks/part1-foundation/lecture-01-intro/training_curve.png">`
- 绝对 URL 和 data: URI 不受影响

部署时 notebooks 目录随 Vite 构建自动复制到输出目录（`import.meta.glob` 加载的 raw 资源）。

## 章节排序与编号

侧边栏中的章节按正序（1, 2, 3... N）连续排列，不按部分重置。

### 排序机制（推荐：从目录名前导数字推导）

**不要**用「中文标题 → 序号」的映射表当排序键。一旦目录名与标题不一致，
查表会全部落到默认值，侧边栏编号就会集体变成 `999`（真实事故）。

改为从目录名的**前导数字**推导，零维护：

```javascript
// 目录形如 01-digital-image-acquisition / 07-image-stitching
function getChapterOrder(dir) {
  const m = String(dir).match(/^(\d+)/)
  return m ? Number(m[1]) : 999          // 未编号的（如附录）排最后
}

// 侧边栏徽章文字：正文用 1..N，附录用 A1/A2
function getChapterLabel(dir) {
  const m = String(dir).match(/^(A\d+|\d+)/i)
  return m ? m[1].toUpperCase() : ''
}
```

排序逻辑（三级排序）：
1. 按 `PARTS` 顺序（part1 在 part2 之前，附录在最后）
2. 按 `chapterOrder`（从目录名推导，连续编号不按部分重置）
3. 同章内主 Notebook 在拓展 Notebook 之前

### getCatalog 必须传递排序与标签字段

```javascript
export function getCatalog() {
  return NOTEBOOKS.map((entry) => ({
    id: entry.id,
    title: entry.title,
    part,
    partDir: entry.partDir,
    chapterOrder: entry.chapterOrder,
    numLabel: entry.numLabel,        // 侧边栏徽章文字："1" / "A1"
  }))
}
```

### 侧边栏章节号显示

`Sidebar.jsx` 的 `buildSidebarSections` 用 **`numLabel`** 生成徽章：

- 正文章节：`1`, `2`, `3` … `N` —— **不要带 `+` 号**；
- 附录：`A1`, `A2`，单独成组排在最后。

**不要用 `4+` 这类后缀**表示"拓展"。拓展内容一律进附录目录，而不是在主章节里挂一个带后缀的孪生 Notebook。

---

## GitHub Pages 部署

### deploy.yml 关键规则

1. **源必须是 "GitHub Actions"**（不是 "Deploy from a branch"），在仓库 Settings → Pages 中设置
2. 使用 `npm install`（不是 `npm ci` — 避免 package-lock.json 依赖问题）
3. Notebooks 目录必须已存在于仓库中

### 工作流模板

```yaml
name: Deploy to GitHub Pages
on:
  push:
    branches: [main]
  workflow_dispatch:
permissions:
  contents: write
  pages: write
  id-token: write
jobs:
  build:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: web
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm install
      - run: npm run build
      - uses: actions/upload-pages-artifact@v3
        with:
          path: docs
  deploy:
    environment:
      name: github-pages
    runs-on: ubuntu-latest
    needs: build
    steps:
      - uses: actions/deploy-pages@v4
```

### 网络/推送故障排除

| 问题 | 解决方案 |
|------|----------|
| SSH 连接重置 | 切换到 HTTPS：`git remote set-url origin https://github.com/USER/REPO.git` |
| `npm ci` 失败 | 改用 `npm install` |
| Pages 显示原始 README | Settings → Pages → Source = "GitHub Actions" |
| 侧边栏为空（"未找到相关章节"） | 检查 `notebooks/` 目录是否存在于仓库；检查构建后的 JS 中的 `NOTEBOOK_CATALOG` |
| 章节号全为 0 | `getCatalog()` 必须包含 `chapterOrder` 字段 |
| 强制推送被拒绝 | 使用 `--force-with-lease`（比 `--force` 更安全）；需用户确认 |

---

## Notebook 组织规范

教程按部分组织，每个部分一个顶层文件夹：
- `part1-xxx/` - 第一部分
- `part2-yyy/` - 第二部分
- `appendix/` - 附录（拓展主题，排最后）

每个部分内可以有子目录，每个子目录包含：
- `practice.ipynb` - 主练习 Notebook
- 图片资源文件（.png, .jpg, .svg）

确保每个 Notebook 的第一个 markdown cell 是一级标题（# 标题），用于自动提取显示名称。

**约定**：每个学习要点对应**一个** `practice.ipynb`，不设多个重复 Notebook。
与主线关系较远的拓展内容统一收进附录（`appendix/`）。

---

## 增量部署

- 增量推送 Notebook — 不要等全部完成
- 每批之后：`git add` → `git commit` → `git push`
- GitHub Actions 在推送到 main 时自动重建

---

## 最佳实践（共 40 条）

1. **教学契约**: 每篇 Notebook 遵循「直觉 → 手算 → 实现 → 实验」的学习路径
2. **代码可读性**: 保持代码单元格短小，添加充分的中文注释
3. **自包含**: 每篇 Notebook 应该可以独立运行，不依赖前面的状态
4. **图片资源**: 图片放在 Notebook 同目录下，使用相对路径引用
5. **固定种子**: 涉及随机实验时使用固定种子，确保可复现
6. **npm install**: 在 GitHub Actions 中使用 npm install 而非 npm ci
7. **连续章节编号**: 跨部分连续编号；`getCatalog` 必须传递 `chapterOrder` 和 `numLabel`
8. **学术严谨**: 每个知识点必须有论文出处，使用标准引用格式
9. **代码作业**: 每篇 Notebook 必须有可运行代码和作业练习（含 assert 验证）
10. **论文研读**: papers/ 目录记录每节课的论文研读笔记
11. **双层代码策略**: 教学核心层从零实现看清数学本质；实际应用层用现代生态（transformers/trl/peft/accelerate），类型注解 + 混合精度 + checkpoint 最小示例
12. **可视化**: 每个核心概念配 matplotlib 图表或论文配图
13. **多课程交叉对照**: 以斯坦福为主线，对照 MIT/CMU/Berkeley 同类课程改写优化
14. **语言策略**: 模式二默认中文生成，英文版本需用户显式要求；README 同时提供中英文
15. **质量审计**: 每批 Notebook 生成后对照质量清单审计，修复问题后再继续
16. **批量生成**: 按模块分批生成，使用 Python 脚本程序化创建 .ipynb 文件
17. **预渲染输出**: 本地执行 Notebook 并嵌入输出，前端无需内核即可显示结果
18. **增量推送**: 每批完成后立即推送，GitHub Actions 自动部署
19. **参考优先**: 任何网站任务动手前，先读参考项目 `web/` 的样式、组件、配置（强制第一步）
20. **样式完整性对账**: 移植或精简 CSS 后必须跑到「缺失类 0、缺失变量 0」
21. **KaTeX 随包内置**: 入口 CSS `@import "katex/dist/katex.min.css"`，不要走 CDN
22. **渲染器更严格**: 表格分隔行至少 3 个短横线；强调不跨行；补 `\*` `\_` 转义支持
23. **标题去重**: 渲染时剥离正文首个 H1，避免与页面标题重复
24. **编号靠目录名**: 从目录名前导数字推导章节号；用标题做键会集体退化成 999
25. **一个要点一个 Notebook**: 拓展内容进附录，不使用 `+` 后缀标签
26. **首页卡片绑定真实 id**: 数据里存 `notebookId`，点击 `onSelect(nb.notebookId)`
27. **复现三件套**: `utils.py` + `requirements.txt` + 注册 conda 环境的 ipykernel
28. **执行走临时文件**: 先执行到 `exec_tmp.ipynb`，校验无 error 再覆盖源文件
29. **机器验收**: 用 Vite SSR 渲染体检 + 线上 asset 校验，不要只靠肉眼看网页
30. **部署瘦身**: 排除大体量数据集；发布前做仓库卫生清理
31. **断言写关系**: 断言"内部区域严格相等""A < B"，不要写脆弱的手工常数
32. **换主题色改两处**: CSS 变量覆盖 + 组件内字面颜色类映射（如 `blue→orange`）
33. **页面背景色**: 改 `--bg-app` 即可控制整页背景
34. **启动按钮管理**: `config.js` 的 `getNotebookLaunchLinks` 控制按钮；去掉时返回空数组，并同步 Welcome 页文案
35. **笔记与高亮**: 提供文本高亮和笔记功能，localStorage 持久化，支持导入导出
36. **搜索与筛选**: 侧边栏提供搜索框和书签/笔记筛选标签
37. **预取缓存**: 空闲时预取下一篇 Notebook，切换时从缓存秒开
38. **TOC 大纲**: 右侧大纲导航，滚动监听高亮当前位置，有笔记的标题显示提示
39. **代码折叠复制**: 长代码块和长输出可折叠；每个代码格有复制按钮
40. **响应式设计**: 移动端侧边栏遮罩、TOC 悬浮弹窗、笔记编辑器 bottom-sheet 模式
41. **引用必核实**: 论文标题/作者/年份/arXiv ID 四项核实后才写入；无法核实就删除
42. **现代工具链**: 优先 uv 管理环境；requirements.txt 锁定具体版本号
43. **CI 校验**: GitHub Actions 重跑 Notebook，0 error 才允许合入 main
44. **资源开销透明**: Notebook 头部注明运行时长与显存需求，提供 CPU 可跑的小规模版本
45. **结果可信**: 多 seed 取均值±标准差；与论文数字不一致时说明原因
46. **开源合规**: LICENSE 与引用代码兼容；论文配图标注出处；数据集注明许可

---

## 面向科研与实际应用（2026 更新）

### 选题与内容要求

- **贴合当前科研热点**：LLM 预训练与后训练（RLHF/DPO/GRPO）、Agent 与工具调用、多模态、具身智能/世界模型、高效推理（量化/蒸馏）、AI for Science。选题过时（如纯入门 SVM 教程）时应建议用户补充前沿章节。
- **理论—实现—工程三线并重**：科研读者不仅要公式与手写实现，还需要**工程实践视角**——显存/算力估算、训练稳定性技巧、checkpoint 管理、混合精度、分布式数据并行的最小示例。
- **每个实验标注资源开销**：在 Notebook 头部注明预计运行时长、显存需求，并提供**CPU 可跑的小规模版本**作为兜底（Colab 免费额度有限，读者不一定有 GPU）。

### 现代工具链

- **包管理**：优先 `uv`（`uv pip install`、`uv venv`），生成 `requirements.txt` 时锁定具体版本号（`numpy>=1.26` 这种宽松写法在复现时会翻车）。
- **Python 版本**：以 3.10–3.12 为准，避免使用已废弃 API。
- **执行**：`jupyter nbconvert --execute` 仍是标准路径；执行环境与 `requirements.txt` 一致，CI 中可加 `uv pip install -r requirements.txt` 后重跑验证。

### CI 自动化校验（推荐加入仓库）

在 `.github/workflows/` 中除部署外增加校验 job：

```yaml
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with: { python-version: '3.11' }
      - run: pip install -r requirements.txt jupyter nbconvert ipykernel
      - run: |
          # 重跑全部 Notebook，校验 0 error cell（长时 Notebook 可抽样或加 continue-on-error 注记）
          find notebooks -name '*.ipynb' -exec jupyter nbconvert --to notebook --execute --inplace {} \;
```

CI 失败的 Notebook 不允许合入 main——防止"本地能跑、读者跑不起来"的教程上线。

### 开源与合规

- **LICENSE** 必须存在且与引用代码兼容（引用 MIT/Apache 代码需保留原版权声明）。
- **图片版权**：论文配图标注出处且仅用于教学（fair use）；不确定来源的图一律自己用 matplotlib 重绘。
- **数据集**：注明来源、许可与引用格式；不把需要授权的数据集文件提交进仓库。

### 数据与结果可信度

- 图表必须**坐标轴带单位、图例完整**，避免"无刻度装饰图"。
- 数值结论要有不确定性意识：多次运行取均值±标准差（至少 3 seeds），而不是单次结果下结论。
- 与论文报告数字不一致时，明确说明差异原因（数据规模/超参/硬件），不要悄悄改数。

---

## 与 course-notebook-generator 的关系

> **今后所有教程网站任务统一以本技能为唯一入口。**
> `course-notebook-generator` 已标记为**废弃（DEPRECATED）**，仅作历史保留。

本技能是 `course-notebook-generator` 的**超集**。后者聚焦「生成课程 Notebook 并部署」的主干流程；
本技能在主干之上补齐了：

- 两种工作模式（自有 Notebook / 大学课程学术教程）与多课程交叉对照改写；
- 语言策略与 `papers/` 论文研读笔记；
- **前端样式与渲染排错**、**渲染体检（Vite SSR）**、**Notebook 执行与复现基础设施**；
- **笔记高亮与书签功能**、**搜索筛选**、**预取缓存**、**TOC 大纲**、**代码折叠复制**等完整学习体验；
- **Welcome 页完整结构**（Hero / Stats / Features / Learning Path / Notebook Cards Grid）；
- **症状 → 原因 → 处理速查表**、**仓库卫生与主题定制**。

两处内容冲突时以本技能为准，尤其是「章节编号」与「前端样式」两部分。

---

## 参考模板

- [modern-llm-notebook](https://github.com/walkinglabs/modern-llm-notebook) - 前端模板基础
- [self-improving-agent-notebook](https://github.com/walkinglabs/self-improving-agent-notebook) - Stanford CS329A 课程模式
- [hands-on-world-models](https://github.com/walkinglabs/hands-on-world-models) - 学术引用规范
- [hands-on-modern-rl](https://github.com/walkinglabs/hands-on-modern-rl) - 章节结构与可视化
- [Hands-On-Computer-Vision](https://github.com/ZOUMDA28/Hands-On-Computer-Vision) - 笔记高亮、书签、搜索、橙色主题、完整 Welcome 页等增强功能的参考实现
