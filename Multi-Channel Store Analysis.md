# Multi-Channel Store Analysis

### Import neccessary library


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
import os

```

### Getting data (csv file)


```python
# Loading a single month of data for testing
store_data = pd.read_csv("C:\\Users\\nn\\OneDrive\\store data\\US_Regional_Sales_Data.csv")


```


```python
# Explore first few observation of the data
store_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>OrderNumber</th>
      <th>Sales Channel</th>
      <th>WarehouseCode</th>
      <th>ProcuredDate</th>
      <th>OrderDate</th>
      <th>ShipDate</th>
      <th>DeliveryDate</th>
      <th>CurrencyCode</th>
      <th>_SalesTeamID</th>
      <th>_CustomerID</th>
      <th>_StoreID</th>
      <th>_ProductID</th>
      <th>Order Quantity</th>
      <th>Discount Applied</th>
      <th>Unit Price</th>
      <th>Unit Cost</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>SO - 000101</td>
      <td>In-Store</td>
      <td>WARE-UHY1004</td>
      <td>12/31/2017</td>
      <td>5/31/2018</td>
      <td>6/14/2018</td>
      <td>6/19/2018</td>
      <td>USD</td>
      <td>6</td>
      <td>15</td>
      <td>259</td>
      <td>12</td>
      <td>5</td>
      <td>0.075</td>
      <td>1,963.10</td>
      <td>1,001.18</td>
    </tr>
    <tr>
      <th>1</th>
      <td>SO - 000102</td>
      <td>Online</td>
      <td>WARE-NMK1003</td>
      <td>12/31/2017</td>
      <td>5/31/2018</td>
      <td>6/22/2018</td>
      <td>7/2/2018</td>
      <td>USD</td>
      <td>14</td>
      <td>20</td>
      <td>196</td>
      <td>27</td>
      <td>3</td>
      <td>0.075</td>
      <td>3,939.60</td>
      <td>3,348.66</td>
    </tr>
    <tr>
      <th>2</th>
      <td>SO - 000103</td>
      <td>Distributor</td>
      <td>WARE-UHY1004</td>
      <td>12/31/2017</td>
      <td>5/31/2018</td>
      <td>6/21/2018</td>
      <td>7/1/2018</td>
      <td>USD</td>
      <td>21</td>
      <td>16</td>
      <td>213</td>
      <td>16</td>
      <td>1</td>
      <td>0.050</td>
      <td>1,775.50</td>
      <td>781.22</td>
    </tr>
    <tr>
      <th>3</th>
      <td>SO - 000104</td>
      <td>Wholesale</td>
      <td>WARE-NMK1003</td>
      <td>12/31/2017</td>
      <td>5/31/2018</td>
      <td>6/2/2018</td>
      <td>6/7/2018</td>
      <td>USD</td>
      <td>28</td>
      <td>48</td>
      <td>107</td>
      <td>23</td>
      <td>8</td>
      <td>0.075</td>
      <td>2,324.90</td>
      <td>1,464.69</td>
    </tr>
    <tr>
      <th>4</th>
      <td>SO - 000105</td>
      <td>Distributor</td>
      <td>WARE-NMK1003</td>
      <td>4/10/2018</td>
      <td>5/31/2018</td>
      <td>6/16/2018</td>
      <td>6/26/2018</td>
      <td>USD</td>
      <td>22</td>
      <td>49</td>
      <td>111</td>
      <td>26</td>
      <td>8</td>
      <td>0.100</td>
      <td>1,822.40</td>
      <td>1,476.14</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Last few observations
#store_data.tail()
```

### Data Wranggling 
####  1. Inspect data structure, check for null and duplicate values


```python
# Check column names
store_data.columns
```




    Index(['OrderNumber', 'Sales Channel', 'WarehouseCode', 'ProcuredDate',
           'OrderDate', 'ShipDate', 'DeliveryDate', 'CurrencyCode', '_SalesTeamID',
           '_CustomerID', '_StoreID', '_ProductID', 'Order Quantity',
           'Discount Applied', 'Unit Price', 'Unit Cost'],
          dtype='object')




```python
# Checking data types for consistency
store_data.dtypes

store_data.info()

# Need to change inconsistent some data types
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 7991 entries, 0 to 7990
    Data columns (total 16 columns):
     #   Column            Non-Null Count  Dtype  
    ---  ------            --------------  -----  
     0   OrderNumber       7991 non-null   object 
     1   Sales Channel     7991 non-null   object 
     2   WarehouseCode     7991 non-null   object 
     3   ProcuredDate      7991 non-null   object 
     4   OrderDate         7991 non-null   object 
     5   ShipDate          7991 non-null   object 
     6   DeliveryDate      7991 non-null   object 
     7   CurrencyCode      7991 non-null   object 
     8   _SalesTeamID      7991 non-null   int64  
     9   _CustomerID       7991 non-null   int64  
     10  _StoreID          7991 non-null   int64  
     11  _ProductID        7991 non-null   int64  
     12  Order Quantity    7991 non-null   int64  
     13  Discount Applied  7991 non-null   float64
     14  Unit Price        7991 non-null   object 
     15  Unit Cost         7991 non-null   object 
    dtypes: float64(1), int64(5), object(10)
    memory usage: 999.0+ KB
    


```python
# Check for null values
print(store_data.isnull().sum().sum())
```

    0
    


```python
# Count the number of null values
count_nan = len(store_data) - store_data.count()
count_nan.head()

# No null values found
```




    OrderNumber      0
    Sales Channel    0
    WarehouseCode    0
    ProcuredDate     0
    OrderDate        0
    dtype: int64




```python
# Check for duplicates
store_data.duplicated()

# No duplicated values in the dataframe
```




    0       False
    1       False
    2       False
    3       False
    4       False
            ...  
    7986    False
    7987    False
    7988    False
    7989    False
    7990    False
    Length: 7991, dtype: bool



##### The data is pretty much cleaned from source.

##### 2.  Transform Data 
######  Change to apprioprate data type and add columns for analysis


###### a. Convert data types


```python
# Checking data types for consistency
store_data.dtypes

store_data.info()

# Need to change inconsistent some data types
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 7991 entries, 0 to 7990
    Data columns (total 16 columns):
     #   Column            Non-Null Count  Dtype  
    ---  ------            --------------  -----  
     0   OrderNumber       7991 non-null   object 
     1   Sales Channel     7991 non-null   object 
     2   WarehouseCode     7991 non-null   object 
     3   ProcuredDate      7991 non-null   object 
     4   OrderDate         7991 non-null   object 
     5   ShipDate          7991 non-null   object 
     6   DeliveryDate      7991 non-null   object 
     7   CurrencyCode      7991 non-null   object 
     8   _SalesTeamID      7991 non-null   int64  
     9   _CustomerID       7991 non-null   int64  
     10  _StoreID          7991 non-null   int64  
     11  _ProductID        7991 non-null   int64  
     12  Order Quantity    7991 non-null   int64  
     13  Discount Applied  7991 non-null   float64
     14  Unit Price        7991 non-null   object 
     15  Unit Cost         7991 non-null   object 
    dtypes: float64(1), int64(5), object(10)
    memory usage: 999.0+ KB
    


```python
# Change OrderDate, Unit Price and Unit Cost to right data type

store_data.OrderDate = pd.to_datetime(store_data.OrderDate) 

```


```python
# Convert Unit Price data type
store_data['Unit Price']= store_data['Unit Price'].str.replace('[\ \,]', "").astype(float)



```

    C:\Users\nn\AppData\Local\Temp/ipykernel_20916/570773134.py:2: FutureWarning: The default value of regex will change from True to False in a future version.
      store_data['Unit Price']= store_data['Unit Price'].str.replace('[\ \,]', "").astype(float)
    


```python
# Conevrt Unit Cost datatype
store_data['Unit Cost']= store_data['Unit Cost'].str.replace('[\ \,]', "").astype(float)
```

    C:\Users\nn\AppData\Local\Temp/ipykernel_20916/623475655.py:2: FutureWarning: The default value of regex will change from True to False in a future version.
      store_data['Unit Cost']= store_data['Unit Cost'].str.replace('[\ \,]', "").astype(float)
    


```python
# Inspect if right data types are stacked correctly
store_data.dtypes
```




    OrderNumber                 object
    Sales Channel               object
    WarehouseCode               object
    ProcuredDate                object
    OrderDate           datetime64[ns]
    ShipDate                    object
    DeliveryDate                object
    CurrencyCode                object
    _SalesTeamID                 int64
    _CustomerID                  int64
    _StoreID                     int64
    _ProductID                   int64
    Order Quantity               int64
    Discount Applied           float64
    Unit Price                 float64
    Unit Cost                  float64
    dtype: object



###### b. Add columns


```python
# Extract Year from order date
store_data['Year'] = store_data['OrderDate'].dt.year
store_data.Year.value_counts() # counts number of rows
```




    2020    3125
    2019    3030
    2018    1836
    Name: Year, dtype: int64




```python
# Add Month column
store_data['Month'] = store_data['OrderDate'].dt.month
store_data.Month.value_counts()
```




    7     795
    8     791
    12    791
    11    781
    9     745
    10    745
    6     740
    1     569
    5     550
    4     533
    2     502
    3     449
    Name: Month, dtype: int64




```python
# Add day of week column
store_data['day_of_week'] = store_data['OrderDate'].dt.day_name()
store_data.day_of_week.value_counts()
```




    Saturday     1194
    Thursday     1168
    Sunday       1158
    Monday       1146
    Tuesday      1114
    Friday       1112
    Wednesday    1099
    Name: day_of_week, dtype: int64




```python
store_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>OrderNumber</th>
      <th>Sales Channel</th>
      <th>WarehouseCode</th>
      <th>ProcuredDate</th>
      <th>OrderDate</th>
      <th>ShipDate</th>
      <th>DeliveryDate</th>
      <th>CurrencyCode</th>
      <th>_SalesTeamID</th>
      <th>_CustomerID</th>
      <th>_StoreID</th>
      <th>_ProductID</th>
      <th>Order Quantity</th>
      <th>Discount Applied</th>
      <th>Unit Price</th>
      <th>Unit Cost</th>
      <th>Year</th>
      <th>Month</th>
      <th>day_of_week</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>SO - 000101</td>
      <td>In-Store</td>
      <td>WARE-UHY1004</td>
      <td>12/31/2017</td>
      <td>2018-05-31</td>
      <td>6/14/2018</td>
      <td>6/19/2018</td>
      <td>USD</td>
      <td>6</td>
      <td>15</td>
      <td>259</td>
      <td>12</td>
      <td>5</td>
      <td>0.075</td>
      <td>1963.1</td>
      <td>1001.18</td>
      <td>2018</td>
      <td>5</td>
      <td>Thursday</td>
    </tr>
    <tr>
      <th>1</th>
      <td>SO - 000102</td>
      <td>Online</td>
      <td>WARE-NMK1003</td>
      <td>12/31/2017</td>
      <td>2018-05-31</td>
      <td>6/22/2018</td>
      <td>7/2/2018</td>
      <td>USD</td>
      <td>14</td>
      <td>20</td>
      <td>196</td>
      <td>27</td>
      <td>3</td>
      <td>0.075</td>
      <td>3939.6</td>
      <td>3348.66</td>
      <td>2018</td>
      <td>5</td>
      <td>Thursday</td>
    </tr>
    <tr>
      <th>2</th>
      <td>SO - 000103</td>
      <td>Distributor</td>
      <td>WARE-UHY1004</td>
      <td>12/31/2017</td>
      <td>2018-05-31</td>
      <td>6/21/2018</td>
      <td>7/1/2018</td>
      <td>USD</td>
      <td>21</td>
      <td>16</td>
      <td>213</td>
      <td>16</td>
      <td>1</td>
      <td>0.050</td>
      <td>1775.5</td>
      <td>781.22</td>
      <td>2018</td>
      <td>5</td>
      <td>Thursday</td>
    </tr>
    <tr>
      <th>3</th>
      <td>SO - 000104</td>
      <td>Wholesale</td>
      <td>WARE-NMK1003</td>
      <td>12/31/2017</td>
      <td>2018-05-31</td>
      <td>6/2/2018</td>
      <td>6/7/2018</td>
      <td>USD</td>
      <td>28</td>
      <td>48</td>
      <td>107</td>
      <td>23</td>
      <td>8</td>
      <td>0.075</td>
      <td>2324.9</td>
      <td>1464.69</td>
      <td>2018</td>
      <td>5</td>
      <td>Thursday</td>
    </tr>
    <tr>
      <th>4</th>
      <td>SO - 000105</td>
      <td>Distributor</td>
      <td>WARE-NMK1003</td>
      <td>4/10/2018</td>
      <td>2018-05-31</td>
      <td>6/16/2018</td>
      <td>6/26/2018</td>
      <td>USD</td>
      <td>22</td>
      <td>49</td>
      <td>111</td>
      <td>26</td>
      <td>8</td>
      <td>0.100</td>
      <td>1822.4</td>
      <td>1476.14</td>
      <td>2018</td>
      <td>5</td>
      <td>Thursday</td>
    </tr>
  </tbody>
</table>
</div>



#### Add Profit column


```python
# First calculate Total Revenue
store_data['Total Revenue'] = store_data['Order Quantity'] * store_data['Unit Price'] -  store_data['Order Quantity'] * store_data['Unit Price'] * store_data['Discount Applied']

```


```python
# Second calculate Total Cost
store_data['Total Cost'] = store_data['Order Quantity'] * store_data['Unit Cost'] 

```


```python
# Now calculate Total Profit
store_data['Total Profit'] = store_data['Total Revenue'] - store_data['Total Cost']


```

# Solving Business Problems

### 1. What is the distribution of sales channels by  store type?


```python
# Number of rows by each channel type. The number of times customers ordered from each store
store_data['Sales Channel'].value_counts()
```




    In-Store       3298
    Online         2425
    Distributor    1375
    Wholesale       893
    Name: Sales Channel, dtype: int64




```python


```


```python
# Import additional tools
from itertools import cycle, islice

channel_group = store_data.groupby('Sales Channel')
quantity_ordered = channel_group.sum()['Order Quantity']

channels = [channel for channel, df in channel_group]

my_colors = list(islice(cycle(['y', 'g', 'b', 'r']), None, len(store_data)))


plt.bar(channels, quantity_ordered,  width = 0.5, color = my_colors)
plt.ylabel('Order Quantity')
plt.xlabel('Sales Channel')
plt.xticks(channels, size=12)
plt.title('Distribution Channels by Orders')
plt.show()
```


    
![png](output_33_0.png)
    



```python
channel_group = store_data.groupby('Sales Channel')
total_profit = channel_group.sum()['Total Profit']

channels = [channel for channel, df in channel_group]


store_data = store_data.sort_values(by=['Total Profit'])
plt.bar(channels, total_profit, width = 0.45)
plt.ylabel('Total Profit')
plt.xlabel('Sales Channel')
plt.xticks(channels, size=12)
plt.title('Profit from Sales Channel')
plt.show()
```


    
![png](output_34_0.png)
    


#### Exploring trends in order quantity

##### By Year


```python
years = [year for year, df in store_data.groupby('Year')]

plt.plot(years, store_data.groupby(['Year']).count(), color = 'purple')
plt.xticks(years )
plt.xlabel('Years')
plt.ylabel('Total Profit')
plt.title('Profit per Year')
plt.show()
```


    
![png](output_37_0.png)
    


##### By Months


```python
monthly_orders = store_data.groupby('Month').sum()

months = range(1, 13)

# Month_Name = list(Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec)

plt.bar(months, monthly_orders['Order Quantity'])
plt.xticks(months)
plt.ylabel('Order Quantity')
plt.xlabel('Month')
plt.title('Orders per Month')
plt.show()

```


    
![png](output_39_0.png)
    


##### By Day of the week


```python
weekday = store_data.groupby('day_of_week').sum()

days = [day for day, df in store_data.groupby('day_of_week')]


plt.bar(days, weekday['Order Quantity'], color = 'b')
plt.xticks(days, rotation='vertical',  size=12)
plt.ylabel('Orders')
plt.xlabel('Day Name')
plt.show()
```


    
![png](output_41_0.png)
    



```python
store_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>OrderNumber</th>
      <th>Sales Channel</th>
      <th>WarehouseCode</th>
      <th>ProcuredDate</th>
      <th>OrderDate</th>
      <th>ShipDate</th>
      <th>DeliveryDate</th>
      <th>CurrencyCode</th>
      <th>_SalesTeamID</th>
      <th>_CustomerID</th>
      <th>...</th>
      <th>Order Quantity</th>
      <th>Discount Applied</th>
      <th>Unit Price</th>
      <th>Unit Cost</th>
      <th>Year</th>
      <th>Month</th>
      <th>day_of_week</th>
      <th>Total Revenue</th>
      <th>Total Cost</th>
      <th>Total Profit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2617</th>
      <td>SO - 0002718</td>
      <td>In-Store</td>
      <td>WARE-PUJ1005</td>
      <td>2/4/2019</td>
      <td>2019-04-05</td>
      <td>4/17/2019</td>
      <td>4/19/2019</td>
      <td>USD</td>
      <td>2</td>
      <td>18</td>
      <td>...</td>
      <td>8</td>
      <td>0.4</td>
      <td>6083.6</td>
      <td>5171.06</td>
      <td>2019</td>
      <td>4</td>
      <td>Friday</td>
      <td>29201.28</td>
      <td>41368.48</td>
      <td>-12167.20</td>
    </tr>
    <tr>
      <th>7968</th>
      <td>SO - 0008069</td>
      <td>Wholesale</td>
      <td>WARE-XYS1001</td>
      <td>9/26/2020</td>
      <td>2020-12-28</td>
      <td>1/5/2021</td>
      <td>1/7/2021</td>
      <td>USD</td>
      <td>28</td>
      <td>16</td>
      <td>...</td>
      <td>8</td>
      <td>0.4</td>
      <td>6505.7</td>
      <td>5139.50</td>
      <td>2020</td>
      <td>12</td>
      <td>Monday</td>
      <td>31227.36</td>
      <td>41116.00</td>
      <td>-9888.64</td>
    </tr>
    <tr>
      <th>7376</th>
      <td>SO - 0007477</td>
      <td>Online</td>
      <td>WARE-MKL1006</td>
      <td>6/18/2020</td>
      <td>2020-10-21</td>
      <td>11/3/2020</td>
      <td>11/11/2020</td>
      <td>USD</td>
      <td>13</td>
      <td>2</td>
      <td>...</td>
      <td>7</td>
      <td>0.4</td>
      <td>5916.1</td>
      <td>4673.72</td>
      <td>2020</td>
      <td>10</td>
      <td>Wednesday</td>
      <td>24847.62</td>
      <td>32716.04</td>
      <td>-7868.42</td>
    </tr>
    <tr>
      <th>7636</th>
      <td>SO - 0007737</td>
      <td>In-Store</td>
      <td>WARE-UHY1004</td>
      <td>9/26/2020</td>
      <td>2020-11-17</td>
      <td>11/23/2020</td>
      <td>11/25/2020</td>
      <td>USD</td>
      <td>2</td>
      <td>8</td>
      <td>...</td>
      <td>8</td>
      <td>0.4</td>
      <td>3685.0</td>
      <td>3095.40</td>
      <td>2020</td>
      <td>11</td>
      <td>Tuesday</td>
      <td>17688.00</td>
      <td>24763.20</td>
      <td>-7075.20</td>
    </tr>
    <tr>
      <th>6834</th>
      <td>SO - 0006935</td>
      <td>In-Store</td>
      <td>WARE-UHY1004</td>
      <td>6/18/2020</td>
      <td>2020-08-18</td>
      <td>8/26/2020</td>
      <td>9/1/2020</td>
      <td>USD</td>
      <td>10</td>
      <td>35</td>
      <td>...</td>
      <td>7</td>
      <td>0.4</td>
      <td>6492.3</td>
      <td>4869.23</td>
      <td>2020</td>
      <td>8</td>
      <td>Tuesday</td>
      <td>27267.66</td>
      <td>34084.61</td>
      <td>-6816.95</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>




```python
df = store_data[store_data['_CustomerID'].duplicated(keep=False)]
df['Grouped'] = df.groupby('_CustomerID')['Sales Channel'].transform(lambda x: ','.join(x))
df = df[['_CustomerID', 'Grouped']].drop_duplicates()
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>_CustomerID</th>
      <th>Grouped</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2617</th>
      <td>18</td>
      <td>In-Store,In-Store,Wholesale,In-Store,In-Store,...</td>
    </tr>
    <tr>
      <th>7968</th>
      <td>16</td>
      <td>Wholesale,Online,Distributor,In-Store,Online,D...</td>
    </tr>
    <tr>
      <th>7376</th>
      <td>2</td>
      <td>Online,Wholesale,Online,Online,In-Store,Distri...</td>
    </tr>
    <tr>
      <th>7636</th>
      <td>8</td>
      <td>In-Store,In-Store,In-Store,Wholesale,Distribut...</td>
    </tr>
    <tr>
      <th>6834</th>
      <td>35</td>
      <td>In-Store,Online,Online,Online,Online,In-Store,...</td>
    </tr>
  </tbody>
</table>
</div>




```python
from itertools import combinations
from collections import Counter

count = Counter()

for row in df['Grouped']:
    row_list = row.split(',')
    count.update(Counter(combinations(row_list, 3)))

for key, value in count.most_common(10):
    print(key, value)
```

    ('In-Store', 'In-Store', 'In-Store') 2432582
    ('In-Store', 'Online', 'In-Store') 1817138
    ('Online', 'In-Store', 'In-Store') 1784786
    ('In-Store', 'In-Store', 'Online') 1730041
    ('Online', 'Online', 'In-Store') 1326672
    ('In-Store', 'Online', 'Online') 1283680
    ('Online', 'In-Store', 'Online') 1270143
    ('In-Store', 'In-Store', 'Distributor') 1038298
    ('In-Store', 'Distributor', 'In-Store') 1005624
    ('Distributor', 'In-Store', 'In-Store') 1005353
    
