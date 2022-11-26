# Instacart Basket Analysis. 

Instacart, an online grocery store that operates through an app.  
Instacart already has very good sales, but they want to uncover more information about their sales patterns.  
My task was to perform an initial data and exploratory analysis of some of their data in order to derive insights and suggest strategies for better segmentation based on the provided criteria.

# Developed skills:  
- Exploratory Analysis using open-source data sets.  
- Provided business insights and strategy suggestions for marketing and segmentation.  
- Data set analysis to detect patterns.  
- Python, creating scripts to perform data wrangling and merge data frames.  
- Python, Data visualization.  
- Presentation results in **[Exel](https://github.com/halinakryvanos/Instacart-Grocery-Basket-Analysis/blob/02a900c9c475bfc1c45ff96aec35f67b6ea50877/05%20Sent%20to%20client/Final%20report%20Halina%20Kryvanos.xlsx)**

# A list of Jupyter notebooks and functions for Instacard tasks.   
Or **Refer to the short [video Jupyter Notebook](https://youtu.be/2BJDCA6wUz4) with the code**.  

Some code and visualization options are shown here, if You want to see the full report look this **[file](https://github.com/halinakryvanos/Instacart-Grocery-Basket-Analysis/blob/02a900c9c475bfc1c45ff96aec35f67b6ea50877/05%20Sent%20to%20client/Final%20report%20Halina%20Kryvanos.xlsx)**  

**Table of Contents**   
1. Data importing  
2. Data checks and merging dataframes  
3. PII privacy checks  
4. Assign regions  
``` 
#creating region flags with loc
df.loc[df['state'].isin(northeast), 'region'] = 'Northeast'
df.loc[df['state'].isin(midwest), 'region'] = 'Midwest'
df.loc[df['state'].isin(south), 'region'] = 'South'
df.loc[df['state'].isin(west), 'region'] = 'West' 
```
5. Flag and exclude customers with less than 5 orders  
```
# creating activity flag using loc
df.loc[df['max_order']< 5, 'activity_flag'] = 'Low Activity'
df.loc[df['max_order'] >= 5, 'activity_flag'] = 'High Activity'. 
```
6. Creating customer profile  
 6a. Age Profile  
``` 
# creating age group profile
df_high_activity.loc[df_high_activity['age']<=30, 'age_profile']= 'Young Adult'
df_high_activity.loc[(df_high_activity['age']>30) & (df_high_activity['age']<65), 'age_profile']= "Mid Adult"
df_high_activity.loc[df_high_activity['age']>=65, 'age_profile']= 'Senior'
df_high_activity['age_profile'].value_counts()
```
 6b. Parental Age Profile  
 ```
 # create parental age profile for different groups
df_high_activity.loc[(df_high_activity['age']<30) & (df_high_activity['no_of_dependents']>=1), 'parental_age_profile']= 'Young Parent'
df_high_activity.loc[(df_high_activity['age']>=30) & (df_high_activity['age']<40) & (df_high_activity['no_of_dependents']>=1), 'parental_age_profile']= 'Mid Parent'
df_high_activity.loc[(df_high_activity['age']>=40) & (df_high_activity['no_of_dependents']>=1), 'parental_age_profile']= 'Older Parent'
df_high_activity['parental_age_profile'].value_counts()
```
 6c. Single Parent Profile (Dependants Profile)   
 ```
 crosstabfam= pd.crosstab(df_high_activity['marital_status'], df_high_activity['no_of_dependents'], dropna=False)
 crosstabfam
 ```
```
# create parental-relationship status flag for different groups
df_high_activity.loc[(df_high_activity['marital_status'] == 'single') & (df_high_activity['no_of_dependents']>=1), 'parental_relationship_status']= 'Single Parent'
df_high_activity.loc[(df_high_activity['marital_status'] == 'divorved/widowed') & (df_high_activity['no_of_dependents']>=1), 'parental_relationship_status']= 'Single Parent'
df_high_activity.loc[(df_high_activity['marital_status'] == 'living with parents and siblings') & (df_high_activity['no_of_dependents']>=1), 'parental_relationship_status']= 'Single Parent'
df_high_activity.loc[(df_high_activity['marital_status'] =='married') & (df_high_activity['no_of_dependents']>=1), 'parental_relationship_status']= 'Nuclear Family'
df_high_activity['parental_relationship_status'].value_counts()
```
 6d. Relationship-Age Profile  
  ```
  # create a relationship-age flag
crosstab_ra= pd.crosstab(df_high_activity['age_profile'], df_high_activity['marital_status'], dropna=False)
crosstab_ra
   ```  
   ```
   df_high_activity.loc[(df_high_activity['age_profile']=='Mid Adult') & (df_high_activity['marital_status'] == 'divorced/widowed'), 'relationship_age_status']= 'Single Mid Adult'
df_high_activity.loc[(df_high_activity['age_profile']=='Mid Adult') & (df_high_activity['marital_status'] == 'single'), 'relationship_age_status']= 'Single Mid Adult'
df_high_activity.loc[(df_high_activity['age_profile']=='Mid Adult') & (df_high_activity['marital_status'] == 'living with parents and siblings'), 'relationship_age_status']= 'Single Mid Adult'
df_high_activity.loc[(df_high_activity['age_profile']=='Mid Adult') & (df_high_activity['marital_status']=='married'), 'relationship_age_status']= 'Married Mid Adult'
df_high_activity.loc[(df_high_activity['age_profile']=='Senior') & (df_high_activity['marital_status'] == 'divorced/widowed'), 'relationship_age_status']= 'Single Senior'
df_high_activity.loc[(df_high_activity['age_profile']=='Senior') & (df_high_activity['marital_status'] == 'single'), 'relationship_age_status']= 'Single Senior'
df_high_activity.loc[(df_high_activity['age_profile']=='Senior') & (df_high_activity['marital_status'] == 'living with parents and siblings'), 'relationship_age_status']= 'Single Senior'
df_high_activity.loc[(df_high_activity['age_profile']=='Senior') & (df_high_activity['marital_status']=='married'), 'relationship_age_status']= 'Married Senior'
df_high_activity.loc[(df_high_activity['age_profile']=='Young Adult') & (df_high_activity['marital_status'] == 'divorced/widowed'), 'relationship_age_status']= 'Single Young Adult'
df_high_activity.loc[(df_high_activity['age_profile']=='Young Adult') & (df_high_activity['marital_status'] == 'single'), 'relationship_age_status']= 'Single Young Adult'
df_high_activity.loc[(df_high_activity['age_profile']=='Young Adult') & (df_high_activity['marital_status'] == 'living with parents and siblings'), 'relationship_age_status']= 'Single Young Adult'
df_high_activity.loc[(df_high_activity['age_profile']=='Young Adult') & (df_high_activity['marital_status']=='married'), 'relationship_age_status']= 'Married Young Adult'  
```
 6e. Income Profile. 
```
# creating Income profile
df_high_activity.loc[df_high_activity['income']<=50000, 'income_profile']= 'Low Class'
df_high_activity.loc[(df_high_activity['income']>51000) & (df_high_activity['income']<=150000), 'income_profile']= "Mid Class"
df_high_activity.loc[df_high_activity['income']>=151000, 'income_profile']= 'High Class'
df_high_activity['income_profile'].value_counts()
```
 6f. Gender Profile  
 6g. Deparments Profiles - Alcohol, Baby items, International items
7. Creating an appropriate visualization to show the distribution of profiles  
7a. Age Profile  
7b. Parental Age Profile. 
7c. Single Parent Profile (Dependants Profile). 
7d. Relationship-Age Profile  
7e. Income Profile  
7f. Gender Profile  
7g. Deparments Profiles - Alcohol, Baby items, International items 
 ```# create an alcohol flag, 0=did not buy alcohol, 1=bought alcohol
df_high_activity['alcohol_flag']= [1 if x== 'alcohol' else 0 for x in df_high_activity['department']]
df_high_activity['alcohol_flag']= df_high_activity.groupby('user_id')['alcohol_flag'].transform(np.max)
df_high_activity['alcohol_flag'].value_counts(dropna=False)
 ```
8. Aggregating the **max**, **mean**, and **min** variables on a customer-profile level for usage frequency and expenditure  
8a. Age Profile  
```
# aggregate for age profile expenditure
df_unique_customers.groupby('age_profile').agg({'prices': ['mean', 'min', 'max']})
```
8b. Parental Age Profile  
8c. Single Parent Profile (Dependants Profile)  
8d. Income Profile  
8e. Relationship-Age Profile  
8f. Gender Profile  
8g. Deparments Profiles - Alcohol, Baby items, International items. 
9. Comparison Visualizations customer profiles with regions and departments  
9a. Region Distribution
```
# Total customers across regions
bar_total_customers=df_unique_customers['region'].value_counts(dropna=False).sort_values(ascending=True).plot.barh(color='teal', fontsize=15)
plt.title('Total customers in each region', fontsize=18)
plt.xlabel('Number of customers', fontsize=15)
plt.legend(fontsize=15)
```  
Output  
![This is an image](https://github.com/halinakryvanos/Instacart-Grocery-Basket-Analysis/blob/753442a4e432b02ff9827715d5a981b4968524c9/04%20Analysis/Visualizations/bar_total_customers.png)
9b. Order Frequency Distribution  
9c. Price Range Distribution  
```
# Grouped bar chart of price range products by regions
bar_ppr_region=ppr_region.plot.bar(color=['teal', 'goldenrod', 'darkkhaki'], fontsize=12)
plt.title('Distribution price ranges in US regions', fontsize=20)
plt.legend(ncol=3, fontsize=12)
```
Output  
![This is an image](https://github.com/halinakryvanos/Instacart-Grocery-Basket-Analysis/blob/753442a4e432b02ff9827715d5a981b4968524c9/04%20Analysis/Visualizations/bar_ppr_region.png)  
9d. Distribution Departments by Regions
```
# create crosstab for Departments and Regions
orders_region=pd.crosstab(index=df_unique_customers['department'], columns=df_unique_customers['region'], normalize='index')
dept_region
```
```
# create stacked bar chart for Departments by Regions
dept_region_stacked=dept_region.plot(kind='bar',
                                    stacked=True,
                                    color=['darkseagreen', 'olivedrab', 'teal', 'powderblue', 'bisque', 'orange'],
                                    figsize=(12,10))
plt.title('Distribution of Departments by Region', fontsize = 20)
plt.legend(loc='upper center', ncol=4)
```  
Output  
![This is an image](https://github.com/halinakryvanos/Instacart-Grocery-Basket-Analysis/blob/9fa9b2555ca21d76d65b72a71cd6d2b88bf3dfe2/04%20Analysis/Visualizations/stacked_department_region1.png). 

10. Basic Visualisation  
10a. Departments  
```
# Distribution by Departments
department_bar=df_high_activity['department'].value_counts(dropna=False).sort_values(ascending=True).plot.barh(color=['olivedrab'], fontsize=12)
plt.title('Distribution by Deparments', fontsize=20)
plt.xlabel('The Amount of Products Purchased', fontsize=15)
```  
Output    
![This is an image](https://github.com/halinakryvanos/Instacart-Grocery-Basket-Analysis/blob/9fa9b2555ca21d76d65b72a71cd6d2b88bf3dfe2/04%20Analysis/Visualizations/bar_departments.png)
10b. Product Price Ranges  
``` 
#create pie chart for product price range distribution
price_pie=df_high_activity['price_range_loc'].value_counts().plot.pie(shadow=True,
                                                            startangle=90,
                                                            autopct=('%1.f%%'),
                                                            colors=['steelblue', 'Sienna', 'paleturquoise'],
                                                            label='')
```   
Output  
![This is an image](https://github.com/halinakryvanos/Instacart-Grocery-Basket-Analysis/blob/9fa9b2555ca21d76d65b72a71cd6d2b88bf3dfe2/04%20Analysis/Visualizations/pie_price_range.png)  
10c. Customer Loyalty  
10d. Customer Spending  
10e. Order Frequency  
11. Exporting Final Dataset to_pickle. 

# Other Received pictures during the analysis. 

1. bar_chart_orders_day_of_week. 
2. bar_chart_parental_age_distribution. 
3. bar_chart_parental_age_region_stacked  
![This is an image](https://github.com/halinakryvanos/Instacart-Grocery-Basket-Analysis/blob/e0c8ab2d01168e1d40264d488b54b721e2243d6e/04%20Analysis/Visualizations/bar_parental_age_region_stacked.png)
4. bar_ppr_region  
5. bar_relationship_age_distribution  
6. bar_relationship_single_parent_distribution  
7. bar_total_customers  
8. gender_distribution_pie  
9. histogram_income  
![This is an image](https://github.com/halinakryvanos/Instacart-Grocery-Basket-Analysis/blob/e0c8ab2d01168e1d40264d488b54b721e2243d6e/04%20Analysis/Visualizations/hist_order_hour_of_day.png)
10. hist_prices  
11. income_distribution_hist 
![This is an image](https://github.com/halinakryvanos/Instacart-Grocery-Basket-Analysis/blob/e0c8ab2d01168e1d40264d488b54b721e2243d6e/04%20Analysis/Visualizations/income_distribution_hist.png)  
12. income_distribution_of_customres  
13. income_distribution  
14. line_dow_prices  
![This is an image](https://github.com/halinakryvanos/Instacart-Grocery-Basket-Analysis/blob/e0c8ab2d01168e1d40264d488b54b721e2243d6e/04%20Analysis/Visualizations/line_dow_prices.png)
15. line_fam_status_age  
16. line_hour_of_day  
![This is an image](https://github.com/halinakryvanos/Instacart-Grocery-Basket-Analysis/blob/e0c8ab2d01168e1d40264d488b54b721e2243d6e/04%20Analysis/Visualizations/line_hour_of_day.png)
17. pei_alcohol_distribution  
18. pie_baby_distribution  
19. pie_customer_spending  
20. pie_international_distribution  
21. pie_loyalty_to_brand 
![This is an image](https://github.com/halinakryvanos/Instacart-Grocery-Basket-Analysis/blob/e0c8ab2d01168e1d40264d488b54b721e2243d6e/04%20Analysis/Visualizations/pie_loyalty_to_brand.png)  
22. pie_order_frequency  
23. pie_pet_distribution  
24. price_range_region_stacked  
![This is an image](https://github.com/halinakryvanos/Instacart-Grocery-Basket-Analysis/blob/e0c8ab2d01168e1d40264d488b54b721e2243d6e/04%20Analysis/Visualizations/price_range_region_stacked.png)
25. splot_income  
26. splot_prices  
![This is an image](https://github.com/halinakryvanos/Instacart-Grocery-Basket-Analysis/blob/e0c8ab2d01168e1d40264d488b54b721e2243d6e/04%20Analysis/Visualizations/splot_prices.png)
27. stacked_age_order_freq  
28. stacked_department_region
29. stacked_family_order_freq  
30. stacked_family_stat_price_range  
31. stacked_parental_relationship_region  
![This is an image](https://github.com/halinakryvanos/Instacart-Grocery-Basket-Analysis/blob/e0c8ab2d01168e1d40264d488b54b721e2243d6e/04%20Analysis/Visualizations/stacked_parental_relationship_region.png)  
32. stacked_order_frequency_region  


