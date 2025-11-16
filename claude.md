# LRZ AI Presentation Project

## Project Overview

This project demonstrates AI-assisted psychological research workflow for an LRZ (Leibniz Rechenzentrum) presentation. The demonstration uses the Bocheva & Rahnev (2025) orientation discrimination study as a hands-on example, simulating the complete research cycle from experiment design to manuscript composition.

**Presentation Goal**: Show how AI agents accelerate psychological research by automating experiment development, data analysis, and scientific writing while maintaining rigor and reproducibility.

## Reference Study

**Paper**: Bocheva & Rahnev (2025) - Orientation discrimination with confidence ratings
- **Task**: Two-choice orientation discrimination (45° clockwise/counterclockwise from vertical)
- **Stimuli**: Gabor patches (4° visual angle, 1.5 cycles/degree, 200ms presentation)
- **Contrast**: Two levels (8.1% low, 11% high)
- **Confidence**: 4-point scale (1=lowest, 4=highest)
- **Structure**: 1,000 trials across 4 runs, 5 blocks per run (50 trials/block)
- **Timing**: 500ms fixation, 200ms stimulus, untimed responses
- **Data**: 45 subjects (Exp 1), 51 subjects (Exp 2)

## Demonstration Workflow

For this presentation, we **simulate** developing the study from scratch:

### Stage 1: Experiment Development
**Goal**: Build PsychoPy Builder experiment matching the published paradigm
- **Input**:
  - Study description from psychopy/readme.md
  - Skeleton PsychoPy Builder file (psychopy/skeleton.psyexp)
- **Output**: Complete PsychoPy Builder experiment (.psyexp file)
- **AI Tasks**:
  - Modify the XML-based .psyexp Builder file (not Python code)
  - Configure Gabor stimulus parameters (4°, 1.5 cpd, ±45°, two contrasts)
  - Set precise timing (500ms fixation, 200ms stimulus)
  - Implement sequential response collection (orientation → confidence)
  - Configure hand-specific keyboard mappings (left: Z/X/C/V, right: N/M/</>)
  - Set up trial structure using conditions file (1000 trials, 4 runs × 5 blocks)
  - Configure breaks (15s between blocks)
  - Ensure data logging matches required format

### Stage 2: Data Collection (Simulated)
**Goal**: Demonstrate data structure and collection
- **Input**: PsychoPy experiment
- **Output**: Understanding of data format
- **Note**: Use existing data from study/Raw_Data/ (pretend it's newly collected)
- **Data Format**: Subject, Stimulus, Response, Confidence, Correct, Contrast, Side, RT_Decision, RT_Confidence

### Stage 3: Data Analysis
**Goal**: Analyze serial dependence effects and hand/action effects
- **Input**: Raw data from study/Raw_Data/ (exp1.csv, exp2.csv)
- **Output**: Statistical analyses, figures, tables
- **AI Tasks**:
  - Data cleaning and preprocessing (exclude subjects/trials per criteria)
  - **Primary Analysis**: Serial dependence using linear regression
    - Fit lagged predictors: Response(t-1) → Response(t), Confidence(t-1) → Confidence(t)
    - Compute slopes (b1, c1) per subject for repeat vs. switch conditions
    - Statistical comparisons: one-sample t-tests, paired t-tests, Cohen's d
  - **Secondary Analysis**: Dominant hand effect (previous left vs. right hand)
  - **Supplementary**: Accuracy by contrast, descriptive statistics
  - Visualization: Bar plots with individual data points and connecting lines
  - Statistical reporting in APA format

### Stage 4: Critical Reflection & Review
**Goal**: Professional review of analyses and visualizations with improvement suggestions
- **Input**: Initial analysis code, figures, statistical outputs
- **Output**: Review document (REVIEW.md) with detailed improvement suggestions
- **AI Tasks**:
  - **Statistical Review**:
    - Verify correctness of exclusion criteria application
    - Check regression models and assumptions
    - Validate statistical test choices (paired vs. independent)
    - Review effect size calculations
    - Assess APA formatting of statistics
  - **Visualization Review**:
    - Evaluate figure clarity and readability
    - Check for proper error bars (SEM vs. SD)
    - Assess color choices and accessibility
    - Review axis labels, legends, and annotations
    - Evaluate figure resolution and publication readiness
  - **Code Quality Review**:
    - Check for reproducibility (random seeds, dependencies)
    - Evaluate code organization and modularity
    - Review documentation and comments
    - Identify potential improvements or refactoring
  - **Create REVIEW.md** documenting:
    - Issues identified (categorized by severity: critical, important, minor)
    - Specific suggestions for improvement
    - Questions for clarification
    - Alternative approaches to consider
- **Iterative Improvement**:
  - User manually reviews REVIEW.md and adds feedback/decisions
  - AI implements approved improvements
  - Save revised version as separate files (e.g., analysis_v2.py, figures_v2/)
  - Keep both initial (v1) and revised (v2) versions for comparison

### Stage 5: Manuscript Composition
**Goal**: Draft results/methods sections
- **Input**: Analysis outputs, figures, paper structure
- **Output**: Manuscript sections in academic style
- **AI Tasks**:
  - Methods section: Translate experiment code → narrative description
  - Results section: Transform statistics → flowing narrative
  - Integration of figures and tables
  - APA-style statistical reporting
  - Academic writing refinement


## AI Agent Guidelines

### General Principles
1. **Fidelity to Source**: All experiment parameters, analyses, and reporting must match the published paper
2. **Reproducibility**: Generate well-documented, reproducible code
3. **Academic Rigor**: Follow psychological research standards (APA format, proper statistics)
4. **Pedagogical Clarity**: Code and outputs should be clear for demonstration purposes
5. **Incremental Development**: Build in stages, test frequently

### Experiment Development (PsychoPy Builder)
- **File Type**: Modify XML-based .psyexp Builder file (psychopy/skeleton.psyexp)
- **Not Python Code**: Work with Builder GUI structure in XML format
- Reference psychopy/readme.md for complete experimental specifications
- **Key Modifications Needed**:
  - Gabor stimulus: Change size to 4°, spatial frequency to 1.5 cpd, orientation to ±45°
  - Contrast: Two levels (8.1% low, 11% high) - requires conditions file
  - Timing: Fixation 500ms (already set), stimulus 200ms (already set)
  - Responses: Two sequential keyboard responses (orientation, then confidence)
  - Hand condition: Left hand (Z/X for orientation, Z/X/C/V for confidence) vs. Right hand (N/M for orientation, N/M/</>/ for confidence)
  - Trial loop: Connect to conditions file defining all trial parameters
  - Blocks/runs: Nested loops for 4 runs × 5 blocks × 50 trials structure
  - Breaks: Add 15s countdown between blocks, unlimited between runs
  - Data output: Enable wide CSV format with required columns

### Data Analysis
- Clean, readable code using modern Python stack (pandas, numpy, scipy, matplotlib/seaborn)
- Follow data science best practices: pipelines, functions, clear variable names
- **Main Analyses**:
  1. **Serial Dependence Analysis** (primary focus):
     - **Choice Serial Dependence**: Response(t) = b0 + b1·Response(t-1) + ε
     - **Confidence Serial Dependence**: Confidence(t) = c0 + c1·Confidence(t-1) + ε
     - Compute regression slopes (b1, c1) separately for:
       - **Experiment 1**: Repeat-hand trials vs. Switch-hand trials
       - **Experiment 2**: Repeat-action trials vs. Switch-action trials
     - Statistical tests:
       - One-sample t-tests (b1/c1 vs. 0) for repeat and switch conditions
       - Paired t-tests (repeat vs. switch slopes)
       - Effect sizes (Cohen's d)
  2. **Dominant Hand Effect**:
     - Compare serial dependence slopes for previous left-hand vs. previous right-hand trials
     - Paired t-tests and effect sizes
  3. **Supplementary Analyses**:
     - Accuracy by contrast condition
     - Descriptive statistics (confidence distributions, RTs)
- Generate publication-quality figures visualizing serial dependence effects
- Format all statistics in APA style

### Manuscript Writing
- **Methods**: Active voice, cumulative sentences, clear narrative flow
- **Results**: Story-driven (not stats-driven): "High contrast improved accuracy by 12%" not "significant main effect of contrast"
- Integrate statistics into prose naturally, not as parenthetical dumps
- Use active verbs: "shorten", "drove", "revealed" vs. "was observed", "were found"
- Balance precision with readability
- Follow APA 7th edition formatting

### Quality Checks
**Stage 1 (Experiment)**:
- [ ] Experiment code runs without errors
- [ ] Generated data matches expected format

**Stage 3 (Analysis)**:
- [ ] Analyses produce correct results
- [ ] Figures are publication-ready (labels, legends, sizing)
- [ ] Statistical reporting is complete and accurate
- [ ] All code is well-commented and reproducible

**Stage 4 (Review)**:
- [ ] REVIEW.md comprehensively evaluates all aspects
- [ ] Issues categorized by severity (critical, important, minor)
- [ ] Specific, actionable recommendations provided
- [ ] User decisions documented in REVIEW.md
- [ ] Revised v2 files implement approved changes
- [ ] Both v1 and v2 versions retained for comparison
- [ ] CHANGELOG.md documents all changes and rationale

**Stage 5 (Manuscript)**:
- [ ] Text reads naturally, not like machine translation
- [ ] Statistics properly integrated into narrative
- [ ] APA format followed throughout

## Presentation Context

This is a **demonstration** for researchers at LMU Munich. The audience wants to:
- See practical AI applications in psychology research
- Understand the workflow from idea to publication
- Learn how to use LRZ computing resources with AI tools
- Evaluate whether AI agents can accelerate their research

**Therefore**:
- Prioritize clarity and demonstrability over perfection
- Make design decisions explicit and explainable
- Show both successes and limitations
- Emphasize reproducibility and transparency
- Keep code clean and well-commented

## Key References

- **Paper**: Bocheva & Rahnev (2025) - primary source for all parameters
- **PsychoPy**: https://www.psychopy.org/ - experiment builder
- **APA Style**: 7th edition for statistical reporting
- 
## Success Criteria

1. **Experiment**: PsychoPy Builder file (.psyexp) that accurately implements the paradigm:
   - Opens and runs in PsychoPy Builder without errors
   - All stimulus parameters match paper specifications exactly
   - Produces data in correct format (all required columns)
   - Structure is clear and modifiable by researchers
2. **Analysis**: Reproducible analysis pipeline with clear outputs:
   - Correct statistical methods applied
   - Publication-quality figures generated
   - Results well-documented and interpretable
3. **Review**: Professional quality control demonstrates AI critical thinking:
   - REVIEW.md identifies genuine issues and opportunities
   - Suggestions are specific, actionable, and well-justified
   - Iterative improvement cycle produces measurably better v2
   - Both versions retained showing evolution of analysis
4. **Manuscript**: Draft sections that read like professional academic writing
5. **Demonstration**: Clear, pedagogical workflow showing AI capabilities
6. **Reproducibility**: All steps documented and repeatable

## Notes for AI Agents

- **Experiment Development**: Edit XML in psychopy/skeleton.psyexp (NOT Python code)
  - Reference psychopy/readme.md for complete experimental specifications
  - Understand .psyexp structure: XML with Settings, Routines, Flow
  - Modify component parameters in XML format
  - Create conditions.csv file to define trial types
  - Test by opening in PsychoPy Builder GUI
- **Data Analysis**: Check study/Raw_Data/readme.md for data format
- **Manuscript Writing**: Follow academic refinement principles from global CLAUDE.md
- **Parameter Verification**: Always verify against original paper (bocheva_Rahnev_2025.pdf)
- **Pedagogical Code**: Include clear comments explaining "why" not just "what"
- **Simulation Context**: We're pretending the study doesn't exist yet, but using real data

---

*Last updated: 2025-11-14*
