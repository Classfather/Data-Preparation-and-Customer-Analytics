# Data Preparation and Customer Analytics

## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools and Libraries](#tools-and-libraries)
- [Data preparation](#data-preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Conclusion](#conclusion)
- [Recommendation](#recommendation)

### Project Overview
---
The Category Manager for Chips wants to better understand the types of customers who purchase Chips and their purchasing behaviour within the region. I seek to:
- Analyze transaction and customer data to identify trends and inconsistencies. 
- Develop metrics and examine sales drivers to gain insights into overall sales performance. 
- Create visualizations and prepare findings to formulate a clear recommendation for the client's strategy.
  
The insights from the analysis will feed into the supermarket’s strategic plan for the chip category in the next half year.

### Data Sources
---
[Customer Data:](QVI_purchase_behaviour.csv) This is the "QVI_purchase_behaviour.csv" file. It is the dataset containing detailed information about the different types of customers that patronized the company, the customers are categorized considering two factors: Life Stages and premium customers. 

[Transaction Data:](QVI_transaction_data.xlsx) This is the "QVI_transaction_data.csv" file. It is the dataset containing detailed information about the purchase by each customer.

[Data:](QVI_data.csv) This is the "QVI_data.csv" file. it is the combination of the Customer data and Transaction data after performing Data cleaning on both datasets 

### Tools and Libraries
---
- Tools
  - Jupyter notebook
  - R programming
- Libraries
  - data.table
  - ggplot2
  - ggmosaic
  - readr
  - stringr

### Data Preparation
---
In the initial Data preparation, i perfomed the following task:

**Loading Data:** I imported customer and transaction data using the data.table and readxl libraries. This allowed for efficient handling of large datasets.

**Date Conversion:** The transaction date was initially stored as an integer in the data. You converted this to a proper Date format using as.Date, ensuring correct handling of time series data for analysis.

**Product Analysis:** I analyzed the product names to identify the most frequent terms and clean them up by removing words containing digits or special characters. This was done to understand common keywords associated with chip products and ensure consistency in further analysis (e.g., standardized pack size or brand names).

**Outlier Detection:** I identified potential outliers in the transaction data, specifically looking for extreme product quantities (e.g., 200 packs). Outliers were flagged for further examination, but it was found that no outliers required removal in the dataset.

**Customer Filtering:** I removed unwanted customer data based on loyalty card numbers, ensuring the analysis focused on the correct group of customers.

### Exploratory Data Analysis
---
These are some of the analysis of the Chips Sales Performance

**Transaction Count by Date:** I calculated and visualized the number of transactions per day, which helped uncover patterns and trends over time. By focusing on December transactions, I was able to examine seasonality or sales spikes during the holiday period.

![Transactions over time](https://github.com/user-attachments/assets/d5ed5441-734b-4d25-8119-42b40a0d4d8a)

![Number of Transaction per Day in December](https://github.com/user-attachments/assets/0f76b160-0ef3-45b4-80de-7f16104c288e)

**Pack Size Distribution:** I analyzed the distribution of product pack sizes (e.g., 70g, 380g) and visualized it using a histogram. This showed that the majority of transactions involved chips with a specific pack size, helping to identify which sizes are most popular among customers.

![Distribution of Transaction by Pack Size](https://github.com/user-attachments/assets/907e1002-cb3d-49bd-a638-b8f017755dbc)

**Brand Name Extraction:** I extracted the first word from each product name as the brand, standardized variations (e.g., "RED" to "RRD"), and checked the consistency across the dataset. This allowed for a more streamlined analysis of brand-level sales performance.
```r
#Combine similar brand names
transactionData[BRAND %in% c("Red", "RRD"), BRAND := "RRD"]
transactionData[BRAND %in% c("Snbts", "Sunbites"), BRAND := "SUNBITES"]
transactionData[BRAND %in% c("Infzns", "Infuzions"), BRAND := "INFUZIONS"]
transactionData[BRAND %in% c("Smith", "Smiths"), BRAND := "SMITHS"]
transactionData[BRAND %in% c("NATURAL", "Natural"), BRAND := "NATURAL"]
transactionData[BRAND %in% c("Dorito", "Doritos"), BRAND := "DORITOS"]
transactionData[BRAND %in% c("Grain", "GrnWves"), BRAND := "GRNWVES"]
transactionData[BRAND %in% c("WOOLWORTHS", "Woolworths","WW"), BRAND := "WOOLWORTHS"]


#Check the unique brand names to ensure they are standardized
unique_brands <- unique(transactionData$BRAND)
print(unique_brands)
transactionData[, .N, by = BRAND][order(BRAND)]
```

**Customer Segmentation:** I segmented customers based on attributes like LIFESTAGE (e.g., young singles, couples, mid-aged) and PREMIUM_CUSTOMER (e.g., mainstream or premium). This segmentation helped in understanding how different groups interacted with the product category.

![Distribution of Customer Lifestages](https://github.com/user-attachments/assets/6aab3003-0ec6-46fb-a501-88863be27b18)

![Distribution of Premium Customers](https://github.com/user-attachments/assets/0afb2f8a-42e6-4acb-96fc-0b3c637d6782)

**Customer Demographics:** I explored the distribution of customers by life stage and premium status, creating visualizations to show how customer demographics aligned with purchasing behavior.

![Total Chip Sales by Customer Segment](https://github.com/user-attachments/assets/d20987bd-9563-4aa4-9c48-1cb3211777d4)

![Number of Unique Customers by Lifestyle](https://github.com/user-attachments/assets/81e3ad66-a1f4-436f-923c-2611177221c6)

Missing Data Check: I checked for missing values in customer data and found none, ensuring that the analysis could be conducted without concerns about incomplete data.
```r
#Check for missing values in each column
missing_values <- sapply(customerData, function(x) sum(is.na(x)))
print(missing_values)
```
| LYLTY_CARD_NBR  | LIFESTAGE | PREMIUM_CUSTOMER |
|--------------|----------------|--------------|
| 0     | 0              |0|

**Merging Datasets:** I merged transaction data with customer data based on the loyalty card number (LYLTY_CARD_NBR). This allowed me to link each transaction to a specific customer, enabling a deeper analysis of customer behavior and sales performance.

**Handling Missing Customer Information:** I identified and handled missing customer information, ensuring no critical data was lost during the merging process

Now that the data is ready for analysis, I can define some metrics of interest to the client:
- Who spends the most on chips(total sales)?
- Describing customers by lifestage and how premium their general purchasing behaviour is?
- How many customers are in each segment?
- How many chips are bought per customer by segment?
- What’s the average chip price by customer segment?

### Data Analysis
---

These are some of the analysis of Chips sales performance.


**Total Sales by Customer Segment:** I calculated total sales for each customer segment (based on life stage and premium status). This analysis provided insights into which customer groups contributed the most to sales, helping to prioritize target segments for the future.

![Total Chip Sales by Customer Segment](https://github.com/user-attachments/assets/7342c7ae-2adc-4ba7-a895-836603af2595)


**Unique Customer Count by Segment:** By calculating the number of unique customers within each segment, I identified the most engaged customer groups. This metric was useful for understanding the customer base's size and diversity.

![Number of Unique Customers by Lifestyle](https://github.com/user-attachments/assets/2e393fa8-6356-430f-a5f6-eb827db7fb17)

**Average Units per Customer:** I computed the average number of units purchased per customer within each segment. This metric helped to assess customer engagement and identify potential opportunities for increasing average purchase volume.

![Average Number of Units per Customer by Segment](https://github.com/user-attachments/assets/83ba4352-e457-4ee9-9963-c961a2f2e144)

**Average Price per Unit:** I analyzed the average price per unit sold, comparing it across customer segments. This highlighted any price sensitivity and the potential for price adjustments or promotions for different groups.

![Average Price per Unit by Segment](https://github.com/user-attachments/assets/a1ac947d-cd79-4b54-bfae-d51fd6f44820)

Statistical Testing (t-tests): I performed independent t-tests to compare price preferences between customer groups:

  - Mainstream vs Premium Customers: I found that mainstream customers paid a significantly lower price per unit compared to premium customers, indicating a potential price sensitivity difference.


  - Budget Mid-age vs Young Singles/Couples: I found that mid-aged budget customers had a different spending pattern compared to young singles and couples, suggesting different purchasing behaviors across life stages.


**Brand Preferences by Segment:** I analyzed brand preferences within a specific customer segment (Mainstream young singles/couples) and compared them with overall brand preferences across all segments. This allowed me to identify which brands were particularly popular in your target segment.

![Brand Preference for Mainstream Young Singles-Couples](https://github.com/user-attachments/assets/05fda8c2-2fde-4cd2-a10d-8c5bafee0fda)


**Pack Size Preferences:** I compared pack size preferences between the target segment (Mainstream young singles/couples) and the rest of the population. I found that the target segment tends to prefer larger pack sizes, which is valuable information for targeting product offerings or promotions to this group.

![Preffered Pack Size Comparison](https://github.com/user-attachments/assets/981f2193-f9f3-4557-b7b9-164ca7c0a191)


### Conclusion
The analysis results are summarize as follows:
1. Sales have mainly been due to Budget-older families, Mainstream-youngsingles/couples, and Mainstream - retirees shoppers. 
2. High spend in chips for mainstream young singles/couples and retirees is due to there being more of them than other buyers. 
3. Mainstream, midage and young singles and couples are also more likely to pay more per packet of chips, This is indicative of impulse buying behaviour.
4. Mainstream young singles and couples are 23% more likely to purchase Tyrrells chips compared to the rest of the population. Tyrells can be used for targeted marketing and promotions.

### Recommendation
Based on the analysis, I recommend that the Category Manager should increase the category’s performance by off-locating some Tyrrells and smaller packs of chips in discretionary space near segments where young singles and couples frequent more often to increase visibilty and impulse behaviour.
