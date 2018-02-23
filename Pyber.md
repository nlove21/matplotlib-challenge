
## Option 1: Pyber

The ride sharing bonanza continues! Seeing the success of notable players like Uber and Lyft, you've decided to join a fledgling ride sharing company of your own. In your latest capacity, you'll be acting as Chief Data Strategist for the company. In this role, you'll be expected to offer data-backed guidance on new opportunities for market differentiation.

You've since been given access to the company's complete recordset of rides. This contains information about every active driver and historic ride, including details like city, driver count, individual fares, and city type.

Your objective is to build a [Bubble Plot](https://en.wikipedia.org/wiki/Bubble_chart) that showcases the relationship between four key variables:

* Average Fare ($) Per City
* Total Number of Rides Per City
* Total Number of Drivers Per City
* City Type (Urban, Suburban, Rural)

In addition, you will be expected to produce the following three pie charts:

* % of Total Fares by City Type
* % of Total Rides by City Type
* % of Total Drivers by City Type

As final considerations:

* You must use the Pandas Library and the Jupyter Notebook.
* You must use the Matplotlib and Seaborn libraries.
* You must include a written description of three observable trends based on the data.
* You must use proper labeling of your plots, including aspects like: Plot Titles, Axes Labels, Legend Labels, Wedge Percentages, and Wedge Labels.
* Remember when making your plots to consider aesthetics!
  * You must stick to the Pyber color scheme (Gold, Light Sky Blue, and Light Coral) in producing your plot and pie charts.
  * When making your Bubble Plot, experiment with effects like `alpha`, `edgecolor`, and `linewidths`.
  * When making your Pie Chart, experiment with effects like `shadow`, `startangle`, and `explosion`.
* You must include an exported markdown version of your Notebook called  `README.md` in your GitHub repository.


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sb
import os
```


```python
filepath = os.path.join("city_data.csv")
filepath2 = os.path.join("ride_data.csv")

city_df = pd.read_csv(filepath)
ride_df = pd.read_csv(filepath2)

#optimized to test later
#city_df = pd.read_csv("city_data.csv")
#ride_df = pd.read_csv("ride_data.csv")
```


```python
city_df.head()
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
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nguyenbury</td>
      <td>8</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>East Douglas</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Dawnfurt</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rodriguezburgh</td>
      <td>52</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
ride_df.head()
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
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Roy</td>
      <td>2016-01-02 18:42:34</td>
      <td>17.49</td>
      <td>4036272335942</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wiseborough</td>
      <td>2016-01-21 17:35:29</td>
      <td>44.18</td>
      <td>3645042422587</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spencertown</td>
      <td>2016-07-31 14:53:22</td>
      <td>6.87</td>
      <td>2242596575892</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nguyenbury</td>
      <td>2016-07-09 04:42:44</td>
      <td>6.28</td>
      <td>1543057793673</td>
    </tr>
  </tbody>
</table>
</div>




```python
merged_df = ride_df.merge(city_df, on="city")
merged_df.head()
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
merged_df.dtypes
```




    city             object
    date             object
    fare            float64
    ride_id           int64
    driver_count      int64
    type             object
    dtype: object




```python
grouped_city = merged_df.groupby(["city"])
#grouped_city.head()
```


```python
avgFare = grouped_city["fare"].mean()
numRides = grouped_city["ride_id"].count()
numDrivers = grouped_city["driver_count"].mean()
```


```python
city_drop = city_df.drop_duplicates("city")
cityType = city_drop.set_index("city")["type"]
```


```python
city_df2 = pd.DataFrame({
    "Number of Rides": numRides, 
    "Average Fare": avgFare,
    "Number of Drivers": numDrivers, 
    "City Type": cityType})
city_df2.head()
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
      <th>Average Fare</th>
      <th>City Type</th>
      <th>Number of Drivers</th>
      <th>Number of Rides</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <td>23.928710</td>
      <td>Urban</td>
      <td>21</td>
      <td>31</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <td>20.609615</td>
      <td>Urban</td>
      <td>67</td>
      <td>26</td>
    </tr>
    <tr>
      <th>Anitamouth</th>
      <td>37.315556</td>
      <td>Suburban</td>
      <td>16</td>
      <td>9</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <td>23.625000</td>
      <td>Urban</td>
      <td>21</td>
      <td>22</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <td>21.981579</td>
      <td>Urban</td>
      <td>49</td>
      <td>19</td>
    </tr>
  </tbody>
</table>
</div>




```python
urban = city_df2.loc[city_df2["City Type"]=="Urban"]
suburban = city_df2.loc[city_df2["City Type"]=="Suburban"]
rural = city_df2.loc[city_df2["City Type"]=="Rural"]
```


```python
plt.scatter(urban["Number of Rides"],urban["Average Fare"],s=urban["Number of Drivers"]*5, color = "gold", edgecolor = "black", label = "Urban", alpha = .50)
plt.scatter(suburban["Number of Rides"],suburban["Average Fare"],s=suburban["Number of Drivers"]*5, color = "lightskyblue", edgecolor = "black", label = "Suburban", alpha = .50)
plt.scatter(rural["Number of Rides"],rural["Average Fare"],s=rural["Number of Drivers"]*5, color = "lightcoral", edgecolor = "black", label = "Rural", alpha = .50)

plt.title("Pyber Ride Sharing Data (2016)")
plt.xlabel("Number of Rides (Per City)")
plt.ylabel("Average Fare ($)")
plt.xlim(0,45)
plt.legend(loc="best")
sb.set_style("darkgrid")

plt.show()
```


![png](output_12_0.png)



```python
totalFares = merged_df.fare.sum()
totalRides = merged_df.ride_id.count()
totalDrivers = numDrivers.sum()

print(totalFares, totalRides, totalDrivers)
```

    64669.11999999993 2407 3340



```python
uFare = merged_df.fare[merged_df.type == "Urban"].sum() 
uFare = round(uFare/totalFares,4)

sFare = merged_df.fare[merged_df.type == "Suburban"].sum() 
sFare = round(sFare/totalFares,4)

rFare = merged_df.fare[merged_df.type == "Rural"].sum() 
rFare = round(rFare/totalFares,5)

check = uFare+sFare+rFare

print(uFare, sFare, rFare, check)
```

    0.6197 0.3145 0.0658 1.0



```python
labels = ["Urban", "Suburban", "Rural"]
sizes = [uFare, sFare, rFare]
explode = (0.1, 0, 0)
colors = ["gold", "lightskyblue", "lightcoral"]

plt.pie(sizes, explode=explode, labels=labels, colors=colors, autopct="%1.1f%%", shadow=True, startangle=140)
plt.title("% of Total Fares by City")
plt.axis("equal")
```




    (-1.145707774398808,
     1.100651443658734,
     -1.2137163456211957,
     1.1108112277387974)




```python
plt.show()
```


![png](output_16_0.png)



```python
uRide = merged_df.ride_id[merged_df.type == "Urban"].count() 
uRide = round(uRide/totalRides,4)

sRide = merged_df.ride_id[merged_df.type == "Suburban"].count() 
sRide = round(sRide/totalRides,4)

rRide = merged_df.ride_id[merged_df.type == "Rural"].count() 
rRide = round(rRide/totalRides,4)

check2 = uRide+sRide+rRide

print(uRide, sRide, rRide, check2)
```

    0.6751 0.273 0.0519 1.0



```python
sizes2 = [uRide, sRide, rRide]

plt.pie(sizes2, explode=explode, labels=labels, colors=colors, autopct="%1.1f%%", shadow=True, startangle=140)
plt.title("% of Rides by City")
plt.axis("equal")
```




    (-1.1319035054960029,
     1.1012781328846053,
     -1.2203952772786926,
     1.1161379604156125)




```python
plt.show()
```


![png](output_19_0.png)



```python
uDriver = city_df2["Number of Drivers"][city_df2["City Type"] == "Urban"].sum() 
uDriver = round(uDriver/totalDrivers,4)

sDriver = city_df2["Number of Drivers"][city_df2["City Type"] == "Suburban"].sum() 
sDriver = round(sDriver/totalDrivers,4)

rDriver = city_df2["Number of Drivers"][city_df2["City Type"] == "Rural"].sum() 
rDriver = round(rDriver/totalDrivers,4)

check3 = uDriver+sDriver+rDriver

print(uDriver, sDriver, rDriver, check3)
```

    0.7805 0.1883 0.0311 0.9999



```python
sizes3 = [uDriver, sDriver, rDriver]

plt.pie(sizes3, explode=explode, labels=labels, colors=colors, autopct="%1.1f%%", shadow=True, startangle=140)
plt.title("% of Drivers by City")
plt.axis("equal")
```




    (-1.087345595011475, 1.1229816935891332, -1.2091533772984551, 1.11908826088157)




```python
plt.show()
```


![png](output_22_0.png)


## Analysis

2016 Pyber ride share data shows 67.5% of rides occur in urban cities. There are slightly more drivers in urban cities, making up 78% of the population, showing that there is more supply than demand in those cities. Though 5.2% of the population shows rural riders, they make up 6.6% of the fare total, meaning their rides cost a bit more than urban or suburban rides.
