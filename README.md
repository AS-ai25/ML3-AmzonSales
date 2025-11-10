## README File: Sales Prediction Model for Amazon India Garment Sales

This project focused on developing a **Classification Prediction Model** to forecast whether a specific women's apparel product sold on the Amazon India platform would successfully sell (resulting in a "Shipped" status).

### Project Objectives

1.  **Sales Prediction:** Develop a machine learning model capable of predicting the **order status** ("Shipped" / "Pending").
2.  **Business Insights Generation:** Analyze the data to derive conclusions about consumption patterns, seasonal trends, and the impact of product characteristics on sales success.
3.  **Strategic Decision Support:** Facilitate more accurate decision-making in inventory planning, pricing, and targeted marketing.

---

###Architecture and Methodology

#### 1. Data Sources and Integration
The analytical foundation was built by merging four main data tables from real Amazon India sales, documenting transactions over several months.
* **Core Table:** "Amazon Sale Report" (128,975 rows).
* **Supplementary Tables:** International Sales, Periodic Catalogs, and Stock/Sale Reports.
* **Data Range:** Integration focused on data from March to June 2022.
* **Final Dataset Size:** After cleaning and imputation, the dataset comprised **29 columns and 130,425 rows**.

#### 2. Data Preparation and Feature Engineering
* **Cleaning & Normalization:** Verified key integrity (e.g., order\_id and SKU), removed erroneous/duplicate records, and standardized column names and data types. Literal values like colors and sizes were cleaned and unified.
* **Feature Creation:** New columns were calculated, including **Price per Unit** (`unit_price`) and time-based features (weekday, month, season, weekend flag). Boolean variables like `has_promotion` and `fulfilled_by_amazon` were added.
* **Imputation of Missing Values:**
    * **Numerical:** Missing values were imputed using the **MICE** (Multiple Imputation by Chained Equations) method via `IterativeImputer`.
    * **Non-Numerical:** Categorical and textual columns were filled with the "unknown" category. Boolean columns were completed with `False`.
* **Outlier Treatment:** Outliers in central variables were identified and assessed using statistical tests to ensure their retention did not compromise the model.

#### 3. Feature Selection
Features were selected if they were deemed significant by at least five different prediction models (including Lasso, Ridge, SVM, and various Tree-based models).
* **Top 8 Features** (scored 7 out of 8 models): `ship-postal-code`, `pcs`, `currency`, `amount`, `stock`, `weekday`, `day`, and `design_no`.

#### 4. Winning Model Selection

| Metric | Winning Model | Value | Rationale for Selection |
| :--- | :--- | :--- | :--- |
| **ROC\_AUC** | GradientBoostingClassifier | **0.997526** | Highest ROC\_AUC, indicating better discrimination between classes. |
| **LogLoss** | GradientBoostingClassifier | **0.002640** | Lowest LogLoss, indicating more stable and confident predictions. |
| **Accuracy** | GradientBoostingClassifier | **0.999483** | Chosen for its optimal combination of accuracy, stability, and superior tuning capability compared to other models. |

* **Class Imbalance:** After testing several balancing techniques, the data was ultimately used **as is** (without artificial balancing) because this configuration yielded the best prediction results.

---

###Key Business Insights

The model identified the most influential factors determining the sale status:

1.  **Seasonality (`season`) - The Dominant Variable (Over 33% of Impact):**
    * **Insight:** Seasonality and calendar alignment are critical, indicating the need for dynamic inventory and pricing.
    * **Recommended Action:** Significantly increase stock and marketing budgets ahead of seasonal high-demand periods (e.g., holidays/festivals) and promptly reprice or clear off-season items.
2.  **Unit Quantity (`pcs`) - High Impact (Over 33% of Impact):**
    * **Insight:** Successful sales are strongly related to the number of units purchased, often involving complementary sets or authentic attire.
    * **Recommended Action:** Implement aggressive **Cross-Selling and Bundling** strategies, promoting product packages. Monitor inventory closely, especially for items sold as part of a set, to prevent out-of-stock components.
3.  **Pricing, Promotions, and Timing (`has\_promotion`, `currency`, `day`):**
    * **Insight:** Promotions positively impact both quantity sold and the **Average Order Value (AOV)**, as higher-priced products tend to sell more during these periods. Sales volume also noticeably decreases towards month-end.
    * **Recommended Action:** Use promotions strategically to raise AOV, not just for clearance. Intensify marketing efforts near month-end or reallocate budget to the start of the month.

---

###Final Conclusion

The **Gradient Boosting Model** provides the optimal combination of high accuracy (≈1.00), stability, and generalization capability, making it the final model of choice for sales status forecasting and generating data-driven operational insights.

---

I can provide the specific metrics (Precision, Recall, F1) for the winning model's performance on the Test Set, or detail the sales patterns found in the DEA phase (e.g., leading categories and sizes). Which would you prefer?
