# Project-2-Recency-Frequency-and-Monetary-Analysis
Project 2: Recency, Frequency and Monetary Analysis

#performing the analysis using Jupiter Notebook
```python
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np

testrfm = pd.read_csv (r"C:\Users\SIMON.NGOTHO\Downloads\rfm_data.csv") #data downloaded from Statso

print (testrfm)

from datetime import datetime 

#Calculating the RFM values
testrfm['PurchaseDate'] = pd.to_datetime(testrfm['PurchaseDate']) #convert to readable date formart

todays_date = datetime(2023,10,16) #todays date

print (testrfm['PurchaseDate'])

#Recency
testrfm ['recency'] = (todays_date - testrfm['PurchaseDate']).dt.days 

#Frequency

frequency = testrfm.groupby('CustomerID')['PurchaseDate'].count().reset_index()
frequency.rename(columns={'PurchaseDate':'frequency'}, inplace=True) #rename column to frequency

#Monetary

monetary = testrfm.groupby('CustomerID')['TransactionAmount'].sum().reset_index()
monetary.rename(columns={'TransactionAmount':'monetary'}, inplace=True) #rename column to monetary

print (monetary)

#combining the three into one dataframe

rfm = pd.merge(frequency, monetary, on='CustomerID')
rfm2 = pd.merge( rfm, testrfm [['recency','CustomerID']], on ='CustomerID')

print (rfm2)

#visualizing the RFM

#Creating histogram

plt.figure(figsize=(10, 10)) #define width and height size

plt.subplot(221) 
plt.hist(rfm2['recency'], bins=50, edgecolor='brown', alpha=0.9, color='yellow')
plt.xlabel('recency')
plt.title('LuxTech Recency')
plt.grid()

plt.subplot(222)
plt.hist(rfm2['frequency'], bins=25, edgecolor='orange', alpha=0.9, color='black')
plt.xlabel('frequency')
plt.title('LuxTech Requency')
plt.grid()

plt.subplot(223)
plt.hist(rfm2['monetary'], bins=50, edgecolor='pink', alpha=0.9, color='blue')
plt.xlabel('monetary')
plt.title('LuxTech Monetary')
plt.grid()

plt.show()
```
