# SafeRoads US — Data-Driven Road Safety Insights

Advanced statistical analysis of road accidents in the United States, built to turn a large public dataset into reliable, actionable insights for a general (non-technical) audience.

> **Context:** data analyst project for a social-good / NGO scenario. The goal is to find out *what drives accident severity*, *how accidents evolve over time*, and *where prevention could help most*, then share the results as a report and slides.

---

## 1. Dataset

- **Original dataset:** about 7.7 million accidents, 49 States, February 2016 to March 2023.
- **File used in this project:** an extract of **1,410,124 accidents** (February 2016 to September 2022). A **reproducible random sample of 500,000 rows** (`random_state=42`) is used for the statistical tests, after checking that it is representative of the extract (monthly distribution and severity proportions).
- **Severity scale:** 1 (low impact on traffic) to 4 (high impact on traffic).

> The raw CSV is **not** stored in this repository (too large for GitHub). See the reproduction steps below.

---

## 2. Repository structure

```
.
├── README.md
├── requirements.txt
├── .gitignore
├── Dataset/
│   └── monthly_counts_full.csv        # exact monthly accident counts (output of 01_EDA)
├── Notebooks/
│   ├── 01_EDA.ipynb                   # cleaning, coverage check, sampling
│   ├── 02_Statistics.ipynb            # Axis 1 (severity factors) + Axis 2 (weather)
│   ├── 03_TimeSeries.ipynb            # Axis 3 (trend, seasonality, forecast)
│   ├── 04_Infrastructure.ipynb        # Axis 4 (road infrastructure)
│   └── 05_StateDayNight.ipynb         # Axis 5 (State and day/night comparisons)
└── presentation/                      # exported slides (PDF)
```

---

## 3. How to reproduce the analysis

1. **Clone the repository and install the dependencies**
```bash
   git clone https://github.com/heeeba739/Data-Driven-Road-Safety-Insights.git
   cd Data-Driven-Road-Safety-Insights
   pip install -r requirements.txt
```
2. **Download the raw dataset** `US_Accidents.csv` from Kaggle or the dataset website and place it in the `Dataset/` folder.
3. **Set the data folder** in the first configuration cell of each notebook:
```python
   DATA_DIR = Path("../Dataset")   # folder that contains US_Accidents.csv
```
4. **Run the notebooks in this order:**
   1. `01_EDA.ipynb` creates `US_Accidents_Sample500k_Clean.csv` and `monthly_counts_full.csv` in `Dataset/`
   2. `02_Statistics.ipynb`
   3. `03_TimeSeries.ipynb`
   4. `04_Infrastructure.ipynb`
   5. `05_StateDayNight.ipynb`

Notebook 01 must be run first: all the others read the files it produces. Run one notebook at a time and restart the kernel between them, because the raw file is large.

---

## 4. Methodology

**Workflow:** EDA, descriptive statistics, hypothesis tests, modelling, reporting, slides.

Key design choices:

- **Exact counts on the full extract, tests on a sample.** Temporal coverage, duplicates and missing values are computed on all rows. The 500k random sample is used for the tests. Monthly counts for the time series come from the full extract, because counting accidents on a sample would distort the series.
- **Data coverage is checked before any time analysis.** Months with no data and the partial first and last months are detected and excluded automatically.
- **Effect sizes next to p-values.** With hundreds of thousands of rows almost every p-value is tiny, so every test is accompanied by an effect size (Cramér's V, eta squared, odds ratios, R², Mahalanobis distance...).
- **Missing data:** missingness is tested (it depends on the State, so it is not completely random) and weather variables are imputed with the median of each State. `Precipitation(in)` is deliberately not imputed.

### Statistical mechanisms used

| Notebook | Mechanisms |
|---|---|
| 01 EDA | Descriptive statistics, skewness / kurtosis, IQR outlier detection, missingness analysis (MCAR / MAR) with Chi-square and Cramér's V, random sampling and representativeness check |
| 02 Statistics | Chi-square test, Cramér's V, one-way ANOVA, Tukey post-hoc, eta squared, logistic regression, odds ratios, AUC, Pearson, Spearman, partial correlation, multiple linear regression with residual diagnostics, VIF, PCA |
| 03 Time series | Seasonal decomposition, ADF stationarity test, differencing, ACF / PACF, ARIMA (order chosen by AIC), forecast, backtest, Ljung-Box test |
| 04 Infrastructure | Mann-Whitney test with effect size, multivariate logistic regression, MANOVA |
| 05 State / Day-Night | Kruskal-Wallis with epsilon squared, Chi-square with Cramér's V, bootstrap confidence interval, Hotelling's T² with Mahalanobis distance |

More than 25 distinct mechanisms, with a written interpretation after each result.

---

## 5. Key findings

*(To complete once the interpretations are written. One plain-language sentence per finding, with its main number.)*

1. **Severity:** ...
2. **Weather:** ...
3. **Time trend and seasonality:** ...
4. **Infrastructure:** ...
5. **State and time of day:** ...

---

## 6. Limitations

- **Observational data:** the analysis shows associations, not causes (for example, traffic signals are installed in places that are already busy).
- **Extract of the dataset:** the file used covers February 2016 to September 2022 (1.41M accidents), not the full 7.7M-row version.
- **Reporting and collection differences:** the number of recorded accidents depends on how the data were collected, which changed over time and differs between States. Raw counts are therefore not a perfect measure of real accident frequency.
- **Severity is an ordinal 1-4 scale** describing the impact on traffic, not the injuries of the people involved.

---

## 7. Deliverables

- Notebooks `01` to `05` (this repository)
- Written report (PDF or Markdown)
- Slides exported from Canva: `presentation/`
- Planning and documentation: Confluence

---

## 8. Author

Hiba — Data Analyst 

