# AI-Assisted Psychological Research Workflow

This project demonstrates how AI agents can accelerate psychological research, 
from experiment design through data analysis to manuscript composition. 
It uses the Bocheva & Rahnev (2025) orientation discrimination study as a hands-on example.


## Demonstration Workflow

### Stage 1: Experiment Development  (Ready to build)
**Goal**: Build PsychoPy Builder experiment from specifications
- **Input**: `psychopy/readme.md` (specifications), `psychopy/skeleton.psyexp` (template)
- **Task**: Modify XML to implement orientation discrimination task

### Stage 2: Data Collection (https://osf.io/preprints/osf/rzus6_v1)
**Goal**: Understand data structure
- **Note**: Using existing data from `study/Raw_Data/` (pretending it's newly collected)
- **Format**: CSV with 9 columns (Subject, Stimulus, Response, Confidence, Correct, Contrast, Side, RT_Decision, RT_Confidence)

### Stage 3: Data Analysis (To be done)
**Goal**: Analyze serial dependence effects
- **Input**: Raw data CSVs
- **Tasks**:
  - Data preprocessing and cleaning
  - Serial dependence via linear regression (repeat vs. switch)
  - Dominant hand effect analysis
  - Publication-quality figures
  - APA-formatted statistics

### Stage 4: Critical Reflection & Review (To be done)
**Goal**: Professional review of analyses and visualizations
- **Input**: Initial analysis code and figures
- **Tasks**:
  - Statistical correctness review
  - Visualization quality assessment
  - Code quality evaluation
  - Generate REVIEW.md with improvement suggestions
  - User manual review and decisions
  - Implement approved improvements (v2)
  - Keep both v1 and v2 versions

### Stage 5: Manuscript Composition (To be done)
**Goal**: Draft Methods and Results sections
- **Input**: Analysis outputs, figures
- **Tasks**:
  - Methods: Describe experiment (from .psyexp structure)
  - Results: Narrative-driven reporting (story over statistics)
  - Integration: Weave statistics into flowing prose
  - Style: Active voice, cumulative sentences, APA format


## For AI Agents

### Task 1: PsychoPy Builder Development
- **DO**: Edit XML in `skeleton.psyexp` (NOT Python code)
- **Reference**: `psychopy/readme.md` for complete specs
- **Output**: Runnable .psyexp file + conditions.csv

### Task 2: Data Analysis
- **Language**: Python (pandas, scipy, seaborn)
- **Style**: Pipelines, vectorization, clear functions
- **Output**: Figures, statistics, reproducible scripts

### Task 3: Critical Review
- **Role**: Professional research reviewer
- **Output**: REVIEW.md with improvement suggestions
- **Iteration**: User review → AI implements v2

### Task 4: Academic Writing
- **Sections**: Methods, Results
- **Style**: Story-driven, active voice, APA format
- **Integration**: Statistics woven into narrative

## Configuration Files

- **`claude.md`**: Comprehensive instructions for Claude Code (this is the primary file for Claude)
- **`agents.md`**: Task-oriented guide for GitHub Copilot, Cursor, GPT-4, etc.

## Presentation Context

**Audience**: researchers interested in AI for psychological research

**Goal**: Show practical AI applications across the research workflow

**Emphasis**:
- Reproducibility and transparency
- Clean, teachable code
- Academic rigor maintained
- Time savings without quality loss

## Quick Start

1. **Read configuration**: `claude.md` (for Claude) or `agents.md` (for other agents)
2. **Task 1**: Modify `psychopy/skeleton.psyexp` based on `psychopy/readme.md`
3. **Task 2**: Analyze data from `study/Raw_Data/` (serial dependence, figures)
4. **Task 3**: Review analysis and create REVIEW.md with improvement suggestions
5. **Task 4**: Draft manuscript sections (Methods, Results)

