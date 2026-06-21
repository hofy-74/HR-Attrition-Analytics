# HR Attrition Intelligence & Power BI Diagnostic Analysis

> A company was losing its best people & not because they were unhappy, and not because they underperformed. They were leaving because they were paid less than the value they delivered.

A full diagnostic analytics project built on the IBM HR Analytics dataset (1,470 employees). Rather than producing a descriptive dashboard, this project follows a hypothesis-driven diagnostic process: every visual exists to test a specific assumption about why employees leave, confirm or reject it against the data, and translate the finding into a dollar-denominated business case.



## Project Structure

```
├── HR.xlsx                              # Source dataset (IBM HR Analytics, 1,470 records, 41 fields)
├── HR_Analytics_Dashboard.pbix          # Power BI dashboard (7 pages, 20+ DAX measures)
├── HR_Analytics_Case_Study.pdf          # Full written case study (methodology + findings)
├── HR_Dashboard_Insights_Report.docx    # Page-by-page insights
├── HR_Story.pptx                        # storytelling  
└── README.md
```

---

## Data Model

Star schema with one fact table and three dimension tables



---

## Methodology

The analysis moved through layers, each one questioning the layer before it:

1. **Started with scale** — 237 of 1,470 employees left in one year (16.1%), roughly one in six
2. **Segmented to locate the problem** — broke attrition down by department, role, and age rather than treating it as one uniform rate
3. **Compared leavers vs. stayers directly** — to identify which factors actually separated the two groups, instead of assuming
4. **Tested a specific hypothesis about promotions** — and let the data overturn it
5. **Quantified the undervalued-talent segment** — using a job-level salary comparison, not a company-wide average
6. **Converted everything into cost** — using the SHRM 75%-of-salary replacement benchmark, applied per department

---

## Key Findings

### 1 — Attrition Is Concentrated, Not Uniform
Sales had the highest departmental attrition (20.63%), but within it, the Sales Representative role alone hit **39.76%** — four in ten employees in that single role left. Employees under 25 left at **39.1%**, nearly four times the rate of the 35–44 age group.

### 2 — Behavioral Factors Outweighed Demographic Ones
| Factor | Attrition Rate |
|---|---|
| Working OverTime | **30.5%** (vs ~10% without) |
| Low manager-relationship satisfaction | 20.6% |
| Low job involvement | **33.73%** (vs 9.03% for high involvement) |
| Frequent business travel | Elevated vs non-travelers |

### 3 — The Promotion Assumption Was Wrong
Employees with 6+ years since their last promotion left at just **16.6%** — *lower* than employees recently promoted, who left at **28.57%**. Acting on the intuitive assumption (accelerate promotions) would have missed the real driver entirely.

### 4 — The Undervalued-Talent Segment
120 high-performing employees were earning below the average for their job level — the clearest, most specific, most solvable finding in the dataset. This wasn't a "company culture" problem; it was a compensation-structure problem with a measurable financial fix.

### 5 — Rate and Cost Are Different Problems
R&D did not have the highest attrition *rate*, but it had the highest attrition *cost* — **$7.5M/year** — because of its size and salary level. A retention strategy built on rate alone would have missed the largest financial exposure in the company.

---

## DAX Highlights

```dax
-- Core attrition rate
Attrition Rate = 
DIVIDE(
    CALCULATE(COUNTROWS('Fact Table'), 'Dim Employee'[Attrition]="Yes"),
    COUNTROWS('Fact Table')
)

-- Undervalued high performer flag (job-level salary comparison)
Below Job Level Avg = 
VAR LevelAvg = 
    CALCULATE(
        AVERAGE('Fact Table'[Monthly Income]),
        ALLEXCEPT('Fact Table', 'Dim Job'[Job Level])
    )
RETURN
    IF('Fact Table'[Monthly Income] < LevelAvg && 'Fact Table'[Performance Rating] = 4, "Underpaid High Performer", "Other")

-- Financial exposure (SHRM 75% benchmark)
Total Attrition Cost = 
SUMX(
    FILTER('Fact Table', 'Dim Employee'[Attrition]="Yes"),
    'Fact Table'[Monthly Income] * 12 * 0.75
)
```

---

## Tools

`Power BI` · `DAX` · `Power Query` · `Star Schema Modeling` · `Statistical Segmentation` · `Executive Reporting`

---

## Data Source & Disclaimer

Dataset: [IBM HR Analytics Employee Attrition & Performance](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset) (public, synthetic).



---

## About This Project

The biggest opportunity wasn't in hiring — it was in retaining the talent already there. Reducing reliance on overtime, improving the first-year experience, and closing the pay gap for high performers were the three levers the data pointed to, each with a measurable path to saving the company millions annually.

