
# Medicare Part B: A Comprehensive Analysis of High-Cost Drugs and Spending Trends

# Introduction
Medicare Part B is one of the four parts of the U.S. government's Medicare program, providing
health insurance coverage to eligible individuals, mainly those who are 65 and older, and some
younger individuals with disabilities.

In this project, the primary goal was to analyze the rising drug costs under Medicare Part B by identifying high-cost drugs, analyzing spending trends, and providing recommendations to optimize spending while ensuring quality healthcare for enrollees. A critical part of the process involved ensuring the accuracy, consistency, and integrity of the dataset, which is where data profiling and data quality checks became essential.

## Quick Link: 
[Power BI Dashboard](https://app.powerbi.com/view?r=eyJrIjoiYjhkMzc1MjMtNGYzMy00OTY0LWE3MWEtZGJhYjVmYWYwOWU1IiwidCI6ImVlMmQ2ZDcyLTk1MzUtNDI0Mi1hMDc3LWFjZjE4NTc4MmY5YiIsImMiOjF9) 

## Why Data Profiling and Quality Checks Matter
Data profiling is crucial for understanding the structure, relationships, and content of your dataset before diving into deeper analysis. It helps identify anomalies, inconsistencies, and potential data quality issues, such as missing values, duplicates, and incorrect formats. Without thorough data profiling, any subsequent analysis or insights drawn could be skewed, ultimately leading to incorrect conclusions or decisions.
For this, I primarily used SQL Server Integration Services (SSIS), which offers powerful data profiling features to assess the quality and reliability of the data. By leveraging SQL Data Quality Services for further quality checks, I ensured that the dataset met key standards such as completeness, uniqueness, and consistency. These steps were essential before moving forward with the analysis to maintain trust in the accuracy of the results.

## Step 1: Data Collection
Medicare Part B Spending by Drug dataset (from the Centers for Medicare & Medicaid Services)
https://catalog.data.gov/no/dataset/medicare-part-b-spending-by-drug-f3e5c
This dataset gives information about how much money is spent on a drug prescribed to
Medicare(B) enrollees. It also includes details about the cost for each dose on average and the
trend in how the cost has changed over time. The dataset also focuses on spending information
for manufacturer(s) of the drugs along with some user-friendly information about what these
drugs are used for and when to be used.

# Step 2: Data Quality
To assess the quality of data,I used SQL Server Integration Services (SSIS) for a comprehensive
evaluation of data quality in the Medicare Part B spending dataset. The objective was to assess
key aspects such as completeness, consistency, uniqueness, timeliness, and other relevant
metrics. Through data profiling using SSIS, I conducted an analysis of the data, including the
examination of NULL value ratios, values lengths, and various measurements. The outcomes of
this Data Profiling Task were documented in the Medicare.xml file, which was subsequently
reviewed using the Data Profile Viewer tool. The results provided valuable insights crucial for
the development and design of Extract, Transform, Load (ETL) operations and dimensional
structures within the solution

## Results:
The dataset has 615 rows and 47 columns.The first profiling output to observe is the Candidate
Key Profiles. Results highlighted two columns, namely "Brand name" and "HSPCS_CD," which
exhibit complete uniqueness across the entire table, registering a 100 percent uniqueness rate.
Additionally, the column "HCPCS_desc" demonstrated a high uniqueness rate of 99 percent.
This information provides valuable insights into the distinctiveness of key columns within the
dataset.
![image](https://github.com/user-attachments/assets/48ea19fb-4620-4d10-b373-9cf1d664bd05)

The column length distribution shows the number of rows by length.

![image](https://github.com/user-attachments/assets/743ac8f1-4dc4-4edd-8930-dbee6bae614e)

The Null Ratio for each column indicates the percentage of rows in the entire dataset that
contains NULL values. This information is crucial for ETL planning as it delineates instances
where handling NULL values is necessary during the ETL process. Notably, the column with the
highest count of NULL values is "Total_beneficiary_2017."
The Column Value Distribution Profile is used to scrutinize the spread of values within a
particular column of the dataset. The statistical findings for each column are attached below,
offering a detailed understanding of the dataset's characteristics.

![image](https://github.com/user-attachments/assets/d365b8ff-fc39-4185-bbc8-1843167963b1)

![image](https://github.com/user-attachments/assets/e099ec02-2c23-4053-9d8b-adce7d839444)

The Column Statistics Profile is valuable when evaluating the source of fact tables, particularly
as measures in a fact table are predominantly numeric. Given the prevalence of fact tables in our
Medicare B dataset, the results give a comprehensive statistical report for all the tables. These
outcomes are useful in comparing the minimum drug spending across the four years and
identifying the maximum amount spent on drugs during this period.The results indicate that the
highest expenditure on Medicare B drugs occurred in the year 2021, while the lowest was
recorded in 2017. This observed trend of increasing drug expenditures each year suggests that
either the cost of drugs is rising or, alternatively, there is an escalating demand for drugs.

![image](https://github.com/user-attachments/assets/95d232c2-97eb-4aec-aed8-8bcef82def45)


## Step 3: Data Cleaning and Transformation:
The data tables for Part B spending on drugs and discarded Medicare Part B drugs were imported
into PowerBI for cleaning. Using the results from the data profiling task, I removed all instances
of null values.Since the data had multiple columns in which few columns are not in use, I
removed unwanted columns that are not helping for the analysis. The final set of columns used
for analysis includes Brand_name, Generic name, Total_spending_2017, Total_spending_2018,
Total_spending_2019, Total_spending_2020, Total_spending_2021,
Avg_spending_per_dosage_unit_2017, Avg_spending_per_dosage_unit_2018,
Avg_spending_per_dosage_unit_2019, Avg_spending_per_dosage_unit_2020,
Avg_spending_per_dosage_unit_2021, Avg_spending_per_bene_2017,
Avg_spending_per_bene_2018, Avg_spending_per_bene_2019, Avg_spending_per_bene_2020,
Avg_spending_per_bene_2021, Avg_DY21_ASP_Price,
Chg_Avg_Spndng_Per_Dsg_Unt_20_21, Annual Growth Rate in Average Spending per Dosage
Unit. As an initial step in data transformation, two additional columns were created. The first
column aggregates the total beneficiaries over the five years by summing the beneficiaries for
each of the four years. The second column calculates the total spending across all five years,
providing insight into which drugs incurred the highest expenditure. This comprehensive data
transformation process ensures that the dataset is refined for in-depth analysis.

![image](https://github.com/user-attachments/assets/ff9e6b1e-5c7e-41eb-86a8-34bb5de173fc)

Fig 6 : Power BI data cleaning

## Additional Data processing and transformation:
Using the data preview option in powerBI, certain features such as column statistics and value
distribution revealed that the 'total_benes_across_years' column contains some empty values.
Similarly, the 'Avg_spending_per_ben_2021' column also has some empty values which can be
removed. Furthermore, column values were converted to currency and percentages where
required. To analyze the trend over the years, one of the research questions posed was to
visualize the total spending on Part B medicines across different years. To achieve this, the
column was unpivoted to separate attributes and values individually. New column
'Total_Benes_Across_Years' was calculated as the sum of beneficiaries for each year
(2017-2021).
Table.AddColumn(#"Removed Columns", "Total_Benes_Across_Years", each
[Tot_Benes_2017] + [Tot_Benes_2018] + [Tot_Benes_2019] + [Tot_Benes_2020] +
[Tot_Benes_2021])
The resulting table was transformed to have 'Total_Benes_Across_Years' in currency format for
better visualization.

![image](https://github.com/user-attachments/assets/eea95a3d-4498-4826-8853-4a2b54fd1b98)

Fig 7 : Data processing using PowerBI data preview option

## Visualization using PowerBI:
Following these transformations, two dashboards were created. One provides a holistic view of
overall spending in Medicare Part B, while the other offers a detailed perspective on Medicare
Part B drugs.Some of the questions that I was trying to answer were:

● Which drugs had the highest and lowest total spend over the overall years?
● What are the total number of beneficiaries for each drug over the cumulative 5 years to
determine which drug has the highest number of beneficiaries?
● How has the average annual growth rate been for each of these drugs in Medicare Part B?

![image](https://github.com/user-attachments/assets/761261c4-8821-489c-9d7d-b94841292318)
### Fig: Holistic view of High-Cost Drugs and Spending Trends over the 5 years(2017 - 2021)

![image](https://github.com/user-attachments/assets/7c343dfc-902d-45a7-bf47-74e36d08f69f)
### Fig : Dynamic Insights Medicare Part B Drug Expenditure Trends (2017-2021)


  1. The rising cost of drugs under Medicare Part B is a significant challenge, potentially straining
healthcare budgets and affecting the quality of care for beneficiaries. Visualizing these questions
aids in identifying bottlenecks. A graph was created to visualize the top 15 high-cost drugs over
the aggregated five years and the top 15 low-cost drugs with the lowest overall spending.
Overall, the amount spent on Medicare Part B drugs over the five years amounted to around
$766 billion. The top 5 drugs with the highest total spending were Eylea, Keytruda, Opdivo,
Prolia, and Lucentis.

  2. Similarly, another BI report was created with sliders, utilizing one for the years (2017-2021) and
another for drug names. These slicers enhance the interactive experience, allowing users to filter
out specific drugs and visualize their trends over the five years. The year slicer provides the
flexibility to focus on specific periods, facilitating a detailed examination of annual variations.
Simultaneously, the drug name slicer offers a targeted approach, enabling users to explore the
unique trends associated with individual drugs. By combining these slicers, stakeholders gain a
powerful tool to view and understand the data dynamically, honing in on specific timeframes and
drugs of interest. This interactive feature enhances the usability of the BI report, fostering a more
customized and insightful analysis.

## Conclusion:

By visualizing the spending trends, identifying high-cost drugs, and analyzing
beneficiary-related metrics over the five-year period, these dashboards offer a valuable tool to
address the pressing issue of escalating drug costs under Medicare Part B. The insights derived
from the detailed visualization of the top 15 high-cost and low-cost drugs, coupled with trends
specific to individual drugs, help us to make informed choices like focusing on the high cost
drugs like Eylea, Keytruda, Opdivo, Prolia, and Lucentis to identify the reason for higher
expenditure . This comprehensive understanding facilitates the optimization of drug spending,
ensuring efficient utilization of healthcare budgets without compromising the quality of care for
Medicare Part B beneficiaries. The BI analysis serves as a proactive strategy for managing and
mitigating the impact of rising drug costs, striking a balance between financial sustainability and
the delivery of high-quality healthcare services.
