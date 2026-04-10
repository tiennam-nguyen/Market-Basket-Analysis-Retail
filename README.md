# Market Basket Analysis: Online Retail Recommendations

## 📌 Project Overview
This project performs Market Basket Analysis on a UK-based non-store online retail dataset to discover hidden patterns in customer purchasing behavior. By leveraging the **FP-Growth algorithm**, the project extracts strong association rules to power a product recommendation engine (e.g., "Customers who bought X also bought Y"). 

The primary notebook for this analysis is `online-retail-transaction.ipynb`.

## 📊 Dataset Information
**Dataset Source:** [Online Retail Dataset - UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/352/online+retail)
**Original Creator:** Dr. Daqing Chen (2015)

This dataset contains transactional data with the following attributes:
* **InvoiceNo:** Invoice number. A 6-digit integral number. If it starts with 'C', it indicates a cancellation.
* **StockCode:** Product (item) code. A 5-digit integral number uniquely assigned to each distinct product.
* **Description:** Product (item) name.
* **Quantity:** The quantities of each product (item) per transaction.
* **InvoiceDate:** The day and time when each transaction was generated.
* **UnitPrice:** Product price per unit in sterling (£).
* **CustomerID:** A 5-digit integral number uniquely assigned to each customer.
* **Country:** The name of the country where each customer resides.

## 🛠️ Methodology & Data Cleansing
Real-world retail data is incredibly messy. This project implements a rigorous data preprocessing pipeline to ensure the resulting recommendation rules are mathematically sound and business-relevant:

1. **Handling Returns:** Filtered out invoices starting with 'C' and transactions where `Quantity <= 0`.
2. **Noise Reduction:** Removed non-physical "administrative" items (e.g., "POST" for postage, "M" for manual entry) using regex matching on `StockCode`.
3. **Entity Resolution:** Grouped basket items strictly by `StockCode` rather than text `Description` to prevent support fracturing, then mapped back to standardized names.
4. **Geographical Scoping:** Filtered the dataset to only include transactions from the `"United Kingdom"` to isolate domestic retail behavior.

## 🧠 Modeling & Benchmark
To overcome the severe memory and processing bottlenecks associated with traditional algorithms, this project utilizes **FP-Growth**. 
* **Algorithm Benchmark:** Experimental results demonstrated that FP-Growth significantly outperforms Apriori in execution time and memory efficiency when dealing with low support thresholds (e.g., < 0.5%), completely avoiding the candidate explosion problem.
* **Parameter Tuning:** The model isolates frequent itemsets with a minimum support of **2%** (capturing high-value niche markets) and filters for strong rules with a minimum confidence of **40%** and a positive correlation (`Lift > 1`).

## 📈 Visualizations
The project goes beyond raw data tables by providing actionable visual insights:
* **Scatter Plot Analysis:** Visualizing the distribution of Support, Confidence, and Lift to identify the "golden" product pairs.
* **Network Graph topology:** Mapping product communities (e.g., the "Regency Teacup" cross-selling network vs. the "Jumbo Bag" hub-and-spoke structure) to guide product bundling strategies.

## 🚀 Results & Business Implications
The script outputs highly actionable, business-ready insights rather than raw data. 

**Example of a High-Value Multi-Item Rule:**
> **IF a customer buys:** [PINK REGENCY TEACUP AND SAUCER, ROSES REGENCY TEACUP AND SAUCER]  
> **Then the best item to recommend is:** [GREEN REGENCY TEACUP AND SAUCER]  
> **Confidence:** 90.29% | **Support:** 2.64% | **Lift:** 18.06x  

These insights are directly applicable to:
1. **UI/UX Optimization:** Dynamic "Frequently Bought Together" widgets.
2. **Product Bundling:** Creating targeted combo deals to increase Average Order Value (AOV).
3. **Targeted Marketing:** Automated email retargeting based on complementary items.

## 💻 Requirements
To run this notebook, ensure you have the following Python libraries installed:
```bash
pip install pandas numpy mlxtend matplotlib seaborn networkx
```
Note: If you wish to run the algorithm performance benchmark, you may also need to install the pyfim library, which requires a C compiler.
