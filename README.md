# Blinkit-Item-Analysis-Project
Using Blinket Sales Data found out insights from Data Using Python Libraries
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("/content/blinkit_data.csv")
df.head()
df.info()
df.shape
print("Size of Data:", df.shape)
df.dtypes
df['Item Fat Content'] = df['Item Fat Content'].replace({'LF': 'Low Fat', 'low fat': 'Low Fat','reg': 'Regular'})
print(df['Item Fat Content'].unique())
print(df['Item Fat Content'].unique())

# Total Sales
total_sales = df['Sales'].sum()
# Avg Sales
avg_sales = df['Sales'].mean()
# Number of Items Sold
no_of_items_sold = df['Sales'].count()
# Average Ratings
avg_ratings = df['Rating'].mean()
print(f"Total Sales: ${total_sales:,.1f}")  # here , is thousand separator and .1f shows decimal up to 1.
print(f"Average Sales:${avg_sales:,.0f}")
print(f"No of Items sold:{no_of_items_sold:,.0f}")
print(f"Average Ratings:{avg_ratings :,.0f}")

sales_by_fat = df.groupby('Item Fat Content')['Sales'].sum()
plt.pie(sales_by_fat, labels =sales_by_fat.index, autopct ='%.0f%%',startangle =90)
plt.title('Sales by Fat Content')
plt.axis('equal')
plt.show()

Total Sales by Item Type
sales_by_item_type = df.groupby('Item Type')['Sales'].sum().sort_values(ascending = False)
plt.figure(figsize=(10,5))
bars = plt.bar(sales_by_item_type.index, sales_by_item_type.values) 
plt.xticks(rotation =-90)  
plt.xlabel('Item Type')
plt.ylabel('Total Sales')
plt.title('Total Sales by item type')
for bar in bars:
  plt.text(bar.get_x()+ bar.get_width()/2, bar.get_height(), f'{bar.get_height():,.0f}', ha ='center', va = 'bottom', fontsize = 8)
plt.tight_layout()
plt.show()

Fat Content by Outlets for Total Sales.
grouped = df.groupby(['Item Fat Content', 'Outlet Location Type'])['Sales'].sum().unstack()
ax= grouped.plot(kind ='bar', figsize=(8,5), title = 'Outlet Tier by Item Fat Content')
plt.xlabel('Outlet location Tier')
plt.ylabel("Total Sales")
plt.legend(title = "Item Fat Content")
plt.tight_layout()
plt.show()

 Total Sales by Outlet Estabilishments
sales_by_year = df.groupby('Outlet Establishment Year')['Sales'].sum().sort_index()
plt.figure(figsize=(10,5))
plt.plot(sales_by_year.index, sales_by_year.values, marker ='o', linestyle ='-')
plt.xlabel('Outlet Estabilishment Year')
plt.ylabel('Total Sales')
plt.title('Outlet Estabilishment')
for x,y in zip(sales_by_year.index, sales_by_year.values):
  plt.text(x,y,f'{y:,.0f}',ha ='center',va= 'bottom',fontsize =8)
plt.tight_layout()
plt.show() 

 Total Sales by Outlet Size
 sales_by_size = df.groupby('Outlet Size')['Sales'].sum()
plt.pie(sales_by_size, labels= sales_by_size.index, autopct = '%.0f%%',startangle =90)
plt.title('Outlet Size')
plt.tight_layout()
plt.show()

 Sales by Outlet location
 sales_by_location = df.groupby('Outlet Location Type')['Sales'].sum().reset_index() 
sales_by_location = sales_by_location.sort_values('Sales', ascending = True)
plt.figure(figsize=(8,5))
ax = sns.barplot(x = 'Outlet Location Type', y= 'Sales', data = sales_by_location)
plt.title('Total Sales by Outlet Location Type')
plt.xlabel('Total Sales')
plt.ylabel('Outlet Location Type')
plt.tight_layout()
plt.show()
