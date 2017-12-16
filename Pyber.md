In the city type 'Rural', since there are not many drivers the avg fares are pretty higher than usual 
In the City Type 'Urban', since theere are a lot of drivers in majority of the cities the avergae fare are lower 
In the City Type 'Urban', there are a lot of people that are using Pyber since the number of Rides ( for majority of cities ) are on the higher end 

```python
# Dependencies
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import seaborn as sns




```


```python
#reading the CSV file 
df = pd.read_csv('/Users/pooja/Documents/ride_data.csv')
df1 = pd.read_csv('/Users/pooja/Documents/city_data.csv')

temp=df.groupby(['city'])['ride_id'].count()
temp1=df.groupby(['city'])['fare'].sum()

result=temp.to_frame().join(temp1.to_frame()).reset_index()

result['Avg_per_city']= result['fare']/result['ride_id']

#result= result.reset_index

#result.head()

result1 = result.merge(df1, on='city')

result1.head()

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>ride_id</th>
      <th>fare</th>
      <th>Avg_per_city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Adamschester</td>
      <td>9</td>
      <td>266.35</td>
      <td>29.594444</td>
      <td>27</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alexisfort</td>
      <td>33</td>
      <td>903.11</td>
      <td>27.366970</td>
      <td>24</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Amberberg</td>
      <td>16</td>
      <td>457.99</td>
      <td>28.624375</td>
      <td>13</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Anthonyfurt</td>
      <td>17</td>
      <td>501.35</td>
      <td>29.491176</td>
      <td>17</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Boyleberg</td>
      <td>5</td>
      <td>161.98</td>
      <td>32.396000</td>
      <td>13</td>
      <td>Suburban</td>
    </tr>
  </tbody>
</table>
</div>




```python
result_Urban=result1.loc[result1['type'] == 'Urban']
#print(result_Urban.head())

result_Suburban=result1.loc[result1['type'] == 'Suburban']
#print(result_Suburban.head())

result_Rural=result1.loc[result1['type'] == 'Rural']
#print(result_Rural.head())



```


```python
ax=result_Urban.plot.scatter(x='ride_id', y='Avg_per_city', s=10*result1.driver_count, color='coral', label='Urban',alpha=0.7, edgecolor='black',linewidth=2.0)

result_Rural.plot.scatter(x='ride_id', y='Avg_per_city', s=10*result1.driver_count, color='gold',ax=ax,label='Rural',alpha=0.7, edgecolor='black',linewidth=2.0)

result_Suburban.plot.scatter(x='ride_id', y='Avg_per_city', s=10*result1.driver_count, color='skyblue',ax=ax,label='Suburban',alpha=0.7, edgecolor='black',linewidth=2.0)


fig_size = plt.rcParams["figure.figsize"]

fig_size[0] = 15
fig_size[1] = 9
plt.rcParams["figure.figsize"] = fig_size

plt.xlabel("Total Number of Rides(per city)")
plt.ylabel("Average Fare($)")
plt.title("Pyber Ride Sharing Data")
ax.legend(loc='upper right', borderpad=2,labelspacing=2, title='Type of City')

plt.grid(True,color='black')
plt.show()
print("Note: The size of the bubble corelates to the number of drivers per city")





```


![png](Pyber_files/Pyber_3_0.png)


    Note: The size of the bubble corelates to the number of drivers per city



```python
#result1.plot.scatter(x='ride_id', y='Avg_per_city', s=5*result1.driver_count)


```


```python
Sum_fare = result1['fare'].sum()
city_type_fare= result1.groupby(['type'])['fare'].sum()



#print(Sum_fare)

Avg_fare=((city_type_fare/Sum_fare)*100).round(decimals=2)


print(Avg_fare)


```

    type
    Rural        6.76
    Suburban    29.74
    Urban       63.49
    Name: fare, dtype: float64



```python
labels =['Rural','Suburban','Urban']

colors = ['gold', 'skyblue','coral']
explode = (0, 0, 0.05) 
Avg_fare.plot.pie(colors=colors,explode=explode, labels=labels,autopct='%1.1f%%', shadow=True, startangle=140)

fig_size1 = plt.rcParams["figure.figsize"]
fig_size1[0] = 6
fig_size1[1] = 6
plt.rcParams["figure.figsize"] = fig_size1


plt.title("Percentage of Shares by City Type")
plt.tight_layout()

#plt.axes().set_ylabel('')

#axis1=plt.axes()

#axis1.get_yaxis().set_visible(False)


plt.show()



```


![png](Pyber_files/Pyber_6_0.png)



```python
Sum_rides = result1['ride_id'].sum()
city_type_rides= result1.groupby(['type'])['ride_id'].sum()
Avg_rides=((city_type_rides/Sum_rides)*100).round(decimals=2)


#print(Avg_rides)

labels =['Rural','Suburban','Urban']

colors = ['gold', 'skyblue','coral']
explode = (0, 0, 0.05) 
Avg_rides.plot.pie(colors=colors,explode=explode, labels=labels,autopct='%1.1f%%', shadow=True, startangle=140)
fig_size2 = plt.rcParams["figure.figsize"]
fig_size2[0] = 6
fig_size2[1] = 6
plt.rcParams["figure.figsize"] = fig_size2


plt.title("% of rides by City Type")
plt.tight_layout()
#plt.axes().set_ylabel('')

plt.show()



```


![png](Pyber_files/Pyber_7_0.png)



```python
Sum_drivers = result1['driver_count'].sum()
city_type_drivers= result1.groupby(['type'])['driver_count'].sum()
Avg_drivers=((city_type_drivers/Sum_drivers)*100).round(decimals=2)


#print(Avg_rides)

labels =['Rural','Suburban','Urban']

colors = ['gold', 'skyblue','coral']
explode = (0, 0, 0.05) 
Avg_drivers.plot.pie(colors=colors,explode=explode, labels=labels,autopct='%1.1f%%', shadow=True, startangle=140)
fig_size3 = plt.rcParams["figure.figsize"]
fig_size3[0] = 6
fig_size3[1] = 6
plt.rcParams["figure.figsize"] = fig_size3


plt.title("% of drivers by City Type")
plt.tight_layout()
plt.axes().set_ylabel('')
plt.show()


```

    /Users/pooja/anaconda3/envs/PythonData/lib/python3.6/site-packages/matplotlib/cbook/deprecation.py:106: MatplotlibDeprecationWarning: Adding an axes using the same arguments as a previous axes currently reuses the earlier instance.  In a future version, a new instance will always be created and returned.  Meanwhile, this warning can be suppressed, and the future behavior ensured, by passing a unique label to each axes instance.
      warnings.warn(message, mplDeprecation, stacklevel=1)



![png](Pyber_files/Pyber_8_1.png)

