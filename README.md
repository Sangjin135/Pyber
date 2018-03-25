

```python
#import dependencies
import matplotlib.pyplot as mlp
import pandas as pd
```


```python
#read data
city = pd.read_csv("raw_data/city_data.csv")
rides = pd.read_csv("raw_data/ride_data.csv")
city_rides = pd.DataFrame(city)
ride_data = pd.DataFrame(rides)
```


```python
#merge data and ensure no dupes
merged_df = pd.merge(ride_data, city_rides, on="city")
merged = merged_df.drop_duplicates(subset = "ride_id")
merged.head()
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
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sarabury</td>
      <td>2016-07-23 07:42:44</td>
      <td>21.76</td>
      <td>7546681945283</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sarabury</td>
      <td>2016-04-02 04:32:25</td>
      <td>38.03</td>
      <td>4932495851866</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sarabury</td>
      <td>2016-06-23 05:03:41</td>
      <td>26.82</td>
      <td>6711035373406</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Sarabury</td>
      <td>2016-09-30 12:48:34</td>
      <td>30.30</td>
      <td>6388737278232</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
#set up 3 data sets to chart and compare
urban = merged.loc[merged["type"] == "Urban"]
suburban = merged.loc[merged["type"] == "Suburban"]
rural = merged.loc[merged["type"] == "Rural"]
```


```python
#Avg Fare
urbanfare = urban.groupby("city")["fare"].mean()
suburbanfare = suburban.groupby("city")["fare"].mean()
ruralfare = rural.groupby("city")["fare"].mean()

#Total Rides
urbanrides = urban.groupby("city")["ride_id"].count()
suburbanrides = suburban.groupby("city")["ride_id"].count()
ruralrides = rural.groupby("city")["ride_id"].count()

#Total Drivers
urbandrivers = urban.groupby("city")["driver_count"].count()
suburbandrivers = suburban.groupby("city")["driver_count"].count()
ruraldrivers = rural.groupby("city")["driver_count"].count()
```


```python
#set up plot
Urban = mlp.scatter(urbanrides, urbanfare, s = urbandrivers*5, color = "gold",edgecolors = "black", label = "Urban")
Suburban = mlp.scatter(suburbanrides, suburbanfare, s = suburbandrivers*5, color = "lightblue", edgecolors = "black", label = "suburban")
Rural = mlp.scatter(ruralrides, ruralfare, s = ruraldrivers*5, color = "lightcoral",edgecolors = "black", label = "rural")
mlp.title("Pyber")
mlp.xlabel("Total Rides by City")
mlp.ylabel("Average Fare by City")
mlp.legend(handles=[Urban, Suburban, Rural], loc="best")
```




    <matplotlib.legend.Legend at 0xb70ab00>




![png](output_5_1.png)



```python
#Total Fare by city type
piefare = merged.groupby("type")["fare"].sum()
mlp.title("Total Fare by City Type")
mlp.pie(piefare, labels=["Rural","Suburban","Urban"], colors = ["lightcoral", "lightblue", "gold"], explode = (.5,0,0), autopct="%.1f%%")
mlp.axis("equal")
mlp.show()
```


![png](output_6_0.png)



```python
#Total Rides by city type
ridepie = merged.groupby("type")["ride_id"].count()
mlp.title("Total Rides by City Type")
mlp.pie(ridepie, labels = ["Rural","Suburban","Urban"], colors = ["lightcoral", "lightblue", "gold"], explode = (.5, 0, 0), autopct="%.1f%%")
mlp.axis("equal")
mlp.show()
```


![png](output_7_0.png)



```python
#Total Drivers by city type
driverpie = merged.groupby("type")["driver_count"].sum()
mlp.title("Total Drivers by City Type")
mlp.pie(driverpie, labels=["Rural","Suburban","Urban"], colors = ["lightcoral", "lightblue", "gold"], autopct="%.1f%%", explode = (1, 0, 0))
mlp.axis("equal")
mlp.show() 
```


![png](output_8_0.png)



```python
#Observations

#1. Populations from lowest to highest tend to go from Rural, Suburban, to Urban
#2. Avg fare tends to decrease with increased population
#3. Higher populations mean there are typically more rides and drivers
```
