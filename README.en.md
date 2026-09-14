# Learning Tutorial Site

An interactive learning tutorial website generator based on Jupyter Notebooks.

> Built as a TRAE Skill referencing the [modern-llm-notebook](https://github.com/walkinglabs/modern-llm-notebook) template | [中文](./README.md)

## Mandatory First Step: Consult the Reference Site

**Before any website task, open the reference project first and treat it as the format baseline**
(local copy preferred, otherwise GitHub):

| Reference | Location | Purpose |
|-----------|----------|---------|
| modern-llm-notebook | `modern-llm-notebook/web/` | The single baseline for components, design-system CSS, build and deploy config |

Must read: `web/src/styles/index.css` (complete design system), `web/src/components/` (component list),
`web/index.html` (fonts and math loading), `web/vite.config.js`, `.github/workflows/`.

**Never rewrite styles/components from memory, and never trim the reference design system.**
This comes from a real incident: trimming `index.css` into a stub left 93 style classes and
10 CSS variables undefined, and wide areas of the page lost all styling.

## What's New in v2

Adds the full playbook for the class of problems where "the site builds but looks wrong":

- **Mandatory first step**: consult the reference project before touching a website
- **Style completeness audit**: a script that cross-checks classes/variables used by components
  against CSS definitions, requiring zero missing
- **KaTeX bundled locally**: no more CDN-loaded math styles (they vanish on restricted networks)
- **Renderer hard constraints**: table separator needs >= 3 dashes, emphasis must not span lines,
  `\*` escapes must be supported, image path prefixes must match real directories
- **Page title de-duplication**: strip the leading H1 from the first markdown cell
- **Chapter numbering derived from directory names**: the "use chapter titles as sort keys"
  approach is deprecated (it silently degrades every number to 999)
- **One notebook per learning point**: the `4+` extension suffix is deprecated; put extras in an appendix
- **Homepage cards bound to real notebook ids**: fixes broken clicks from prop/id mismatches
- **Render audit via Vite SSR**: render every notebook with the real renderer and check the HTML
- **Reproducibility kit**: `utils.py` + `requirements.txt` + registered ipykernel, plus the
  "execute to a temp file, validate, then overwrite" convention
- **Deploy slimming**: `rsync --exclude` keeps large datasets out of the site artifact
- **Theme changes touch two places**: CSS variables and literal colour classes (e.g. `blue→orange`)
- **Symptom → Cause → Fix cheat sheet**: 11 common failures with targeted fixes

## Language Policy

- **Mode 1**: Follows the language of the user's notebooks
- **Mode 2**: **Defaults to Chinese** for all notebooks and reading notes. English versions are generated only when the user explicitly requests them, producing `notebooks-en/` and `NOTES.en.md` mirrors
- **README**: The project provides both [Chinese](./README.md) and [English](./README.en.md) versions

## Two Working Modes

### Mode 1: Custom Notebook Tutorial
Users provide .ipynb notebooks to generate a browsable web tutorial. Optionally, notebooks can be further improved by cross-referencing similar courses from Stanford, MIT, CMU, and UC Berkeley.

### Mode 2: University Course Academic Tutorial
Users provide a research direction and learning content, and the system automatically generates an academic tutorial website based on university open courses (Stanford first, then MIT/CMU/Berkeley). **Defaults to Chinese**; English requires explicit request.

**Core requirements:**
- Academic rigor: Every knowledge point must cite paper sources, referencing latest arXiv papers
- Code assignments: Every notebook must include runnable code implementations and exercises (with assert validation)
- Visualization: Every core concept includes matplotlib charts or paper figures
- Paper reading notes: `papers/` directory records reading notes for each lecture
- Zero-dependency implementation: Core algorithms implemented from scratch, no wrapper libraries
- Multi-course cross-reference: Stanford as the main line, cross-referencing MIT/CMU/Berkeley for improvement
- **Quality audit**: Each notebook follows a four-step learning path (Intuition→Manual Calc→Implementation→Experiment), with quality checklist audit
- **Batch generation**: Generate by module batches, using Python scripts to programmatically create .ipynb files
- **Pre-rendered outputs**: Execute notebooks locally and embed outputs, no kernel needed at runtime

**Reference benchmarks:**
- [self-improving-agent-notebook](https://github.com/walkinglabs/self-improving-agent-notebook) - Stanford CS329A course model
- [hands-on-world-models](https://github.com/walkinglabs/hands-on-world-models) - Academic citation standards
- [hands-on-modern-rl](https://github.com/walkinglabs/hands-on-modern-rl) - Chapter structure and visualization

## Features

- **Direct .ipynb rendering** - No backend needed, frontend parses notebooks directly
- **Nested directory support** - Notebooks can be placed at any depth
- **Automatic image path rewriting** - Relative paths in Markdown are automatically corrected
- **Sidebar navigation** - Organized by learning paths with sequential chapter numbers
- **Right-side outline** - Quick jump to sections
- **Code syntax highlighting** - Python code intelligent highlighting
- **Full Markdown rendering** - Headings, lists, tables, blockquotes, code blocks
- **Math formula support** - KaTeX renders LaTeX formulas
- **Light/Dark theme** - Theme switching with customizable colors
- **Font size adjustment** - Small/Medium/Large
- **Notes and bookmarks** - Local storage for learning notes
- **Code folding** - Long code blocks collapsed by default
- **Output folding** - Long outputs collapsed by default
- **Image lightbox** - Click to enlarge
- **URL hash routing** - Shareable links
- **Responsive design** - Mobile-friendly
- **Prefetch cache** - Smart prefetching for faster navigation
- **GitHub Actions auto-deploy** - Push to deploy to GitHub Pages (dual deployment strategy)
- **Quality audit checklist** - Each notebook audited against 8 quality standards
- **Batch generation scripts** - Python scripts for programmatic .ipynb creation
- **Pre-rendered outputs** - Executed notebook embedded outputs, no local runtime needed
- **walkinglabs teaching style** - Intuition-first, small-number verification, key observation callouts

## Tech Stack

| Tech | Version | Purpose |
|------|---------|--------|
| React | 19 | Frontend framework |
| Vite | 6 | Build tool |
| Tailwind CSS | 4 | Style framework |
| KaTeX | 0.17 | Math formula rendering |
| Lucide React | 0.546 | Icon library |

## Skill Installation

### Option 1: Clone to workspace

```bash
mkdir -p .trae/skills
cd .trae/skills
git clone https://github.com/ZOUMDA28/learning-tutorial-site.git
```

### Option 2: Manual copy

Copy the `.trae/skills/learning-tutorial-site/` directory to your workspace `.trae/skills/` directory.

## Usage

### Mode 1: Custom Notebooks

In TRAE, the skill is automatically invoked when you say:

- "Create a learning tutorial website"
- "Generate a notebook tutorial website"
- "Make an online course website"
- "Convert ipynb to web pages"

### Mode 2: University Course Academic Tutorial

Mode 2 is activated when you say:

- "Help me create a tutorial website about XXX, referencing Stanford courses"
- "Turn MIT's XXX course into an interactive tutorial"
- "I want an academic tutorial about XXX with paper citations and code assignments"

Mode 2 generates Chinese content by default. For English versions, explicitly request them (e.g., "also generate English versions"), and `notebooks-en/` and `NOTES.en.md` mirrors will be created.

## Project Structure

### Mode 1 Structure

```
tutorial-site/
├── notebooks/                    # Tutorial notebooks
│   ├── part1-image-processing/
│   │   ├── 数字图像的获取和表示/
│   │   │   ├── practice.ipynb
│   │   │   └── lena.jpeg
│   │   └── 几何变换/
│   │       └── practice.ipynb
│   └── part2-optimization-3d/
├── web/                          # React/Vite frontend
├── .github/workflows/
├── README.md                    # Chinese docs
├── README.en.md                 # English docs
└── LICENSE
```

### Mode 2 Structure (University Course)

```
university-course-site/
├── notebooks/               # Chinese tutorial notebooks (default)
│   ├── part1-foundation/
│   │   ├── lecture-01-intro/
│   │   │   ├── practice.ipynb
│   │   │   └── *.png
│   │   └── lecture-02-core/
│   │       └── practice.ipynb
│   ├── part2-advanced/
│   └── part3-frontiers/
├── notebooks-en/            # English mirror (only when requested)
├── papers/
│   ├── lecture-01/
│   │   ├── NOTES.md          # Chinese reading notes (default)
│   │   └── NOTES.en.md       # English reading notes (only when requested)
│   └── lecture-02/
├── web/
├── scripts/
├── .github/workflows/
├── README.md
├── README.en.md
└── LICENSE
```

## Teaching Contract

Every notebook must follow a four-step learning path:

```
Intuitive Understanding → Manual Verification → Code Implementation → Experimental Observation
```

Each notebook must include: course title, introduction, math formulas, runnable code, matplotlib charts, 2-3 exercises (with assert), and a references list.

## Academic Citation Standards

Three citation formats: inline `[[Author, Year]](arXiv link)`, figure captions with source, and a references list at the end.

## Nested Directory Support

Notebooks can be placed at any directory depth. The system automatically scans recursively, generates unique IDs, extracts titles, and rewrites image paths.

## Chapter Sorting

Sidebar chapters are numbered sequentially (1, 2, 3... N) without resetting per part.

## Quick Start

### 1. Initialize project

In TRAE, say "Create a learning tutorial website" and the skill will generate the full project structure.

### 2. Add tutorial content

Place your `.ipynb` files in the corresponding `notebooks/` part directories.

### 3. Start dev server

```bash
cd web
npm install
npm run dev
```

### 4. Deploy to GitHub Pages

Configure `.github/workflows/deploy.yml`, push to main branch for auto-deployment.

## Theme Customization

Modify CSS variables in `web/src/styles/index.css`.

## Best Practices

1. Teaching contract: Intuition → Manual calc → Implementation → Experiment
2. Code readability: Short cells, ample comments
3. Self-contained: Each notebook runs independently
4. Image assets: Same directory, relative paths
5. Fixed seeds: Ensure reproducibility
6. npm install: Use npm install in GitHub Actions
7. Academic rigor: Every knowledge point has paper sources
8. Code assignments: Every notebook has code and exercises
9. Sequential chapter numbering: No reset per part
10. Multi-course cross-reference: Stanford first, then MIT/CMU/Berkeley
11. Language policy: Mode 2 defaults to Chinese, English requires explicit request
12. Quality audit: Audit each batch against the quality checklist
13. Batch generation: Generate by module batches, use Python scripts
14. Pre-rendered outputs: Execute notebooks and embed results
15. Incremental push: Push and deploy immediately after each batch
16. Reference first: read the reference project's styles and components before touching a site
17. Style audit: after porting or trimming CSS, verify zero missing classes and variables
18. Bundle KaTeX locally instead of using a CDN
19. Table separators need at least three dashes; emphasis must not span lines
20. Strip the leading H1 so the page title is not duplicated
21. Derive chapter numbers from directory names; pass `chapterOrder` and `numLabel` from `getCatalog()`
22. One notebook per learning point; extras go to an appendix (no `+` suffix)
23. Bind homepage cards to real notebook ids
24. Reproducibility kit: `utils.py` + `requirements.txt` + registered ipykernel
25. Execute notebooks via "temp file → validate → overwrite"
26. Machine-verify with a Vite SSR render audit instead of eyeballing the page
27. Exclude large datasets from the deploy artifact with `rsync --exclude`
28. Assert relationships (interior regions are exactly equal / A < B), not brittle constants
29. Changing the theme colour means updating both CSS variables and literal colour classes

## Reference Projects

- [modern-llm-notebook](https://github.com/walkinglabs/modern-llm-notebook) - Frontend template base
- [self-improving-agent-notebook](https://github.com/walkinglabs/self-improving-agent-notebook) - Stanford CS329A course model
- [hands-on-world-models](https://github.com/walkinglabs/hands-on-world-models) - Academic citation standards
- [hands-on-modern-rl](https://github.com/walkinglabs/hands-on-modern-rl) - Chapter structure and visualization

## License

MIT License
