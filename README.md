
# Revenue & Margin Intelligence

### Financial Performance, Profitability Analysis & Business Intelligence

**Power BI | DAX | SQL | Python | Financial Analytics | Data Modeling**

An end-to-end business intelligence project exploring financial performance, revenue trends, budget variances, and product profitability using Microsoft's AdventureWorksDW sample data.

The project transforms financial and transactional sales records into two interactive Power BI dashboards designed to support business decision-making.

**The objective:** Go beyond reporting what happened by identifying financial performance gaps, understanding their significance, and translating analytical findings into actionable business insights.

---

## Explore the Project

| Resource | Description |
|---|---|
| [Download Power BI Report](Power%20BI/Revenue_Margin_Intelligence.pbix) | Interactive two-page Power BI report |
| [Financial Performance Dashboard](Images/financial_performance.png) | Actual vs. budget analysis |
| [Revenue & Profitability Dashboard](Images/revenue_profitability.png) | Sales trends and product profitability |
| [Prepared Datasets](Data/PowerBI_star) | Financial and sales reporting tables |

**To explore the interactive dashboards:** Download the `.pbix` file and open it in Power BI Desktop. Use the reporting-period, account, and product slicers to investigate the results.

A public browser-based version is not currently available.

---

## Project Overview

Finance and commercial teams need more than financial totals. Effective reporting should explain:

- How actual financial performance compares with expectations.
- Where budget variances are concentrated.
- Whether revenue growth translates into higher profitability.
- Which products generate the greatest financial contribution.
- How business stakeholders can investigate performance independently.

This project addresses these questions through two complementary reporting perspectives.

| Dashboard | Primary Business Question |
|---|---|
| Financial Performance | Where is revenue falling short of budget, and why should management investigate? |
| Revenue & Profitability | How are revenue, gross profit, and margins developing, and which products contribute most? |

The dashboards use separate financial and sales datasets with different reporting periods. Their results are not presented as a single reconciled income statement.

---

# Dashboard 1: Financial Performance Overview

![Financial Performance Overview](Images/financial_performance.png)

### Business Question

**How does revenue compare with budget, and which financial accounts contribute most to the variance?**

The dashboard combines actual and budgeted financial records across matched reporting dates.

Interactive features allow users to filter by reporting period and revenue account while keeping financial KPIs and variance charts synchronized.

### Key Financial Results

| KPI | Result |
|---|---:|
| Actual Revenue | $16,882,125 |
| Budgeted Revenue | $18,658,200 |
| Revenue Variance | -$1,776,075 |
| Revenue Variance % | -9.52% |

*Scope: USD, selected revenue accounts, and matched financial reporting dates in 2010–2011.*

### Key Findings

**1. Revenue performed below budget.**

Actual revenue was approximately **9.52% below budget**, representing a shortfall of approximately **$1.78 million** within the selected reporting scope.

**2. The revenue gap was highly concentrated.**

Trade Sales contributed approximately **$1.72 million** to the shortfall, representing roughly **97% of the total revenue gap**.

Intercompany Sales contributed approximately $0.06 million.

**3. Management attention should prioritize the largest variance drivers.**

Rather than treating the shortfall as equally distributed across all revenue accounts, the analysis identifies Trade Sales as the primary area requiring further investigation.

### Business Implication

A finance team reviewing these results could prioritize understanding the Trade Sales variance before designing corrective actions.

Potential follow-up investigations could examine sales volume, pricing, customer demand, or changes in business conditions.

**Important:** These are proposed investigation areas, not proven causes of the shortfall.

---

# Dashboard 2: Revenue & Profitability Overview

![Revenue & Profitability Overview](Images/revenue_profitability.png)

### Business Question

**How are sales and profitability evolving, and which products contribute most to gross profit?**

This dashboard analyzes transactional sales data through four financial KPIs, monthly performance trends, and an interactive product profitability matrix.

### Key Financial Results

| KPI | Result |
|---|---:|
| Sales Revenue | 29,312,982.50 |
| Product Cost | 17,257,651.79 |
| Gross Profit | 12,055,330.71 |
| Gross Margin | 41.13% |

*Scope: Selected single-currency sales records, 2011–2013. Figures are shown in the underlying reporting currency.*

### Key Findings

**1. Revenue and gross profit increased substantially during 2013.**

The monthly trend analysis shows higher reported sales and gross profit toward the end of 2013 than in earlier reporting periods.

**2. Profitability did not move identically to revenue.**

Monthly gross margin fluctuated around 40–42%, demonstrating why revenue growth alone is insufficient to evaluate commercial performance.

**3. Product-level profitability provides a different perspective from sales rankings.**

The product matrix identifies individual products generating substantial gross profit and allows users to compare their profit contributions and margin percentages.

For example, **Mountain-200 Black, 46** generated approximately:

| Metric | Result |
|---|---:|
| Sales Revenue | 1,373,469.55 |
| Gross Profit | 626,621.57 |
| Gross Margin | 45.62% |

Its gross margin was approximately **4.5 percentage points above** the overall reported margin of 41.13%.

### Business Implication

Products with high revenue do not necessarily have the highest profitability rates.

Analyzing both gross profit and margin can help stakeholders prioritize product-level investigations, evaluate commercial performance, and understand differences in financial contribution.

Changes in pricing, sales mix, and costs would require further analysis before attributing specific causes to the observed margin patterns.

---

# Technical Implementation

## 1. Data Preparation and Modeling

The project uses financial and transactional data derived from Microsoft's AdventureWorksDW sample database.

The reporting model includes two separate fact tables:

**Financial Reporting**

`fact_finance_paired`

Contains matched actual and budget observations with:

- Reporting dates
- Organizational and departmental identifiers
- Financial account identifiers
- Currency identifiers
- Actual financial amounts
- Budgeted financial amounts
- Signed variance amounts

**Sales Reporting**

`fact_sales`

Contains transactional sales information used for:

- Sales revenue analysis
- Product cost analysis
- Gross profit calculations
- Product-level profitability
- Monthly financial trends

Supporting dimensions include:

- `dim_date`
- `dim_currency`
- `dim_account`
- `dim_organization`
- `dim_department`
- `dim_product`
- `dim_territory`

The Power BI model uses many-to-one relationships, with dimension tables filtering the relevant fact tables.

Financial and sales fact tables are deliberately kept separate because they represent different analytical scopes.

## 2. Financial Measures Using DAX

Reusable DAX measures were developed to ensure consistency across KPI cards, charts, and interactive selections.

### Financial Performance

```dax
Total Actual =
SUM('fact_finance_paired'[SignedActualAmount])

Total Budget =
SUM('fact_finance_paired'[SignedBudgetAmount])

Budget Variance =
[Total Actual] - [Total Budget]

Budget Variance % =
DIVIDE([Budget Variance], [Total Budget])

Revenue Budget Gap =
[Total Budget] - [Total Actual]
```

### Revenue & Profitability

```dax
Sales Revenue =
SUM('fact_sales'[SalesAmount])

Product Cost =
SUM('fact_sales'[TotalProductCost])

Gross Profit =
[Sales Revenue] - [Product Cost]

Gross Margin % =
DIVIDE([Gross Profit], [Sales Revenue])
```

The gross margin calculation divides aggregated gross profit by aggregated revenue rather than averaging individual product margins.

This ensures that the reported total correctly reflects the weighted financial result.

## 3. Interactive Reporting

The dashboards provide:

- Dynamic financial KPI cards
- Reporting-period dropdown slicers
- Revenue account and product filtering
- Actual-versus-budget comparisons
- Financial variance analysis
- Monthly revenue and gross profit trends
- Gross margin trend analysis
- Product-level profitability matrices
- Conditional formatting for relative margin comparisons

Users can explore different periods, accounts, and products while the relevant financial measures update automatically.

---

# Data Quality and Analytical Validation

Financial BI requires more than correct-looking charts. Reliable results depend on consistent financial definitions, appropriate filtering, and clear reporting boundaries.

The project addresses several analytical challenges.

### Matched Financial Reporting Dates

The financial dashboard uses dates for which matching actual and budget observations are available.

It does not treat absent budgets as zero or present its results as complete annual financial statements.

### Currency Consistency

The financial dashboard is restricted to USD.

The sales dashboard uses a single-currency reporting scope.

Amounts in different currencies are not aggregated into misleading combined totals.

### Financial Sign Conventions

The financial dashboard uses the prepared signed actual and budget measures to maintain a consistent reporting convention.

### Reconciled KPI Calculations

Dashboard calculations were checked for consistency between:

- Actual, budget, and variance
- Revenue, product cost, and gross profit
- Gross profit and gross margin percentage

### Reporting-Period Coverage

The sales dashboard currently presents historical activity through December 2013.

January 2014 was excluded from the displayed analysis because its available reported revenue was dramatically below preceding months. Final confirmation of transaction-period completeness remains outstanding.

This avoids presenting an unverified partial-period effect as a genuine collapse in commercial performance.

### Remaining Reproducibility Work

The repository currently contains the Power BI report and prepared reporting tables.

The upstream preparation scripts and detailed automated validation outputs are not yet included in this published version. Adding those materials is a planned improvement to technical reproducibility.

---

# Tools and Skills Demonstrated

| Area | Skills |
|---|---|
| Business Intelligence | Power BI dashboards, KPI development, interactive reporting |
| Financial Analytics | Budget variance, revenue, gross profit, margin analysis |
| Data Modeling | Relational schemas, fact and dimension tables, filter relationships |
| DAX | Measures, ratios, financial calculations, filter context |
| Data Preparation | Structured datasets, data cleaning, reporting transformations |
| Analytical Validation | Calculation reconciliation, currency consistency, reporting-scope checks |
| Business Communication | Interpreting findings, identifying investigation priorities, documenting limitations |
| Supporting Analytics | SQL and Python-based data preparation workflows |

---

# Repository Structure

```text
revenue-margin-intelligence/
|
|-- README.md
|
|-- Images/
|   |-- financial_performance.png
|   |-- revenue_profitability.png
|
|-- Power BI/
|   |-- Revenue_Margin_Intelligence.pbix
|
|-- Data/
    |-- PowerBI_star/
        |-- fact_finance_paired.csv
        |-- fact_sales.csv
        |-- dim_account.csv
        |-- dim_currency.csv
        |-- dim_date.csv
        |-- dim_department.csv
        |-- dim_organization.csv
        |-- dim_product.csv
        |-- dim_territory.csv
```

## Running the Project

1. Clone or download this repository.
2. Install Microsoft Power BI Desktop.
3. Open `Power BI/Revenue_Margin_Intelligence.pbix`.
4. If prompted to reconnect data sources, update the CSV file paths to the local `Data/PowerBI_star` folder.
5. Navigate between the two report pages.
6. Use the slicers and product matrix to explore the analytical results.

The dashboard screenshots provide a static preview without requiring Power BI Desktop.

---

# Project Limitations

This project uses **Microsoft AdventureWorks sample data** and does not represent a live business engagement.

Important limitations include:

- Financial actual/budget comparisons cover selected matched dates, not complete financial years.
- The finance and sales datasets have different reporting periods and are not directly reconciled to each other.
- January 2014 is excluded from the sales analysis pending final transaction-coverage verification.
- The reported gross profit measure excludes costs beyond recorded product costs.
- The analysis identifies patterns and possible investigation priorities, not verified causal explanations.
- No commercial savings, realized profit improvements, or operational outcomes are claimed.

The purpose is to demonstrate analytical methodology, financial BI skills, and business-oriented interpretation.

---

# Key Takeaways

This project demonstrates the ability to:

1. Translate financial business questions into measurable KPIs.
2. Structure relational data into usable BI reporting models.
3. Develop dynamic Power BI dashboards using DAX.
4. Investigate budget variances and commercial profitability.
5. Identify financially significant patterns and exceptions.
6. Apply data-quality controls and recognize reporting limitations.
7. Communicate findings in a form useful to business stakeholders.

**The central principle of the project is that financial reporting should not stop at showing performance. It should help stakeholders understand where to investigate and what decisions the data can support.**

---

# Data Source

Microsoft AdventureWorks Sample Databases

https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure

---

# Author

**Kyle Haustein**

MSc Chemistry | Data Analytics | AI Evaluation & Quality Operations

[Portfolio](https://orgo4ever.github.io/) | [GitHub](https://github.com/Orgo4ever) | [LinkedIn](https://www.linkedin.com/in/kyle-haustein-620a38126/)

