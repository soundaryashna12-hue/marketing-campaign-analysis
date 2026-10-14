Marketing Campaign Customer Analysis

Statistical analysis of a customer marketing dataset in Python (Jupyter Notebook), exploring what drives customer spending and how purchasing behaviour differs across customer groups.


 Dataset

- Source:https://www.kaggle.com/code/suprithahalesh/marketing-campaign
- Size: 2,240 customers in the raw file, 2,213 after cleaning**
- Contents: demographics (birth year, education, marital status, income, children), purchase amounts by product category, purchase counts by channel (deals, web, app, store), web visits and recency.

## Data cleaning

| Step | Detail |
|------|--------|
| Missing values | Removed 24 rows with missing `Income` |
| Impossible values | Removed 3 rows with birth years before 1940 (1893, 1899, 1900) |
| Dates | Parsed `Dt_Customer` as day-month-year |
| Renamed columns | `MntWines` to `MntDrinks`, `NumCatalogPurchases` to `NumAppPurchases` |
| Outlier handling | Income outlier of 666,666 retained; noted as a limitation as it may influence correlation and regression results |

Derived variables

- `Total_spend`: sum of the five product spend columns (drinks, fruits, meat, fish, sweets)
- `Total_children`: `Kidhome` + `Teenhome`
- `Total Purchase`: The sum of customers’ spending on meat, fish, and sweet products, plus the number of purchases made through deals, the web, the app, and physical stores.
- `Age`: 2025 minus `Year_Birth`
- `Total Tenure`: days from `Dt_Customer` to 8 Dec 2025

 Analyses

1. Descriptive statistics (mean, median, spread, skewness, kurtosis)
2. Correlation matrix and scatter plots
3. Paired t-test: web vs store purchases
4. One-way ANOVA: total spend by education level (with Tukey post-hoc test)
5. Chi-square test: education vs spend category (Low / Medium / High)
6. Multiple linear regression: `Total_spend` on `Total_children`, `Recency`, `NumDealsPurchases`, `NumWebPurchases`
7. Regression diagnostics: VIF, residual plots, Q-Q plot, Breusch-Pagan and Durbin-Watson tests

## Key findings


- Channels: Customers make significantly more store purchases (mean about 5.8) than web purchases (mean about 4.1), paired t-test p < 0.001.
- Education: Average spend differs significantly across education levels (ANOVA F about 14.8, p < 0.001). Customers with only a basic education spend far less than the other groups.
- Spending drivers: The regression explains about 45% of the variation in total spend (R² about 0.45). Each additional child is associated with a large drop in spend, while more web purchases are associated with higher spend. Recency and deal purchases are not statistically significant at the 5% level.
- Multicollinearity: All VIF values are well below 5, so collinearity is not a concern.

 Limitations

- Spending and income are right-skewed, and a few extreme values can influence results.
- Regression residuals show signs of non-constant variance, so [robust standard errors were used / this is noted as a limitation].
- The data shows association, not causation.

 Repository structure

```
.
├── analysis.ipynb              # Main notebook
├── marketing_campaign.csv      # Raw data
└── README.md
```


Used Python, pandas, NumPy, SciPy, statsmodels, seaborn, matplotlib
