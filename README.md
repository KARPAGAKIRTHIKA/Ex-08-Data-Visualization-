# Ex-07-Data-Visualization-

## AIM
To Perform Data Visualization on the given dataset and save the data to a file. 

# Explanation
Data visualization is the graphical representation of information and data. By using visual elements like charts, graphs, and maps, data visualization tools provide an accessible way to see and understand trends, outliers, and patterns in data.

# ALGORITHM
### STEP 1
Read the given Data
### STEP 2
Clean the Data Set using Data Cleaning Process
### STEP 3
Apply Feature generation and selection techniques to all the features of the data set
### STEP 4
Apply data visualization techniques to identify the patterns of the data.


# CODE
```
Developed by: Karpagakirthika.V
Reg No: 212221220025
```

Data Pre-Processing
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
df=pd.read_csv("Superstore.csv",encoding='unicode_escape')
df
df.head()
df.info()
df.drop('Row ID',axis=1,inplace=True)
df.drop('Order ID',axis=1,inplace=True)
df.drop('Customer ID',axis=1,inplace=True)
df.drop('Customer Name',axis=1,inplace=True)
df.drop('Country',axis=1,inplace=True)
df.drop('Postal Code',axis=1,inplace=True)
df.drop('Product ID',axis=1,inplace=True)
df.drop('Product Name',axis=1,inplace=True)
df.drop('Order Date',axis=1,inplace=True)
df.drop('Ship Date',axis=1,inplace=True)
print("Updated dataset")
df
df.isnull().sum()
#detecting and removing outliers in current numeric data
plt.figure(figsize=(8,8))
plt.title("Data with outliers")
df.boxplot()
plt.show()
plt.figure(figsize=(8,8))
cols = ['Sales','Quantity','Discount','Profit']
Q1 = df[cols].quantile(0.25)
Q3 = df[cols].quantile(0.75)
IQR = Q3 - Q1
df = df[~((df[cols] < (Q1 - 1.5 * IQR)) |(df[cols] > (Q3 + 1.5 * IQR))).any(axis=1)]
plt.title("Dataset after removing outliers")
df.boxplot()
plt.show()
```
Which Segment has Highest sales?
```
sns.lineplot(x="Segment",y="Sales",data=df,marker='o')
plt.title("Segment vs Sales")
plt.xticks(rotation = 90)
plt.show()
sns.barplot(x="Segment",y="Sales",data=df)
plt.xticks(rotation = 90)
plt.show()
```
Which City has Highest profit?
```
df.shape
df1 = df[(df.Profit >= 60)]
df1.shape
plt.figure(figsize=(30,8))
states=df1.loc[:,["City","Profit"]]
states=states.groupby(by=["City"]).sum().sort_values(by="Profit")
sns.barplot(x=states.index,y="Profit",data=states)
plt.xticks(rotation = 90)
plt.xlabel=("City")
plt.ylabel=("Profit")
plt.show()
```
Which ship mode is profitable?
```
sns.barplot(x="Ship Mode",y="Profit",data=df)
plt.show()
sns.lineplot(x="Ship Mode",y="Profit",data=df)
plt.show()
sns.violinplot(x="Profit",y="Ship Mode",data=df)
sns.pointplot(x=df["Profit"],y=df["Ship Mode"])
Sales of the product based on region.
states=df.loc[:,["Region","Sales"]]
states=states.groupby(by=["Region"]).sum().sort_values(by="Sales")
sns.barplot(x=states.index,y="Sales",data=states)
plt.xticks(rotation = 90)
plt.xlabel=("Region")
plt.ylabel=("Sales")
plt.show()
df.groupby(['Region']).sum().plot(kind='pie', y='Sales',figsize=(6,9),pctdistance=1.7,labeldistance=1.2)
```
Sales of the product based on region.
```
states=df.loc[:,["Region","Sales"]]
states=states.groupby(by=["Region"]).sum().sort_values(by="Sales")
sns.barplot(x=states.index,y="Sales",data=states)
plt.xticks(rotation = 90)
plt.xlabel=("Region")
plt.ylabel=("Sales")
plt.show()
df.groupby(['Region']).sum().plot(kind='pie', y='Sales',figsize=(6,9),pctdistance=1.7,labeldistance=1.2)
```
Find the relation between sales and profit.
```
df["Sales"].corr(df["Profit"])
df_corr = df.copy()
df_corr = df_corr[["Sales","Profit"]]
df_corr.corr()
sns.pairplot(df_corr, kind="scatter")
plt.show()
Heatmap
df4=df.copy()

#encoding
from sklearn.preprocessing import LabelEncoder,OrdinalEncoder,OneHotEncoder
le=LabelEncoder()
ohe=OneHotEncoder
oe=OrdinalEncoder()

df4["Ship Mode"]=oe.fit_transform(df[["Ship Mode"]])
df4["Segment"]=oe.fit_transform(df[["Segment"]])
df4["City"]=le.fit_transform(df[["City"]])
df4["State"]=le.fit_transform(df[["State"]])
df4['Region'] = oe.fit_transform(df[['Region']])
df4["Category"]=oe.fit_transform(df[["Category"]])
df4["Sub-Category"]=le.fit_transform(df[["Sub-Category"]])

#scaling
from sklearn.preprocessing import RobustScaler
sc=RobustScaler()
df5=pd.DataFrame(sc.fit_transform(df4),columns=['Ship Mode', 'Segment', 'City', 'State','Region',
                                               'Category','Sub-Category','Sales','Quantity','Discount','Profit'])
```
# Heatmap
```
plt.subplots(figsize=(12,7))
sns.heatmap(df5.corr(),cmap="PuBu",annot=True)
plt.show()
Find the relation between sales and profit based on the following category.
```
Segment
```
grouped_data = df.groupby('Segment')[['Sales', 'Profit']].mean()
# Create a bar chart of the grouped data
fig, ax = plt.subplots()
ax.bar(grouped_data.index, grouped_data['Sales'], label='Sales')
ax.bar(grouped_data.index, grouped_data['Profit'], bottom=grouped_data['Sales'], label='Profit')
ax.set_xlabel('Segment')
ax.set_ylabel('Value')
ax.legend()
plt.show()
```
City
```
grouped_data = df.groupby('City')[['Sales', 'Profit']].mean()
# Create a bar chart of the grouped data
fig, ax = plt.subplots()
ax.bar(grouped_data.index, grouped_data['Sales'], label='Sales')
ax.bar(grouped_data.index, grouped_data['Profit'], bottom=grouped_data['Sales'], label='Profit')
ax.set_xlabel('City')
ax.set_ylabel('Value')
ax.legend()
plt.show()
```
States
```
grouped_data = df.groupby('State')[['Sales', 'Profit']].mean()
# Create a bar chart of the grouped data
fig, ax = plt.subplots()
ax.bar(grouped_data.index, grouped_data['Sales'], label='Sales')
ax.bar(grouped_data.index, grouped_data['Profit'], bottom=grouped_data['Sales'], label='Profit')
ax.set_xlabel('State')
ax.set_ylabel('Value')
ax.legend()
plt.show()
```
Segment and Ship Mode
```
grouped_data = df.groupby(['Segment', 'Ship Mode'])[['Sales', 'Profit']].mean()
pivot_data = grouped_data.reset_index().pivot(index='Segment', columns='Ship Mode', values=['Sales', 'Profit'])
# Create a bar chart of the grouped data
fig, ax = plt.subplots()
pivot_data.plot(kind='bar', ax=ax)
ax.set_xlabel('Segment')
ax.set_ylabel('Value')
plt.legend(title='Ship Mode')
plt.show()
```
Segment, Ship mode and Region
```
grouped_data = df.groupby(['Segment', 'Ship Mode','Region'])[['Sales', 'Profit']].mean()
pivot_data = grouped_data.reset_index().pivot(index=['Segment', 'Ship Mode'], columns='Region', values=['Sales', 'Profit'])
sns.set_style("whitegrid")
sns.set_palette("Set1")
pivot_data.plot(kind='bar', stacked=True, figsize=(10, 5))
plt.xlabel('Segment - Ship Mode')
plt.ylabel('Value')
plt.legend(title='Region')
plt.show()
```
# OUPUT
Data Pre-Processing

![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/d672583f-3ab8-438e-8e80-a9d4db580957)

![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/37cfc22b-2efe-46e9-96cf-6e1145b71a6e)

Which Segment has Highest sales?

![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/956ad2a3-4ec0-4e5c-9324-2f3c751e496d)

![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/7d1f30b2-dc4b-4638-80bd-b0b21a2760fd)

Sales of the product based on region.

![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/0d39702d-287a-4ab6-bf87-9b17153a7dbc)
![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/33f7dd26-7644-4a89-a59d-a8872250fd57)

Which City has Highest profit?

![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/a4230ac6-3788-449b-ad99-34d8faeb0aa7)

Which ship mode is profitable?

![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/10e945d4-2bc8-4776-be03-dbceed0f4688)
![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/1a8fd38a-1fe5-4780-ac9a-9037544e646c)



Sales of the product based on region.

![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/6ee8d1b1-20b0-47e8-8282-8313c882fbf1)
![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/bb5f39de-8098-4e4c-affe-686e5aca6cc1)


Find the relation between sales and profit

![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/03e1d598-b352-4df2-9ec5-a1123e33e182)


Heatmap
![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/7ffd8f89-f9a0-4437-935f-a5610ad89f95)


Find the relation between sales and profit based on the following category.
Segment

![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/4196db9b-4306-4e94-ad2f-8e85ab11c7ad)


City

![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/c103fd87-878b-49d0-9ec6-a91a36456d15)

States

![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/496db89c-d8c0-4611-9db2-106d1265861c)


Segment and Ship Mode

![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/bf59f756-b409-4468-b800-cd8fe19fbde7)


Segment, Ship mode and Region

![image](https://github.com/KARPAGAKIRTHIKA/Ex-08-Data-Visualization-/assets/103020162/06241563-443b-4e75-af59-50e9b3b679d4)


# Result:
Thus, Data Visualization is performed on the given dataset and save the data to a file.

# Inferences

Which Segment has Highest sales?
Consumer Segment has the highest sales

Which City has Highest profit?
New York City has the Highest Profit

Which ship mode is profitable?
First Class Ship Mode is most profitable

Sales of the product based on region.
West region has the most sales

Find the relation between sales and profit
Sales is not much related to profit

Find the relation between sales and profit based on the following category.
Segment
Profit is much related to Segment than Sales

City
Profit is much related to City than Sales

States
Sales is much related to City than Profit

Segment and Ship Mode
Ship mode is more related to Sales than Profit.

Segment, Ship mode and Region
Region is more related to Profit than Sales.
