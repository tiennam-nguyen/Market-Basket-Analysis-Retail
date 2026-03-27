# Market Basket Analysis: Online Retail Recommendations

## 📌 Project Overview
This project performs Market Basket Analysis on a UK-based non-store online retail dataset to discover hidden patterns in customer purchasing behavior. By leveraging the **FP-Growth algorithm**, the project extracts strong association rules to power a product recommendation engine (e.g., "Customers who bought X also bought Y"). 

The primary notebook for this analysis is `online-retail-transaction.ipynb`.

## 📊 Dataset Information
**Dataset:** [Online Retail Dataset](https://www.kaggle.com/datasets/ulrikthygepedersen/online-retail-dataset) - Ulrik Thyge Pedersen

This dataset contains transactional data with the following attributes:
* **InvoiceNo:** Invoice number. Nominal, a 6-digit integral number uniquely assigned to each transaction. If this code starts with letter 'c', it indicates a cancellation.
* **StockCode:** Product (item) code. Nominal, a 5-digit integral number uniquely assigned to each distinct product.
* **Description:** Product (item) name. Nominal.
* **Quantity:** The quantities of each product (item) per transaction. Numeric.
* **InvoiceDate:** Invoice Date and time. Numeric, the day and time when each transaction was generated.
* **UnitPrice:** Unit price. Numeric, Product price per unit in sterling.
* **CustomerID:** Customer number. Nominal, a 5-digit integral number uniquely assigned to each customer.
* **Country:** Country name. Nominal, the name of the country where each customer resides.

## 🛠️ Methodology & Data Cleansing
Real-world retail data is incredibly messy. This project implements a rigorous data preprocessing pipeline to ensure the resulting recommendation rules are mathematically sound and business-relevant:

1. **Handling Returns & Cancellations:** Filtered out invoices starting with 'C' and transactions where `Quantity <= 0` to prevent returned items from being algorithmically treated as "purchases".
2. **Noise Reduction:** Removed non-physical "administrative" items (e.g., "POST" for postage, "M" for manual entry, "D" for discount) using regex matching on `StockCode`. This prevents postage and fees from skewing the association rules.
3. **Entity Resolution:** Grouped basket items strictly by `StockCode` rather than text `Description`. This prevents the support of a single item from being fractured across multiple slightly different text descriptions.
4. **Geographical Scoping:** Filtered the dataset to only include transactions from the `"United Kingdom"` to isolate domestic retail behavior from international wholesale behavior.

## 🧠 Modeling: FP-Growth
To overcome the severe memory and processing bottlenecks associated with traditional Apriori algorithms, this project utilizes **FP-Growth** via the `mlxtend` library. 
* **Matrix Discretization:** Transaction quantities are converted to efficient boolean types (`True/False`).
* **Support & Confidence:** The model isolates frequent itemsets with a minimum support of **3%** and filters for strong rules with a minimum confidence of **50%** and a positive Lift (`Lift > 1`).

## 🚀 Results & Output
The script outputs highly actionable, business-ready insights rather than raw data tables. 

**Example Output:**
> **IF a customer buys:** [PINK REGENCY TEACUP AND SAUCER]  
> **Then the best item to recommend is:** [GREEN REGENCY TEACUP AND SAUCER]  
> **Confidence:** 82.08% | **Support:** 3.09% | **Lift:** 16.42x  

These statements can be directly implemented into e-commerce "Frequently Bought Together" UI features or used for targeted email marketing campaigns.

## 💻 Requirements
To run this notebook, you will need the following Python libraries installed:
* `pandas`
* `mlxtend`
