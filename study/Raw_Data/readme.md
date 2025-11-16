## Raw_Data

It contains the raw data from two experiments from https://osf.io/yns96/. Forty-five subjects participated in Experiment 1 and 51 subjects participated in Experiment 2. A total of four subjects were excluded (three for Experiment 1 and one for Experiment 2) for using a single confidence rating in over 90% of the trials, because such extreme responses make estimates of confidence serial dependence unstable.

## Experimental procedure

In both experiments, subjects completed a two-choice orientation-discrimination task. Each trial began with a 500-ms fixation screen, followed by a Gabor patch presented for 200 ms in the center of the screen. After the stimulus disappeared, subjects were required to indicate the correct Gabor patch orientation (counterclockwise vs. clockwise from vertical). After they made a choice, subjects gave a confidence rating on a 4-point scale where 1 is the lowest and 4 is the highest confidence rating. Both decisions were untimed. The Gabor patches (size=4° of visual angle) were oriented 45° clockwise or counterclockwise relative to vertical, with a spatial frequency of 1.5 cycles per degree. The Gabor patches were presented in two contrast conditions (low 8.1% vs. high 11%). Both Gabor orientations appeared with equal probability throughout the experiment.

Both experiments consisted of a total of 1,000 trials separated into four runs, where each run consisted of five blocks of 50 trials each. Subjects were given 15-s breaks between blocks and unlimited breaks between runs.

The saved raw files have the following columns: Subject,Stimulus,Response,Confidence,Correct,Contrast,Side,RT_Decision,RT_Confidence


## Experiment 1: Keyboard only

In Experiment 1, subjects were instructed to make their perceptual and confidence decisions with either the left or the right hand using a keyboard. Left- and right-hand prompts appeared with equal probability throughout the experiment. The hand condition was randomly determined on each trial with no constraints relative to the previous trials. Perceptual and confidence responses within a trial were always given with the same hand. Whenever the left hand was prompted, responses were given by pressing "Z" for a counterclockwise-oriented Gabor and "X" for a clockwise-oriented Gabor, and confidence ratings were given via "Z," "X," "C," and "V," where "Z" indicated the lowest confidence and "V" indicated to the highest confidence. Similarly, when the right hand was prompted, responses were given by pressing "N" for a counterclockwise-oriented Gabor and "M" for a clockwise-oriented Gabor. Confidence ratings were given via the "N," "M," " < " and " > " keys, where "N" indicated the lowest confidence and " > " indicated the highest confidence.

---

## Data Files

- **exp1.csv**: Experiment 1 data (keyboard with hand manipulation)
- **exp2.csv**: Experiment 2 data (different design)

## Column Descriptions

Each CSV file contains the following columns:

| Column | Type | Values | Description |
|--------|------|--------|-------------|
| **Subject** | String | e.g., "001", "002" | Unique participant identifier |
| **Stimulus** | Numeric | -45, 45 | Gabor orientation in degrees (-45 = CCW, 45 = CW from vertical) |
| **Response** | Numeric/String | Varies | Participant's orientation choice (key press or coded response) |
| **Confidence** | Integer | 1, 2, 3, 4 | Confidence rating (1 = lowest, 4 = highest) |
| **Correct** | Binary | 0, 1 | Response accuracy (0 = incorrect, 1 = correct) |
| **Contrast** | Integer | 1, 2 | Stimulus contrast level (1 = low [8.1%], 2 = high [11%]) |
| **Side** (Exp 1) / **Hand** (Exp 2) | Integer | 1, 2 | Hand used for response (1 = left hand, 2 = right hand) |
| **RT_Decision** | Numeric | Seconds | Response time for orientation decision (in seconds) |
| **RT_Confidence** | Numeric | Seconds | Response time for confidence rating (in seconds) |

## Data Summary

### Experiment 1
- **Total subjects**: 45 (3 excluded)
- **Valid subjects**: 42
- **Raw trials**: 45,000 (45 subjects × 1,000 trials)
- **Excluded trials**: 3,940
- **Valid trials**: 41,060

### Experiment 2
- **Total subjects**: 51 (1 excluded)
- **Valid subjects**: 50
- **Raw trials**: 51,000 (51 subjects × 1,000 trials)
- **Excluded trials**: 3,324
- **Valid trials**: 47,676

## Data Exclusion Criteria

**Subjects excluded**:
- Used a single confidence rating in >90% of trials (makes serial dependence estimates unstable)
  - Experiment 1: 3 subjects excluded
  - Experiment 2: 1 subject excluded

**Trials excluded** (after subject exclusion):
- Trials with RT_Decision ≥ 3 seconds
- Trials with RT_Confidence ≥ 3 seconds

## Data Coding Notes

1. **Stimulus orientation**:
   - -45° = counterclockwise from vertical
   - 45° = clockwise from vertical

2. **Contrast levels**:
   - 1 = Low contrast (8.1% or 0.081)
   - 2 = High contrast (11% or 0.11)

3. **Hand coding**:
   - 1 = Left hand responses
   - 2 = Right hand responses

4. **Response times**:
   - Measured in seconds
   - RT_Decision: from stimulus onset to orientation response
   - RT_Confidence: from confidence prompt to confidence response

5. **Response accuracy**:
   - Calculated based on matching participant's response to correct orientation
   - Binary: 0 (incorrect) or 1 (correct)
