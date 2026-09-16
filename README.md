# Reporter Assay Analysis

A Jupyter notebook that turns a reporter assay spreadsheet (e.g. a luciferase assay) into tidy data, summary statistics, statistical tests and clear plots with every replicate shown.

I originally wrote this to help a biology PhD student analyze transcription factor reporter assays, then generalized it so it works with any spreadsheet in the same layout.

![Overview of reporter activity by condition and dose](figures/overview_bar.png)

## What it does

1. **Loads and cleans** a spreadsheet with a two-row header (condition name + dose), converts values to numbers and reshapes the data to long format
2. **Summarizes** each condition/dose: mean, standard deviation, number of replicates and fold change vs. a baseline condition
3. **Plots**
   - bar chart (mean ± SD) of all conditions with replicate points overlaid
   - boxplot of replicate distributions
   - side-by-side comparison of all conditions at each dose level (low / medium / high), including combinations like `12.5 ng each`
4. **Tests** each condition against the baseline: a one-way ANOVA across all groups, Dunnett's test for the many-against-one comparisons, and an unadjusted Welch's t-test beside it
5. **Exports** figures to `figures/` as PNG, and tidy data, summary and statistics to `results/` as CSV

<p align="center">
  <img src="figures/dose_level_3.png" width="480" alt="Comparison of conditions at the highest dose level">
</p>

## Spreadsheet layout

| Row | Content | Example |
|-----|---------|---------|
| 1 | Condition name, above the first dose column of each condition | `TF-A`, `TF-B`, `TF-A + TF-B`, `GFP control`, `Reporter only` |
| 2 | Dose for each column | `25 ng`, `50 ng`, `100 ng`, `12.5 ng each` |
| 3+ | One row per replicate (blank cells allowed) | `1.80`, `1.84`, `1.76` |

`data/example_reporter_assay.xlsx` contains **synthetic data** generated for demonstration. It is not real experimental data.

## Usage

```bash
pip install -r requirements.txt
jupyter notebook reporter_assay_analysis.ipynb
```

Edit the settings cell at the top:

```python
DATA_FILE = "data/your_assay.xlsx"   # your spreadsheet
BASELINE = "Reporter only"           # reference condition for fold change and statistics
```

Then run all cells. Figures are written to `figures/` and CSVs to `results/`, both regenerated on every run.

> **Note on statistics:** significance comes from Dunnett's test, which compares each condition with the baseline and corrects for making all those comparisons at once. The unadjusted Welch's t-test is reported next to it, and the two can disagree in either direction — Dunnett's pools the spread of every group, so a condition with few replicates borrows confidence from the rest of the plate, while Welch's uses only that group's own spread. With 2–3 replicates per group, treat either p-value as a rough indicator of which effects deserve more replicates.

## Built with

Python · pandas · seaborn · matplotlib · SciPy · Jupyter

## Author

Jose E. Rodriguez Rios
