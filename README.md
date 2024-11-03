# Power-Bi_Financial-Dataset-Dashboard

Dashboard Link:

## Problem Statement

A company seeks to enhance its financial health by closely monitoring key financial metrics such as sales, receivables, payables, and inventory. The goal is to gain actionable insights into cash flow efficiency, optimize the timing of collections and payments, and effectively manage inventory. This Power BI dashboard aims to provide a holistic view of business performance by breaking down these elements into measurable insights.

### Key Requirements for the Dashboard
#### 1) Sales and Receivables Information

- Sales & Receivable Amount: Display sales and the total receivable amount owed by customers.
- Receivable by Region: Show Receivable in each region.
- Top Customers: List the top five customers by outstanding receivable amounts, allowing focus on key accounts.
- Aging Analysis of Receivables: Break down receivables by age categories (e.g., <30 days, 31-60 days, 61-90 days) to monitor overdue payments.
- Days Sales Outstanding (DSO): Provide insight into the average time taken to collect receivables, showing efficiency in collections and cash flow impact.

#### 2) Payables Information

- Due by Supplier: List top suppliers by outstanding payable amounts to prioritize payment schedule.
- Aging Analysis of Payables: Show the total amount the company owes to suppliers, including a breakdown of overdue payments in each aging category. (e.g., <30 days, 31-60 days, 61-90 days) to manage overdue balances and avoid supplier penalties.
- Days Payable Outstanding (DPO): Monitor the average time taken to pay suppliers, reflecting on working capital utilization and supplier relations.

#### 3) Inventory Management

- Inventory Levels and Value: Display the current units and value of unsold inventory, indicating investment in stock.
- Inventory Aging: Categorize inventory based on aging (e.g., <30 days, 31-60 days), identifying items that may require discounting or liquidation.
- Days Inventory Outstanding (DIO): Track how long inventory stays in stock, influencing storage costs and cash flow allocation.



### Steps followed 

- Step 1: Loaded data into Power BI Desktop; the dataset is a CSV file.
- Step 2: Open the power query editor & in the view tab under the Data Preview section, and check the "column distribution", "column quality" & "column profile" options. 
- Step 3: Also, since by default, the profile will be opened only for 1000 rows, so select "column profiling based on the entire dataset." 
- Step 4: It was observed that in none of the columns, errors & empty values were present except the column named "Receipt Date". 
- Step 5: For calculating receivable and payable, columns with null "Receipt Date" values were taken into account as they indicate due.  
- Step 6: In the report view, under the view tab, the theme was selected. 
- Step 7: Three pages were created for each account receivable, account payables, and inventory aging analysis.

#### Account Receivables

- Step 8: In the report view, under the account receivable page, under the insert tab, one text box is added to the canvas to write "Account Receivables".
- Step 9: A card is added to the report design area representing the "total sales" with a filter for the year 2018. 
- Step 10: A second card is added to the report design area representing the "total due" using sales for which "Receipt Date" is blank. 
- Step 11: A new measure is created, "Days Sales Outstanding (DSO)" by dividing "total receivable amount" using sales for which "Receipt Date" is blank divided by total sales in 2018, multiplied by 365.

           DSO = 
                (
                CALCULATE(SUM(Datatbl[Sales]),ISBLANK(Datatbl[Receipt Date]))
                /
                CALCULATE(SUM(Datatbl[Sales]),Datetbl[Year]=2018)
                )
                *365
- Step 12: A KPI visual was added for "Days Sales Outstanding (DSO)" to show the average time taken to collect receivables, reflecting the efficiency of collections and overall cash flow impact.
- Step 13: Visuals for Receivable by State. Added a table visual for Receivable by Region to display the total due in each state. Used State and Total Due fields from the data. Applied data bars for easy readability of each region’s outstanding amounts.

- Step 14: Top 5 Customers by Receivables. Created a Bar Chart visual showing the Top 5 Customers by outstanding receivable amounts. Used CustomerID and Total Due fields. Filtered for the top five values and sorted in descending order to spotlight the largest outstanding accounts.

- Step 15: Aging column was created to categories and displayed total receivables by age.

for creating new column following DAX expression was written;
       
        Aging = 
        IF(Datatbl[Payment]>90,">90 Days", 
                (IF(Datatbl[Payment]>60,"61-90 Days",
                        IF(Datatbl[Payment]>30,"31-60 Days",
                                "<30 Days"))))
        
Snap of new calculated column ,

![image](https://github.com/user-attachments/assets/e83f26b5-b3de-4690-8a9f-d84311d52bba)

- Step 16: Added a Stacked Column Chart visual for Receivable Aging Analysis to show receivables in categories: <30 days, 31-60 days, and 61-90 days.

#### Account Payables

- Step 17: In the report view, under the Account Payables page, under the insert tab, one text box is added to the canvas to write "Account Payables".
- Step 18: Added a Card visual to show Total Purchase and another Card for Total Due by payables, representing the amount owed to suppliers.

- Step 19: Due by Supplier. Added a Table visual listing Due by Supplier with Vendor and Total Due columns to identify priority payments. Filtered the table to show the top suppliers with outstanding payables. Applied data bars for easy readability of each vendor’s outstanding amounts.

- Step 20: Aging Analysis for Payables. Added a Stacked Column Chart for Payable Aging Analysis to categorize amounts due by aging periods (<30 days, 31-60 days, and 61-90 days). This aging chart helps manage overdue balances and avoid penalties.

- Step 21: A new measure is created, "Days Payable Outstanding (DPO)" by dividing "total payable amount" using sales for which "Receipt Date" is blank divided by total payments in 2018, multiplied by 365.

           DPO = 
                (
                CALCULATE(SUM(Datatbl[Purchase]),ISBLANK(Datatbl[Receipt Date]))
                /
                CALCULATE(SUM(Datatbl[Purchase]),Datetbl[Year]=2018)
                )
                *365

- Step 22: Days Payable Outstanding (DPO) KPI. Created a KPI Visual for DPO to show the average time taken to pay suppliers, calculated based on the measure for DPO. This DPO measure indicates working capital efficiency.

#### Inventory Aging Analysis

- Step 23: In the report view, under the Inventory Aging Analysis page, under the insert tab, one text box is added to the canvas to write "Inventory Aging Analysis".

- Step 24: A new measure is created, "Unsold Inventory" by SUMX "Unsold Quantity" and "Cost".

           Unsold Inventory = 
           SUMX(Datatbl,Datatbl[Unsold]*Datatbl[Cost])
- Step 25: Added Cards for Total Unsold Inventory (Value) and Total Unsold Inventory (Units) to indicate current stock levels and financial investment in inventory.


- Step 26 : Inventory Aging column was created to categories and displayed total Payable by age.

for creating new column following DAX expression was written;
       
        Inventory Aging = 
                IF(Datatbl[Inventory Days]>180,">180 Days", 
                        IF(Datatbl[Inventory Days]>90,"91-180 Days", 
                                IF(Datatbl[Inventory Days]>60, "61-90 Days",
                                        IF(Datatbl[Inventory Days]>30,"31-60 Days",
                                                "<30 Days"))))
        
Snap of new calculated column ,

![image](https://github.com/user-attachments/assets/055751c0-ef2e-4d5d-9b25-fb4d215053ea)


- Step 27: Inventory Aging Analysis. Added a Stacked Column Chart to categorize Inventory Aging based on time in stock (<30 days, 31-60 days, etc.), showing value in each aging bucket.This analysis helps identify items that may need discounting or liquidation.


- Step 28: A new measure is created, "Days Inventory Outstanding (DIO)" by dividing "Unsold Inventory" to "sold plus unsold inventory", multiplied by 365.

           DIO = 
                (
                SUMX(Datatbl,Datatbl[unsold]*Datatbl[Cost])
                /
                SUMX(Datatbl,Datatbl[Units]*Datatbl[Cost])
                )
                *365

- Step 29: Created a KPI Visual for DIO, calculated to represent the average time inventory is held, reflecting on storage cost and stock turnover efficiency.


        
 - Step 30: Final Dashboard Formatting. Adjusted visuals for consistent color schemes across pages and added titles to each section. Reviewed data filters and interactions to ensure smooth, focused navigation across visuals.

- Step 31: Add a Table visual to the Inventory Management page to display the Top 20 Inventory Items. Data Fields: Select StockCode, Sum of Cost, and Days Inventory Outstanding (DIO) fields to include in the table, showing each item’s identifier, cost, and days in inventory. Top 20 Filter: Apply a filter 
 - Step 32 : The report was then published to Power BI Service.
 
 
![image](https://github.com/user-attachments/assets/110468f1-9990-46ee-b73a-9880e006b1ce)


# Snapshot of Dashboard (Power BI Service)

![image](https://github.com/user-attachments/assets/0ac6e41d-fb7b-4e2e-8775-038b269e7246)

![image](https://github.com/user-attachments/assets/e05c9497-4f75-41df-b72e-449cc0a759c0)
 
 ![image](https://github.com/user-attachments/assets/e11fa9c0-22a6-4a27-9347-b320df70ffba)
 # Report Snapshot (Power BI DESKTOP)

 
![image](https://github.com/user-attachments/assets/4461000a-9d0a-49e2-9cfd-9ec919c768bd)

![image](https://github.com/user-attachments/assets/babf252f-4b33-4df5-b4f3-7a2f0f072c39)

![image](https://github.com/user-attachments/assets/61352638-4f69-4fb8-8435-2a536f2430ad)

# Insights - 62,211 Rows Dataset

A three page report was created on Power BI Desktop & it was then published to Power BI Service.

Following inferences can be drawn from the dashboard;

### [1] Sales and Receivables

   Total Sales: The company generated sales totaling ₹5,78,90,538, indicating a strong revenue flow. However, monitoring the proportion of these sales that remain as receivables is crucial for cash flow.

   Total Receivables (Due): Outstanding receivables amount to ₹1,16,77,505. This accounts for approximately 20% of total sales, suggesting a significant portion of sales is yet to be collected. Efforts to speed up collections could improve liquidity.


   Receivables by Region:
Top 3 Regions: Delhi (₹51,91,030), Gujarat (₹24,53,900), and Maharashtra (₹18,85,217) hold the highest receivable amounts, representing over 80% of total receivables. These regions should be a priority in collection efforts to accelerate cash inflow.


   Top Customers: The top five customers collectively owe substantial receivables, with the largest single outstanding amount around ₹1.25M. Targeting these key accounts for timely payments could significantly enhance cash flow.

Aging Analysis of Receivables:

        <30 Days: ₹4.7M
        31-60 Days: ₹3.7M
        61-90 Days: ₹3.4M

The aging analysis indicates that receivables are spread across various aging categories, with a noticeable amount overdue. Focusing on overdue payments could reduce the Days Sales Outstanding (DSO), which currently stands at 74 days.
           
### [2] Payables Information

Total Purchases: Purchases total ₹2,76,78,983, reflecting the company's expenditure on goods or services.

Total Payables (Due): Outstanding payables amount to ₹55,53,031, about 20% of total purchases. This indicates a moderate level of obligations to suppliers, manageable but still impactful on working capital.

Top Suppliers: The top suppliers with the largest amounts due include vendors 10, 41, 25, 24, and 31, making up a significant portion of payables. Prioritizing payments to these suppliers could help maintain favorable terms and supplier relations.

Aging Analysis of Payables:

        <30 Days: ₹2.21M
        31-60 Days: ₹1.75M
        61-90 Days: ₹1.59M
Similar to receivables, payables are spread across aging categories, with some overdue. This distribution implies a need to improve cash flow management to meet obligations on time and avoid late fees or strained supplier relationships. Days Payable Outstanding (DPO) is 73 days, suggesting relatively prompt payments.
  
  ### [3] Inventory Management
Inventory Levels and Value:
Total inventory value is ₹4,00,41,271, with 2,52,939 units in stock, showing a high level of investment in inventory.

Top 20 Inventory: The most valuable items in inventory collectively hold around ₹20,00,796 in value. Items with high holding periods should be reviewed for potential discounting or clearance to free up cash.

Inventory Aging Analysis:

        >180 Days: ₹92M (largest portion)
        91-180 Days: ₹14M
        61-90 Days: ₹5M
        31-60 Days: ₹3M
        <30 Days: ₹1M
A substantial portion of inventory (₹92M) has been held for over 180 days, tying up capital in slow-moving stock. Reviewing aged inventory for obsolescence or discounting opportunities may reduce carrying costs and improve turnover.

Days Inventory Outstanding (DIO): Currently at 154 days, indicating the inventory is held in stock for an extended period. Reducing DIO through better demand forecasting and inventory control could enhance cash flow and lower storage costs.

 ### [4] Summary Recommendations

#### Improve Receivable Collections: With a high DSO of 74 days and substantial overdue receivables, accelerating collections in key regions (Delhi, Gujarat, Maharashtra) and from top customers could improve cash flow.

#### Optimize Payables Management: Maintaining or potentially extending the DPO while ensuring timely payments to priority suppliers could improve working capital without risking supplier relationships.

#### Streamline Inventory: Reducing DIO by addressing slow-moving items, particularly those held for more than 180 days, could free up significant cash tied in inventory. This might involve strategic discounting or a change in stock management practices.

