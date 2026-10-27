# IKEA Gifting: Customer Analytics and RFM Segmentation

_Customer analytics project using SQL, Python, and Power BI_

**Owner:** Pankaj Kumar Meena

## Disclaimer

**IKEA Gifting** is a fictional company name used for my portfolio project. It is not affiliated with, endorsed by, or representative of the real IKEA corporation. All figures are derived from a public, anonymized dataset. IKEA branding in the dashboard is used for design purposes only.

## Project Overview

Customer analytics for a fictional UK-based gifting and home decor e-commerce brand. Raw transactional data is cleaned in SQL, segmented with RFM in Python, and presented in a three-page Power BI dashboard.

## Business Problem

Identify the most valuable customers and reduce churn among customers at risk of leaving.

## Business Questions

- Who are the most valuable customers, and how much revenue do they contribute?
- Which customers have gone quiet and are at risk of churning?
- How do customer value and retention trend by month, quarter, and year?
- How do customer value and churn differ across countries?
- What actions should the business take for each segment?

## Key Findings (from the dashboard)

- Champions: 1,291 customers (about 22% of 5,881) generating $11.8M of $17.4M total revenue (about 68%).
- Overall churn: 50.86% (90-day inactivity threshold).
- Average customer value: 9,169 for Champions vs 429 for Lost.
- Revenue peaks in Q4 ($6.2M).

## Data Source

**Online Retail II**, UCI Machine Learning Repository (donated by Daqing Chen), CC BY 4.0. UK non-store online retailer, Dec 2009 to Dec 2011, 1,067,371 raw rows.
Download: https://archive.ics.uci.edu/dataset/502/online+retail+ii

## Tools

| Tool                     | Purpose                               |
| ------------------------ | ------------------------------------- |
| SQL Server / SSMS        | Import, cleaning, deduplication       |
| Python (pandas), Jupyter | RFM scoring, segmentation, churn flag |
| Power BI Desktop, DAX    | Three-page interactive dashboard      |

## Methodology

1. **Business Requirements Document:** scope, questions, definitions, success criteria.
2. **SQL cleaning:** mixed date formats resolved, 32,907 exact duplicates removed, row counts validated against the UCI total.
3. **RFM segmentation:** Recency, Frequency, Monetary scored 1 to 5; 5,881 customers assigned to six segments (Champions, Loyal, At Risk, New, Needs Attention, Lost); 90-day churn flag.
4. **Power BI:** segmentation merged onto transaction data; three-page report.

## Dashboard

- **Home:** navigation and metric glossary.
- **Business Overview:** customers, revenue, churn %, average customer value; segment and time trends; filters for month, quarter, year, country, segment, churn status.
- **RFM Analysis:** segment performance table, average customer value by segment, churn % over time.

## Sample SQL: Mixed Date Formats

```sql
SELECT
    Invoice, StockCode, Description, Quantity,
    COALESCE(
        TRY_CONVERT(datetime2, InvoiceDate, 120),
        TRY_CONVERT(datetime2, InvoiceDate, 105)
    ) AS InvoiceDate,
    TRY_CONVERT(decimal(10,2), Price) AS Price,
    Customer_ID, Country
INTO dbo.RetailCombined
FROM dbo.[Year 2009-2010];
```

## Sample Python: RFM Scoring

```python
rfm['R_Score'] = pd.qcut(rfm['Recency'], 5, labels=[5, 4, 3, 2, 1]).astype(int)
rfm['F_Score'] = pd.qcut(rfm['Frequency'].rank(method='first'), 5, labels=[1, 2, 3, 4, 5]).astype(int)
rfm['M_Score'] = pd.qcut(rfm['Monetary'], 5, labels=[1, 2, 3, 4, 5]).astype(int)
```

## Credits

- project idea taken from Mansi Goel youtube video.
