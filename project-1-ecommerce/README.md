# E-Commerce Sales Analytics Dashboard

## 📋 Project Overview

Comprehensive analysis of 50,000+ e-commerce transactions to identify sales patterns, customer behavior, and revenue optimization opportunities. This project demonstrates end-to-end data analysis from data cleaning to actionable business insights.

## 🎯 Business Objectives

1. Identify sales trends and seasonal patterns
2. Analyze product category performance
3. Segment customers based on purchasing behavior
4. Provide data-driven recommendations for revenue growth

## 📊 Key Findings

### Sales Performance
- **Total Revenue:** $520K (2024)
- **Total Orders:** 5,200
- **Average Order Value:** $100
- **Growth Rate:** 23% YoY

### Critical Insights
1. **Seasonal Pattern:** 40% sales increase in Q4 (Oct-Dec)
2. **Product Concentration:** Top 20% products generate 65% of revenue
3. **Customer Segmentation:** 20% high-value customers drive significant revenue
4. **At-Risk Customers:** 15% customers showing signs of disengagement

## 🔧 Technical Implementation

### Data Processing
```python
# Data cleaning and preprocessing
import pandas as pd
import numpy as np

# Load and clean data
df = pd.read_csv('ecommerce_data.csv')
df = df.dropna(subset=['CustomerID', 'InvoiceDate'])
df = df[df['Quantity'] > 0]
df['TotalPrice'] = df['Quantity'] * df['UnitPrice']

# Feature engineering
df['InvoiceDate'] = pd.to_datetime(df['InvoiceDate'])
df['Month'] = df['InvoiceDate'].dt.month
df['Year'] = df['InvoiceDate'].dt.year
```

### SQL Analysis
```sql
-- Monthly sales trend
SELECT 
    DATE_FORMAT(invoice_date, '%Y-%m') AS month,
    COUNT(DISTINCT invoice_no) AS total_orders,
    SUM(quantity * unit_price) AS total_revenue,
    COUNT(DISTINCT customer_id) AS unique_customers
FROM transactions
WHERE invoice_date BETWEEN '2024-01-01' AND '2024-12-31'
GROUP BY month
ORDER BY month;

-- Top products by revenue
SELECT 
    product_category,
    SUM(quantity * unit_price) AS revenue,
    COUNT(DISTINCT invoice_no) AS orders
FROM transactions
GROUP BY product_category
ORDER BY revenue DESC
LIMIT 10;
```

### RFM Customer Segmentation
```python
# Calculate RFM metrics
import datetime as dt

snapshot_date = df['InvoiceDate'].max() + dt.timedelta(days=1)

rfm = df.groupby('CustomerID').agg({
    'InvoiceDate': lambda x: (snapshot_date - x.max()).days,  # Recency
    'InvoiceNo': 'count',  # Frequency
    'TotalPrice': 'sum'  # Monetary
})

rfm.columns = ['Recency', 'Frequency', 'Monetary']

# Assign RFM scores
rfm['R_Score'] = pd.qcut(rfm['Recency'], 4, labels=[4,3,2,1])
rfm['F_Score'] = pd.qcut(rfm['Frequency'], 4, labels=[1,2,3,4])
rfm['M_Score'] = pd.qcut(rfm['Monetary'], 4, labels=[1,2,3,4])

# Create customer segments
rfm['RFM_Segment'] = rfm['R_Score'].astype(str) + rfm['F_Score'].astype(str) + rfm['M_Score'].astype(str)
```

## 📈 Visualizations

The dashboard includes:
1. **KPI Cards:** Revenue, Orders, AOV, Customer Count
2. **Line Chart:** Monthly sales trend showing seasonality
3. **Bar Chart:** Product category performance
4. **Pie Chart:** Customer segmentation (RFM analysis)
5. **Insights Panel:** Key findings and recommendations

## 💡 Business Recommendations

1. **Inventory Management:** Increase stock 30-40% for Q4 based on historical patterns
2. **Product Strategy:** Focus marketing on Electronics & Clothing (65% revenue drivers)
3. **Customer Retention:** Launch VIP program for top 20% high-value customers
4. **Re-engagement Campaign:** Target 15% at-risk customers with personalized offers

## 📦 Technologies Used

- **Python:** Pandas, NumPy, Matplotlib, Seaborn
- **SQL:** MySQL for data extraction and aggregation
- **Visualization:** Recharts, Power BI concepts
- **Statistics:** Descriptive statistics, trend analysis, segmentation

## 🎓 Skills Demonstrated

- Data cleaning and preprocessing
- Exploratory Data Analysis (EDA)
- SQL querying (joins, aggregations, CTEs)
- Statistical analysis
- Customer segmentation (RFM)
- Data visualization
- Business intelligence
- Stakeholder communication

## 📁 Project Files

- `analysis.py` - Python code for data analysis
- `queries.sql` - SQL queries used in the project
- `dashboard.html` - Interactive dashboard
- `README.md` - Project documentation

## 🔗 Live Dashboard

[View Interactive Dashboard](../project-1-ecommerce/dashboard.html)

---

**Dataset Source:** Simulated e-commerce transaction data based on common business patterns

**Analysis Date:** December 2024
