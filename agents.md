# AI Agents Project Instructions
## LRZ AI Presentation - Psychological Research Workflow

> **Purpose**: Instructions for AI coding assistants (GitHub Copilot, Cursor, GPT-4, OpenAI Codex, etc.) working on this demonstration project.

---

## Project Summary

**What**: Demonstrate AI-assisted psychological research workflow using Bocheva & Rahnev (2025) orientation discrimination study
**Why**: Show researchers how AI accelerates research from experiment design → data collection → analysis → manuscript
**How**: Build PsychoPy experiment, analyze existing data, draft manuscript sections

---

## Key Study Parameters

**Bocheva & Rahnev (2025) - Orientation Discrimination with Confidence**

### Stimuli & Task
- **Stimulus**: Gabor patches (sine-wave gratings)
  - Size: 4° visual angle
  - Spatial frequency: 1.5 cycles/degree
  - Orientation: ±45° from vertical (clockwise/counterclockwise)
  - Presentation: 200ms duration, center of screen
  - Contrast: Two levels (8.1% low, 11% high)

- **Task**: Two-interval, two-response
  1. Orientation discrimination (clockwise vs. counterclockwise)
  2. Confidence rating (1-4 scale, 1=lowest, 4=highest)
  3. Both responses untimed

### Trial Structure
- **Timing**:
  - 500ms fixation
  - 200ms stimulus
  - Untimed response (orientation)
  - Untimed response (confidence)

- **Design**:
  - Total: 1,000 trials per subject
  - Structure: 4 runs × 5 blocks × 50 trials
  - Breaks: 15 seconds between blocks, unlimited between runs
  - Balanced: 50% clockwise, 50% counterclockwise

### Data Format
Required columns: `Subject, Stimulus, Response, Confidence, Correct, Contrast, Side, RT_Decision, RT_Confidence`

### Participants & Design
- **Experiment 1**: 45 subjects (3 excluded)
  - **Key Feature**: Hand manipulation (left vs. right hand for responses)
  - Hand randomly cued each trial
  - This is what we implement in PsychoPy Builder
- **Experiment 2**: 51 subjects (1 excluded)
  - Different design (not for this demonstration)
- **Exclusion Criterion**: >90% trials using single confidence rating

---

## Tasks & Guidelines

### Task 1: PsychoPy Builder Experiment Development

**Goal**: Modify PsychoPy Builder XML file to match published paradigm

**IMPORTANT**: Work with XML-based `.psyexp` file, NOT Python code
- **Input File**: `psychopy/skeleton.psyexp` (XML structure)
- **Reference**: `psychopy/readme.md` (complete experimental specifications)
- **Output**: Modified `.psyexp` file ready to run in PsychoPy Builder

**Understanding .psyexp Files**:
- PsychoPy Builder files are XML documents with `<PsychoPy2experiment>` root
- Structure: `<Settings>` + `<Routines>` + `<Flow>`
- Components in routines: GratingComponent, TextComponent, KeyboardComponent, etc.
- Each parameter: `<Param name="..." val="..." valType="..." updates="..."/>`
- Trial loops use TrialHandler with conditions files

**Required Modifications**:

1. **Gabor Stimulus (probe component)**:
   - Size: Set to 4° visual angle `size="(4, 4)"`
   - Spatial frequency: `sf="1.5"` (cycles per degree)
   - Orientation: Use variable from conditions file `ori="$orientation"` (±45)
   - Contrast: Use variable from conditions file `contrast="$contrast_level"` (0.081 or 0.11)
   - Timing: Already set (starts at 0.5s, duration 0.2s) ✓

2. **Fixation Cross**:
   - Duration: Already 500ms (0.5s) ✓
   - Just verify it's correct

3. **Response Collection - Need TWO Sequential Responses**:
   - **First Response (Orientation)**:
     - Hand-specific keys from conditions: `allowedKeys="$orient_keys"`
     - Left hand: 'z' (CCW), 'x' (CW)
     - Right hand: 'n' (CCW), 'm' (CW)
     - Starts after stimulus offset (0.7s)
     - Untimed (no stop duration)

   - **Second Response (Confidence)**:
     - Hand-specific keys from conditions: `allowedKeys="$conf_keys"`
     - Left hand: 'z' (conf=1), 'x' (conf=2), 'c' (conf=3), 'v' (conf=4)
     - Right hand: 'n' (conf=1), 'm' (conf=2), 'comma' (conf=3), 'period' (conf=4)
     - Starts after first response collected
     - Untimed (no stop duration)

4. **Conditions File** (create `conditions.csv`):
   - Columns: `orientation, contrast_level, side, orient_keys, conf_keys, correct_ccw, correct_cw`
   - 4 rows per trial type (2 orientations × 2 contrasts × 2 hands)
   - Example:
     ```
     orientation,contrast_level,side,orient_keys,conf_keys,correct_ccw,correct_cw
     -45,0.081,left,"['z','x']","['z','x','c','v']",z,x
     45,0.081,left,"['z','x']","['z','x','c','v']",z,x
     -45,0.081,right,"['n','m']","['n','m','comma','period']",n,m
     ...
     ```

5. **Trial Structure**:
   - Outer loop (runs): 4 repetitions, sequential
   - Middle loop (blocks): 5 repetitions per run, sequential
   - Inner loop (trials): Use conditions file, 50 trials per block, random order
   - Total: 4 × 5 × 50 = 1000 trials

6. **Breaks**:
   - Between blocks: Add routine with 15-second countdown timer
   - Between runs: Add routine with "Press SPACE to continue" (unlimited)

7. **Data Output**:
   - Enable "Save wide csv file" in Settings
   - Ensure columns match: Subject, Stimulus, Response, Confidence, Correct, Contrast, Side, RT_Decision, RT_Confidence

**XML Editing Tips**:
- Keep XML well-formed (balanced tags, proper nesting)
- Preserve attribute order and formatting
- Use `valType="code"` for variables/expressions (e.g., `$orientation`)
- Use `valType="str"` for literal strings
- Use `valType="num"` for numeric constants
- Set `updates="set every repeat"` for trial-varying parameters

**Testing After Modification**:
- Open in PsychoPy Builder GUI to verify no XML errors
- Check Flow view shows nested loops (runs → blocks → trials)
- Run pilot: Verify timing, stimuli, responses work correctly
- Inspect output CSV: Confirm all required columns present
- Validate stimulus: 4° size, 1.5 cpd, ±45°, correct contrasts

---

### Task 2: Data Analysis

**Goal**: Analyze serial dependence effects in choice and confidence using linear regression

**IMPORTANT**: Focus on serial dependence (lagged regression analysis), not traditional accuracy/confidence analyses

---

## Data Preprocessing

**Input files**: `study/Raw_Data/exp1.csv`, `study/Raw_Data/exp2.csv`

**Exclusion criteria**:
1. **Subject-level**: Exclude subjects who used a single confidence rating in >90% of trials
2. **Trial-level**: Exclude trials with RT_Decision ≥ 3 seconds OR RT_Confidence ≥ 3 seconds

**Expected valid data**:
- Experiment 1: 42 subjects, ~41,060 trials
- Experiment 2: 50 subjects, ~47,676 trials

---

## Primary Analysis: Serial Dependence via Linear Regression

### Analysis 1: Choice Serial Dependence

**Model**: `Response(t) = b0 + b1 · Response(t-1) + ε`

**Procedure**:
1. **Create lagged variable**: For each subject, shift `Response` column by 1 trial to create `Response(t-1)`
2. **Separate trials by hand/action**:
   - **Experiment 1**:
     - Repeat-hand trials: `Side(t) == Side(t-1)` (same hand used on consecutive trials)
     - Switch-hand trials: `Side(t) != Side(t-1)` (different hand used)
   - **Experiment 2**:
     - Repeat-action trials: Similar motor action criterion
     - Switch-action trials: Different motor action
3. **Fit regression per subject**:
   - For repeat trials: Regress Response(t) on Response(t-1) → get slope `b1_repeat`
   - For switch trials: Regress Response(t) on Response(t-1) → get slope `b1_switch`
   - Use `scipy.stats.linregress()`
4. **Statistical tests**:
   - One-sample t-test: `b1_repeat` vs. 0 (test if serial dependence exists in repeat trials)
   - One-sample t-test: `b1_switch` vs. 0 (test if serial dependence exists in switch trials)
   - Paired t-test: `b1_repeat` vs. `b1_switch` (test if repeat > switch)
   - Compute Cohen's d for all comparisons
5. **Report**:
   - Mean β values: `mean(b1_repeat)`, `mean(b1_switch)`
   - t-statistics, p-values, Cohen's d in APA format

### Analysis 2: Confidence Serial Dependence

**Model**: `Confidence(t) = c0 + c1 · Confidence(t-1) + ε`

**Procedure**: Same as choice serial dependence, but using `Confidence` column
1. Create `Confidence(t-1)` lagged variable
2. Separate repeat vs. switch trials (same hand/action criterion)
3. Fit regression per subject → slopes `c1_repeat`, `c1_switch`
4. Statistical tests: one-sample t-tests, paired t-test, Cohen's d
5. Report mean slopes and statistics

### Analysis 3: Dominant Hand Effect

**Goal**: Compare serial dependence strength by previous hand used

**Procedure**:
1. **Separate by previous hand**:
   - Previous left-hand trials: `Side(t-1) == 1`
   - Previous right-hand trials: `Side(t-1) == 2`
2. **Fit regression per subject**:
   - For previous-left trials: Regress Confidence(t) on Confidence(t-1) → `c1_prev_left`
   - For previous-right trials: Regress Confidence(t) on Confidence(t-1) → `c1_prev_right`
3. **Statistical test**:
   - Paired t-test: `c1_prev_left` vs. `c1_prev_right`
   - Cohen's d
4. **Report**: Mean slopes, t-statistic, p-value, Cohen's d

---

## Supplementary Analyses

### Accuracy by Contrast

**Goal**: Verify stimulus manipulation worked (high contrast → higher accuracy)

**Procedure**:
1. Calculate mean accuracy by subject and contrast level
2. Paired t-test: Low contrast vs. High contrast
3. Report: Means, t-statistic, p-value, Cohen's d

### Descriptive Statistics

- Confidence distributions
- Response time summaries (mean, SD)
- Overall accuracy

---

## Visualization

### Figure 1: Serial Dependence - Repeat vs. Switch

**Type**: Bar plot with error bars and individual data points

**Design**:
- X-axis: Two bars (Repeat, Switch)
- Y-axis: Serial dependence strength (β values)
- Error bars: SEM (standard error of the mean)
- Individual points: Scatter plot overlaid on bars (one point per subject)
- Connecting lines: Gray lines connecting each subject's repeat and switch points
- Significance annotation: p-value and significance line above bars

**Code approach**:
```python
import matplotlib.pyplot as plt
import numpy as np

# Calculate means and SEMs
mean_repeat = np.mean(repeat_slopes)
sem_repeat = np.std(repeat_slopes) / np.sqrt(len(repeat_slopes))
mean_switch = np.mean(switch_slopes)
sem_switch = np.std(switch_slopes) / np.sqrt(len(switch_slopes))

# Plot bars with error bars
plt.bar([0, 1], [mean_repeat, mean_switch], yerr=[sem_repeat, sem_switch])

# Overlay individual points
plt.scatter([0]*len(repeat_slopes), repeat_slopes, color='blue', zorder=3)
plt.scatter([1]*len(switch_slopes), switch_slopes, color='red', zorder=3)

# Connect paired points
for i in range(len(repeat_slopes)):
    plt.plot([0, 1], [repeat_slopes[i], switch_slopes[i]], 'gray', alpha=0.5)
```

**Specifications**:
- Figure size: (9, 8)
- Font size: Title 26, axis labels 24-25, ticks 21-22
- Remove top and right spines
- Include p-value annotation

### Figure 2: Dominant Hand Effect - Previous Left vs. Right

**Similar design** to Figure 1, but comparing previous left-hand vs. previous right-hand trials

---

## Code Quality Standards

**Structure**:
- Write modular functions for each analysis:
  - `load_and_clean_data(filename)` → cleaned DataFrame
  - `compute_serial_dependence_slopes(df, variable, condition_column)` → repeat_slopes, switch_slopes
  - `compute_statistics(repeat_slopes, switch_slopes)` → t_stat, p_value, cohen_d
  - `plot_serial_dependence(repeat_slopes, switch_slopes, p_value, title)` → figure

**Style**:
- Use pandas method chaining
- Vectorize operations (avoid explicit loops over rows when possible)
- Type hints for function signatures
- Docstrings explaining what each function does

---

## Statistical Reporting (APA Format)

**Format**:
- Means: *M* = X.XX, *SD* = X.XX
- t-tests: *t*(df) = X.XX, *p* = .XXX (or *p* < .001)
- Cohen's d: *d* = X.XX
- Round to 2 decimal places (3 for p-values)

**Example output**:
```
Serial dependence was stronger for repeat-hand trials (M = 0.33, SD = 0.13)
than switch-hand trials (M = 0.28, SD = 0.14), t(41) = 4.91, p < .001, d = 0.76.
```

---

## Deliverables

1. **Analysis script** (`analysis_serial_dependence.py` or `.ipynb`)
2. **Figures** (PNG and PDF):
   - `fig_serial_dependence_exp1.png/pdf`
   - `fig_serial_dependence_exp2.png/pdf`
   - `fig_dominant_hand_exp1.png/pdf`
   - `fig_dominant_hand_exp2.png/pdf`
3. **Results summary** (text file or markdown with all statistics)

---

### Task 4: Critical Reflection & Review

**Goal**: Act as a professional research reviewer to evaluate analyses and visualizations

**IMPORTANT**: This is a quality control step. Review the initial analysis with fresh eyes as if you were a senior researcher or journal reviewer.

---

## Review Process

### Step 1: Generate Initial Review

**Input**:
- Analysis code (Python script or Jupyter notebook)
- Generated figures (PNG/PDF files)
- Statistical outputs (printed results, summary files)

**Output**: `REVIEW.md` file with structured feedback

### Step 2: User Manual Review

**User action**:
- Read `REVIEW.md`
- Add comments/decisions for each suggestion
- Mark items as: [APPROVED], [DECLINED], or [MODIFIED: new suggestion]

### Step 3: Implement Improvements

**AI action**:
- Read user-annotated `REVIEW.md`
- Implement approved changes
- Save revised versions:
  - `analysis_v1.py` → `analysis_v2.py` (keep both)
  - `figures_v1/` → `figures_v2/` (keep both)
- Document changes in `CHANGELOG.md`

---

## Review Criteria

### 1. Statistical Correctness

**Check**:
- [ ] **Exclusion criteria correctly applied**:
  - Subject exclusion: >90% single confidence rating
  - Trial exclusion: RT_Decision ≥ 3s OR RT_Confidence ≥ 3s
  - Exclusions applied in correct order (subjects first, then trials)
- [ ] **Lagged variables created correctly**:
  - Used `.groupby('Subject').shift(1)` (not global shift)
  - NaN rows dropped after lagging
  - No data leakage across subjects
- [ ] **Repeat vs. switch separation correct**:
  - Exp 1: Side(t) == Side(t-1) for repeat
  - Exp 2: Hand(t) == Hand(t-1) for repeat
  - Previous hand/action properly lagged
- [ ] **Regression assumptions met**:
  - Linear relationship appropriate for data
  - Sufficient data points per subject (>10 trials per condition)
  - Outliers identified and addressed
- [ ] **Statistical test choices correct**:
  - Paired t-tests for repeat vs. switch (within-subject)
  - One-sample t-tests for slopes vs. 0
  - Appropriate alpha level (0.05)
- [ ] **Effect sizes calculated correctly**:
  - Cohen's d formula: (M1 - M2) / SD_pooled for paired tests
  - Cohen's d = M / SD for one-sample tests
- [ ] **Multiple comparisons addressed** (if needed):
  - Bonferroni correction if testing multiple DVs
  - Or justification for not correcting

**Suggestions**:
- Alternative statistical approaches to consider
- Additional robustness checks (e.g., bootstrap)
- Sensitivity analyses

### 2. Visualization Quality

**Check**:
- [ ] **Figure clarity**:
  - Large enough fonts (title ≥24, labels ≥20, ticks ≥18)
  - Clear axis labels with units
  - Appropriate figure size (minimum 6×6 inches)
- [ ] **Error bars**:
  - SEM (not SD) for between-subject comparisons
  - Clearly labeled in figure caption
- [ ] **Data representation**:
  - Individual subject data points visible
  - Connecting lines for paired data
  - Color scheme accessible (colorblind-friendly)
- [ ] **Statistical annotations**:
  - p-values formatted correctly (e.g., p < .001, p = .023)
  - Significance bars positioned clearly
  - Effect sizes included when appropriate
- [ ] **Professional appearance**:
  - Removed unnecessary spines (top, right)
  - Consistent styling across figures
  - High resolution (≥300 DPI for raster formats)
  - Vector formats available (PDF, SVG)
- [ ] **Figure-data correspondence**:
  - Figures accurately represent statistical results
  - No cherry-picking or selective reporting

**Suggestions**:
- Alternative visualization approaches (e.g., violin plots, raincloud plots)
- Additional subplots for supplementary analyses
- Improvements to color scheme or layout

### 3. Code Quality & Reproducibility

**Check**:
- [ ] **Reproducibility**:
  - All random seeds set (if applicable)
  - Package versions documented
  - File paths are relative or documented
  - All required data files listed
- [ ] **Code organization**:
  - Modular functions with clear names
  - Logical flow (import → load → preprocess → analyze → visualize)
  - No code duplication
- [ ] **Documentation**:
  - Docstrings for all functions
  - Comments explaining "why" not just "what"
  - Clear variable names
- [ ] **Error handling**:
  - Checks for missing data
  - Warnings for unusual patterns (e.g., very few trials)
  - Graceful handling of edge cases
- [ ] **Efficiency**:
  - Vectorized operations where possible
  - No unnecessary loops
  - Reasonable computation time

**Suggestions**:
- Refactoring opportunities
- Additional helper functions
- Code simplification

### 4. Reporting & Interpretation

**Check**:
- [ ] **APA format compliance**:
  - Statistical symbols italicized (*M*, *SD*, *t*, *p*, *d*)
  - Correct rounding (2 decimals for stats, 3 for p-values)
  - Degrees of freedom reported correctly
- [ ] **Complete reporting**:
  - Means and SDs for all conditions
  - Test statistics, df, p-values, effect sizes
  - Sample sizes clear
- [ ] **Interpretation**:
  - Results interpreted correctly
  - Conclusions supported by data
  - Limitations acknowledged

**Suggestions**:
- Clearer presentation of results
- Additional descriptive statistics
- Tables to supplement figures


---

## Deliverables

After Task 4 completion:

1. **REVIEW.md**: Comprehensive review document with categorized feedback
2. **User-annotated REVIEW.md**: With decisions marked
3. **analysis_v2.py**: Revised analysis incorporating approved changes
4. **figures_v2/**: Revised figures
5. **CHANGELOG.md**: Document of what changed from v1 to v2 and why
6. **Retained v1 files**: Original versions kept for comparison

---

### Task 5: Manuscript Writing

**Goal**: Draft a complete manuscript from analysis outputs.

**Sub-tasks**:

1.  **Generate Results Report**:
    *   **Input**: `study/analysis/analysis_v2.html` 
    *   **Task**: Generate a markdown report of the results in APA format.
    *   **Content**: Highlight major findings:
        *   Confidence leak strength decreases for switch vs. repeat conditions (for both hand and action).
        *   Confidence leak was weaker when the previous hand was 'left' for both experiments.
    *   **Output**: A markdown file summarizing the key statistical results.

2.  **Draft Methods & Results Section**:
    *   **Input**: The results report from sub-task 1 and `psychopy/readme.md`.
    *   **Task**: Write a combined Methods and Results section.
    *   **Output**: `study/manuscript/method_results.md`

3.  **Draft Introduction Section**:
    *   **Input**: Literature reviews in `study/literature/`.
    *   **Task**: Write a 1500-word introduction with the following structure:
        1.  Start with a broad overview of confidence leak, using a vivid example.
        2.  Provide a brief review of confidence leak literature.
        3.  Identify the research gap: the missing link between motor responses and confidence leak.
        4.  Describe how this study addresses the gap (experimental design and hypothesis).
    *   **Style**: Avoid obvious AI-generated phrases (e.g., "delve", "shape", "dive in"). avoid using em-dashes. Transform passive constructions to active voice as default for energy and clarity. Eliminate heavy nominalization in favor of strong, precise verbs. Vary sentence length strategically to create natural rhythm and flow. Use cumulative sentence structures (subject + verb upfront, modifiers after) for clarity. avoid empty summary sentences like "Building on the literature, we advance ...", "By bridging classic theories ..., this study set this study sets the stage for a more comprehensive understanding...". 
    *   **Output**: `study/manuscript/introduction.md`

4.  **Draft Discussion Section**:
    *   **Task**: Recap the findings and provide an intellectual discussion that connects the current results with previous literature. Highlight the study's contributions.
    *   **Length**: ~1000 words.
    *   **Output**: `study/manuscript/discussion.md`
    *   **Style**: Emphasize active voice and clear, engaging prose. Avoid overused phrases like "shed light on" or "in light of". Avoid empty sentences such as "these findings have important implications". Similar style guidelines as for the Introduction.

5.  **Draft Abstract and Title**:
    *   **Task**: Based on the findings and conclusions, create a 200-word abstract and an attractive title.
    *   **Output**: `study/manuscript/abstract.md`

6.  **Compile Manuscript**:
    *   **Task**: Use `pandoc` to combine the generated markdown files (`introduction.md`, `method_results.md`, `discussion.md`, `abstract.md`) and figures from `study/analysis/figures_v2/` into a single Microsoft Word document.
    *   **Output**: `study/manuscript/manuscript.docx`

**General Writing Guidelines**:
- **Voice**: Prefer active voice.
- **Style**: Tell a story with the results. Lead with findings, not just statistics.
- **APA 7th Edition**:
  - Italicize statistical symbols: *M*, *SD*, *t*, *F*, *p*.
  - Report exact *p*-values (e.g., *p* = .023), or *p* < .001.
  - Round to 2 decimal places (3 for *p*-values).

---

## Quality Checklist

### Experiment (PsychoPy Builder)
- [ ] XML file is well-formed (opens in PsychoPy Builder without errors)
- [ ] Gabor parameters match paper (4°, 1.5 cpd, ±45°)
- [ ] Timing accurate (500ms fixation, 200ms stimulus)
- [ ] Both contrast conditions implemented (8.1%, 11%)
- [ ] Two sequential keyboard responses (orientation → confidence)
- [ ] Hand-specific key mappings (left: Z/X/C/V, right: N/M/</>)
- [ ] Conditions file correctly defines all trial types
- [ ] Nested loop structure correct (4 runs × 5 blocks × 50 trials = 1000)
- [ ] Breaks implemented (15s between blocks, unlimited between runs)
- [ ] Data output columns match specification
- [ ] Experiment runs without errors in PsychoPy

### Analysis
- [ ] Data loads correctly (exp1.csv, exp2.csv)
- [ ] Subject exclusion applied (>90% single confidence rating)
- [ ] Trial exclusion applied (RT ≥ 3 seconds)
- [ ] Valid subject counts match expected (Exp1: 42, Exp2: 50)
- [ ] Lagged variables created correctly (shift by 1 within subject)
- [ ] Repeat vs. switch trials separated correctly
- [ ] Linear regressions fit per subject for both conditions
- [ ] One-sample t-tests performed (slopes vs. 0)
- [ ] Paired t-tests performed (repeat vs. switch)
- [ ] Cohen's d calculated for all comparisons
- [ ] Dominant hand analysis completed (previous left vs. right)
- [ ] Statistics reported in APA format (italics, rounding)
- [ ] Figures include: bars, error bars, individual points, connecting lines
- [ ] Figure specifications met (size, fonts, spines removed)
- [ ] Code is modular with clear function names
- [ ] Code is reproducible and well-commented

### Manuscript
- [ ] All parameters stated explicitly
- [ ] Methods allow replication
- [ ] Results tell coherent story
- [ ] Statistics integrated naturally into prose
- [ ] Active voice predominates
- [ ] APA format correct (italics, spacing, rounding)
- [ ] Figures/tables referenced explicitly
- [ ] No typos or grammatical errors

---

## Common Pitfalls to Avoid

### Experiment (Builder)
- ❌ Editing Python code instead of XML .psyexp file
- ❌ Using incorrect Gabor spatial frequency (must be 1.5 cpd)
- ❌ Wrong size (must be 4° visual angle)
- ❌ Wrong orientation angles (must be ±45° from vertical)
- ❌ Forgetting second response component for confidence
- ❌ Not using hand-specific key mappings
- ❌ Missing or incorrect conditions file structure
- ❌ Wrong loop nesting (must be runs → blocks → trials)
- ❌ Malformed XML (unbalanced tags, wrong valType)

### Analysis
- ❌ Not shifting lagged variables within each subject (must use `.groupby('Subject').shift()`)
- ❌ Forgetting to exclude subjects (>90% single confidence rating)
- ❌ Forgetting to exclude trials (RT ≥ 3 seconds)
- ❌ Wrong column names (Exp1: 'Side', Exp2: 'Hand' for hand/action)
- ❌ Comparing repeat vs. switch with independent t-test (must use paired t-test)
- ❌ Not dropping NaN rows after creating lagged variables
- ❌ Missing Cohen's d effect sizes
- ❌ Incorrect APA formatting (no italics on statistical symbols)
- ❌ Poor figure aesthetics (tiny fonts, no individual points, no connecting lines)
- ❌ Not computing per-subject slopes before group statistics

### Writing
- ❌ Passive voice overuse ("Accuracy was measured")
- ❌ Stats-first narrative ("There was a main effect...")
- ❌ Missing statistical details
- ❌ Over-signposting ("First, second, finally...")
- ❌ Inconsistent formatting (italics, rounding)

---

## AI Assistant Behavior

### Do:
- ✅ Reference psychopy/readme.md for experimental specifications
- ✅ Work with XML .psyexp files (not Python code for experiment)
- ✅ Create well-formed XML with proper structure
- ✅ Write clean, pedagogical code for analysis (this is a demo)
- ✅ Include explanatory comments in analysis code
- ✅ Generate complete, runnable files
- ✅ Follow APA style precisely for manuscript
- ✅ Ask for clarification if parameters unclear
- ✅ Test modifications before suggesting (open in Builder)

### Don't:
- ❌ Write Python code when asked to build PsychoPy experiment (use XML .psyexp)
- ❌ Invent parameters not in psychopy/readme.md
- ❌ Create malformed XML (check tags, valType, nesting)
- ❌ Use deprecated packages or methods in analysis
- ❌ Write cryptic, uncommented code
- ❌ Ignore formatting standards (XML or APA)
- ❌ Assume defaults—state everything explicitly
- ❌ Generate broken or incomplete files

---

## Success Criteria

**This demonstration succeeds when**:
1. **Experiment** (.psyexp file) accurately implements the paradigm:
   - Opens without errors in PsychoPy Builder
   - All stimulus parameters match specifications
   - Produces correct data output format
   - Can be understood and modified by researchers
2. **Analysis** produces clear insights with professional visualizations
3. **Manuscript** reads like published academic work
4. **Code** is clean, reproducible, and teachable
5. **Workflow** demonstrates AI value for psychological research

---

## Quick Reference

| Component | Format | Key Tools | Output |
|-----------|--------|-----------|--------|
| Experiment | XML | PsychoPy Builder | `.psyexp` file, conditions CSV, data CSVs |
| Analysis | Python | pandas, scipy, seaborn | Figures, statistics |
| Manuscript | Markdown/LaTeX | - | Methods, Results sections |

**Study**: Orientation discrimination, Gabor patches, ±45°, two contrasts, confidence ratings
**Data**: 1000 trials/subject, 4 runs × 5 blocks × 50 trials
**Format**: Subject, Stimulus, Response, Confidence, Correct, Contrast, Side, RT_Decision, RT_Confidence

**Main Analysis**:
- **Serial Dependence**: Response(t) = b0 + b1·Response(t-1) + ε, Confidence(t) = c0 + c1·Confidence(t-1) + ε
- **Conditions**: Repeat-hand vs. Switch-hand (Exp1), Repeat-action vs. Switch-action (Exp2)
- **Statistics**: One-sample t-tests (slopes vs. 0), paired t-tests (repeat vs. switch), Cohen's d
- **Dominant Hand**: Previous left-hand vs. previous right-hand trials

---

*Version: 2025-11-14*
*Project: AI Psychological Research Workflow Demo*
