# PsychoPy Experiment

In the `skeleton.psyexp`, it has a trial with fixation, gabor presentation. This serves as a very basic template, telling AI what structure of psyexp file looks like. The loop `conditions.csv` is also arbitaryly defined (not related to the study).


**Task for AI Agents**: Build this experiment by modifying `skeleton.psyexp` (XML file), NOT by writing Python code.


Please do the following tasks:

1. create a csv file as exp_cond.csv, containing experimental conditions (see trial structure below), 
2. At the beginning of the experiment, add an instruction routine explaining the task to participants. Participant presses any key to start.
3. At the end of the experiment, add a thank you routine.
4. Build the experiment and save as exp1.psyexp. 

## Trial Structure

Each trial began with a **500-ms fixation** screen, followed by a **Gabor patch presented for 200 ms** in the center of the screen. After the stimulus disappeared, subjects were required to indicate the correct Gabor patch orientation (counterclockwise vs. clockwise from vertical). After they made a choice, subjects gave a **confidence rating on a 4-point scale** where 1 is the lowest and 4 is the highest confidence rating. **Both decisions were untimed**.

### Stimulus Parameters
- **Size**: 4° of visual angle
- **Orientation**: ±45° from vertical (counterclockwise or clockwise)
- **Spatial Frequency**: 1.5 cycles per degree
- **Contrast**: Two conditions (for simplicity purpose. In the original study, they used adaptive staircases to determine contrast levels)
  - Low: 8.1% (0.081)
  - High: 11% (0.11)
- **Presentation**: Equal probability for both orientations

## Hand Manipulation (Experiment 1)

Subjects were instructed to make their perceptual and confidence decisions with either the **left or right hand** using a keyboard. Left- and right-hand prompts appeared with equal probability throughout the experiment. The hand condition was randomly determined on each trial with no constraints relative to the previous trials. Perceptual and confidence responses within a trial were always given with the same hand.

### Keyboard Mappings

**Left Hand Trials**:
- **Orientation**: "Z" (counterclockwise), "X" (clockwise)
- **Confidence**: "Z" (1-lowest), "X" (2), "C" (3), "V" (4-highest)

**Right Hand Trials**:
- **Orientation**: "N" (counterclockwise), "M" (clockwise)
- **Confidence**: "N" (1-lowest), "M" (2), "," (comma/< key, 3), "." (period/> key, 4-highest)

## Trial Organization

The experiment consisted of a total of **960 trials** organized hierarchically:
- **4 runs** (240 trials each)
  - **5 blocks per run** (48 trials each)
    - **48 trials per block** (randomized)

**Breaks**:
- 15-second countdown between blocks
- Unlimited self-paced breaks between runs

## Data Output

The saved raw files must have the following columns:
```
Subject, Stimulus, Response, Confidence, Correct, Contrast, Side, RT_Decision, RT_Confidence
```

Where:
- **Subject**: Participant ID
- **Stimulus**: Gabor orientation (-45 or 45)
- **Response**: Participant's orientation choice
- **Confidence**: Rating from 1-4
- **Correct**: 1 (correct) or 0 (incorrect)
- **Contrast**: 0.081 (low) or 0.11 (high)
- **Side**: "left" or "right" (hand used)
- **RT_Decision**: Response time for orientation decision (ms)
- **RT_Confidence**: Response time for confidence rating (ms)
