# Unit-Economics

TechStream Solutions, a well-established company in the SaaS (Software as a Service) industry, offers a platform called Streamline Pro, designed to provide advanced project management and collaboration tools for businesses of all sizes. Over the years, TechStream Solutions has accumulated a substantial amount of data on their operational costs and revenues. As they continue to grow, the company is now focused on analyzing their unit economics to gain insights into the profitability of Streamline Pro on a per-customer basis, helping them refine their business strategies and ensure sustainable growth.
Data Analyst at TechStream Solutions is to calculate the unit economics for their flagship product, Streamline Pro, focusing on the month of March 2023. The key metrics you'll be analyzing include:
- Customer Acquisition Cost (CAC)
- Average Revenue Per User (ARPU)
- Cost of Goods Sold (COGS)
- Gross Margin
- Customer Lifetime Value (LTV)
- LTV/CAC Ratio
By assessing these metrics, i help the company evaluate the profitability of Streamline Pro, understand the efficiency of customer acquisition strategies, and identify operational costs, aiding in strategic decision-making and future growth.

## **1. Extract Data**

Let's start with importing pandas to our notebook

```
#Your code
import pandas as pd
```

**Loading data by Google Sheets** "customer_lifespan_data"
```
google_sheet_id = '1by8tPHwOnq3uKYK2E7sA9VBUYoPM4p1Rnrm_Ss9cyHI'
url = 'https://docs.google.com/spreadsheets/d/' + google_sheet_id + '/export?format=xlsx'
customer_lifespan = pd.read_excel(url, sheet_name = 'Sheet1')
```
The random 5 rows
Before performing any operation with data, you need to take a look to ensure that everything is okay with the loaded data.
In the cell below, write a code to output the random 5 rows of the dataframe.
```
customer_lifespan.sample(5)
```

**Loading data by Google Sheets** "daily_marketing_spendings"
```
google_sheet_id = '1AZOIThOV4P-0eYDge53ZwumVkfkHoYPWxst3k3Bv87c'
url = 'https://docs.google.com/spreadsheets/d/' + google_sheet_id + '/export?format=xlsx'
daily_marketing = pd.read_excel(url, sheet_name = 'Sheet1')
```
The random 5 rows
Before performing any operation with data, you need to take a look to ensure that everything is okay with the loaded data.
In the cell below, write a code to output the random 5 rows of the dataframe.
```
daily_marketing.sample(5)
```
**Loading data by Google Sheets** "Monthly expenses"
```
google_sheet_id = '10OGbaywwMIqKgnPGy8VDvpBVtjyqln47iYa2lFhI9Mw'
url = 'https://docs.google.com/spreadsheets/d/' + google_sheet_id + '/export?format=xlsx'
Monthly_expenses = pd.read_excel(url, sheet_name = 'Sheet1')
```
The random 5 rows
Before performing any operation with data, you need to take a look to ensure that everything is okay with the loaded data.
In the cell below, write a code to output the random 5 rows of the dataframe.
```
Monthly_expenses.sample(5)
```
**Loading data by Google Sheets** "Payroll"
```
google_sheet_id = '1c_WihqTZCQvNgxzmd-OwhR9i5diwtfxXVLyMn8R-Lp4'
url = 'https://docs.google.com/spreadsheets/d/' + google_sheet_id + '/export?format=xlsx'
Payroll = pd.read_excel(url, sheet_name = 'Sheet1')
```

The random 5 rows
Before performing any operation with data, you need to take a look to ensure that everything is okay with the loaded data.
In the cell below, write a code to output the random 5 rows of the dataframe.
```
Payroll.sample(5)
```
**Loading data by Google Sheets** "receipts_history"
```
google_sheet_id = '1qayqML1zCKdmtzutkcy9LWvE6xFRm6TGBEVkHHJKIuE'
url = 'https://docs.google.com/spreadsheets/d/' + google_sheet_id + '/export?format=xlsx'
receipts_history = pd.read_excel(url, sheet_name = 'Sheet1')
```
The random 5 rows
Before performing any operation with data, you need to take a look to ensure that everything is okay with the loaded data.
In the cell below, write a code to output the random 5 rows of the dataframe.
```
receipts_history.sample(5)
```
## 2. Calculation

### 2.1. CAC Calculation

We will use the amount column of the Monthly Expenses table, the spending column of the daily_marketing table, and the paid column of Payroll to calculate the total sales marketing expenses.

Create a get_last_month function to get columns containing all transactions that took place in March
```
def get_last_month(dataset, data_col):
  last_month = dataset[data_col].max().month
  cond = dataset[data_col].dt.month == last_month
  return dataset[cond]
```

Use the function you just created to get the columns of the expenses table that have transactions that took place in March
```
expenses = get_last_month(Monthly_expenses, 'month')
expenses
```

We will calculate the total amount of Sales force as it relates to the company's marketing.
```
cond = expenses['item'] == 'Salesforce'
crm_cond = expenses[cond]['amount'].values[0]
print(crm_cond)
```

Use the function you just created to get the columns of the daily_marketing table that have transactions that took place in March
```
last_month_marketing = get_last_month(daily_marketing, 'date')
last_month_marketing
```
```
#Sum the values ​​in the spending column
sales_marketing = last_month_marketing['spending'].sum()
sales_marketing
```

Use the function you just created to get the columns of the Payroll table that have transactions that took place in March
```
Sale_payroll = get_last_month(Payroll, 'month')
Sale_payroll
```
Filter out marketing related positions and sum their paid columns.
```
sale_marketing_payroll = ['Sales Manager', 'Sales Associate', 'Content Specialist', 'Marketing Manager']
last_month_sales_marketing_payroll = Sale_payroll[Sale_payroll['position'].isin(sale_marketing_payroll)]['paid'].sum()
last_month_sales_marketing_payroll
```
Filter out new customers in March and calculate total number of customers
```
last_month_customers = get_last_month(receipts_history, 'date')
last_month_customers
```
```
Number_of_customers = last_month_customers['new_customer'].sum()
Number_of_customers
```
```
#Calculate total sales marketing expenses
total_sales_marketing_expenses = crm_cond + sales_marketing + last_month_sales_marketing_payroll
total_sales_marketing_expenses
```
Perform CAC calculation
```
CAC = total_sales_marketing_expenses / Number_of_customers
print(CAC)
```

### 2.2. ARPU Calculation

To perform the ARPU calculation, we must first get the receipt amount for March.
```
last_month_payments_from_new_cus = last_month_customers[last_month_customers['new_customer'] == 1]
last_month_payments_from_new_cus
```
```
total_revenue = last_month_payments_from_new_cus['receipt_amount'].sum()
total_revenue

ARPU = total_revenue / Number_of_customers
print(ARPU)
```

### 2.3. COGS Calculation
```
production_rows = ['AWS Hosting', 'Google Cloud Storage', 'Atlassian Jira']
production_purchases = expenses[expenses['item'].isin(production_rows)]
production_purchases
```
```
shared_rows = ['Slack', 'Zoom']
shared_purchases = expenses[expenses['item'].isin(shared_rows)]
shared_purchases
```
Salary calculation of positions will be related to product
```
production_salaries_df = Sale_payroll[Sale_payroll['department'] == 'Engineering']
production_salaries_df
```

```
production_salaries = production_salaries_df['paid'].sum()
print(production_salaries)

software_n_server_expenses = production_purchases['amount'].sum() + (shared_purchases['amount'].sum() * 0.6)
print(software_n_server_expenses)
```

```
COGS = production_salaries + software_n_server_expenses
print(COGS)
```
### 2.4. Gross Margin Calculation

We will use the last_get_month function to get the customer revenue data in March, then sum them up. After having enough data, start calculating Gross Margin based on the formula

```
def get_last_month(dataset, data_col):
  last_month = dataset[data_col].max().month
  cond = dataset[data_col].dt.month == last_month
  return dataset[cond]
```

```
revenue = last_month_customers['receipt_amount'].sum()
revenue

GrossMargin = (revenue - COGS) / revenue *100
print(GrossMargin)
```

### 2.5. LTV and LTV/CAC Calculation
Process to remove data of customers who have not left the service
```
lifespan_data = customer_lifespan.dropna(subset=['churn_date'])
```
Calculate the number of days each customer remains and calculate the average number of days a customer remain
```
lifespan_data['lifespan_days'] = (customer_lifespan['churn_date'] - customer_lifespan['start_date']).dt.days

avg_lifespan = lifespan_data['lifespan_days'].mean()

avg_lifespan_months = avg_lifespan / 30

print(avg_lifespan_months)
```

```
LTV = ARPU * avg_lifespan_months * GrossMargin
print(LTV)

LTV_CAC = LTV / CAC
print(LTV_CAC)
```

## 3. Conclusion
- CAC is 1213.97 which shows that the business has to spend about 1213.97 USD on marketing to attract each customer.
- ARPU is 274.92 indicating that each customer brings in an average of 274.92 in revenue for the business
- Total cost of production service COGS is 20,264
- Gross Margin is 75.60%, indicating that the business retains approximately 75.60% of its profits from revenue after deducting production costs.
- LTV is $204,529.41, which is the value a customer brings to a business over their lifetime.
- The LTV/CAC Ratio is 168.48, which is very high, indicating that each customer is worth 168 times more than the cost of acquiring them. Demonstrates a very sustainable business and high profits from customers.

The business has a solid financial and capital base, a very high LTV/CAC ratio and a good Gross Margin, showing high efficiency in attracting and retaining customers, while optimizing profits. italicized text





