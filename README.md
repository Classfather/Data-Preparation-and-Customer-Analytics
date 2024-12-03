# Data Preparation and Customer Analytics

## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools-used)
- [Data preparation](#data-preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Conclusion](#conclusion)
- [Recommendation](#recommendation)

### Project Overview
The Category Manager for Chips wants to better understand the types of customers who purchase Chips and their purchasing behaviour within the region. I seek to:
- Analyze transaction and customer data to identify trends and inconsistencies. 
- Develop metrics and examine sales drivers to gain insights into overall sales performance. 
- Create visualizations and prepare findings to formulate a clear recommendation for the client's strategy.
  
The insights from the analysis will feed into the supermarket’s strategic plan for the chip category in the next half year.

### Data Sources

[Customer Data:](qvi-purchase_behavior.csv) This is the "QVI_purchase_behaviour.csv" file. It is the dataset containing detailed information about the different types of customers that patronized the company, the customers are categorized considering two factors: Life Stages and premium customers. 

Transaction Data: This is the "QVI_transaction_data.csv" file. It is the dataset containing detailed information about the purchase by each customer.

Data: This is the "QVI_data.csv" file. it is the combination of the Customer data and Transaction data after performing Data cleaning on both datasets 

### Tools Used
- Jupyter notebook
- R programming

### Data Preparation
In the initial Data preparation, i perfomed the following task:
1. Data loading and inspection
2. Handled missing values and outliers
3. Data cleaning and formatting
4. Merge the Customer data and Transaction Data

### Exploratory Data Analysis
Now that the data is ready for analysis, I can define some metrics of interest to the client:
- Who spends the most on chips(total sales)?
- Describing customers by lifestage and how premium their general purchasing behaviour is?
- How many customers are in each segment?
- How many chips are bought per customer by segment?
- What’s the average chip price by customer segment?

### Data Analysis
 These are some of the analysis of Chips sales performance.
 
![Chips Sales Chart](https://raw.githubusercontent.com/username/repository/main/assets/chips_sales_chart.png)

![Average Number of Units per Customer by Segment](https://github.com/user-attachments/assets/111ee801-ab84-4b6c-a83e-d078f3f87782)

![Number of Unique Customers by Lifestyle](https://github.com/user-attachments/assets/a2069483-4b3c-4e7e-9e3a-b86037dbd8b9)

 
![Average Price per Unit by Segment](https://github.com/user-attachments/assets/2d05e1a4-2ffb-4e22-a6ff-55997654017c)
![Total Chip Sales by Customer Segment](https://github.com/user-attachments/assets/88a310b3-8703-4bbe-b6cc-812429d93d0d)

 Perform an independent t‐test between mainstream vs premium and budget midage and young singles and couples 
```R
pricePerUnit = data[, price := TOT_SALES/PROD_QTY] 
t.test(data[LIFESTAGE %in% c("YOUNG SINGLES/COUPLES", "MIDAGE SINGLES/COUPLES") 
            & PREMIUM_CUSTOMER == "Mainstream", price]
               , data[LIFESTAGE %in% c("YOUNG SINGLES/COUPLES", "MIDAGE SINGLES/COUPLES")
            & PREMIUM_CUSTOMER != "Mainstream", price]
                , alternative = "greater")
```
![Brand Preference for Mainstream Young Singles-Couples](https://github.com/user-attachments/assets/513adaf6-8dcd-4877-8fda-f37792b5a063)

### Conclusion
The analysis results are summarize as follows:
1. Sales have mainly been due to Budget-older families, Mainstream-youngsingles/couples, and Mainstream - retirees shoppers. 
2. High spend in chips for mainstream young singles/couples and retirees is due to there being more of them than other buyers. 
3. Mainstream, midage and young singles and couples are also more likely to pay more per packet of chips, This is indicative of impulse buying behaviour.
4. Mainstream young singles and couples are 23% more likely to purchase Tyrrells chips compared to the rest of the population.

### Recommendation
Based on the analysis, I recommend that the Category Manager should increase the category’s performance by off-locating some Tyrrells and smaller packs of chips in discretionary space near segments where young singles and couples frequent more often to increase visibilty and impulse behaviour.
