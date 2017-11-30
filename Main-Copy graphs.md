

```python
# Dependencies
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import seaborn as sns
```


```python
#import both files
cityDF = pd.read_csv("city_data.csv")

# sum driver count by city to account for duplicate Port James entry
cityDFmergetype = cityDF
city_df = cityDFmergetype

```


```python
rideDF = pd.read_csv("raw_data/ride_data.csv")

```


```python
#combine both files on city
combineDF = pd.merge(rideDF, cityDFmergetype, on = "city", how = "inner")
combineDF.columns = ("City", "Date", "Fare", "Ride ID", "Driver Count", "Type")

```


```python
#calculate average fare per city
meanDF = pd.DataFrame(combineDF.groupby("City")["Fare"].mean())
meanDF.columns = ["Average Fare"]
meanDF
averagefarecity = meanDF["Average Fare"]

```


```python
#calculate total fare per city
sumDF = pd.DataFrame(combineDF.groupby("City")["Fare"].sum())
sumDF.columns = ["Total Fare"]
sumDF
totalfarecity = sumDF["Total Fare"]

```


```python
#calculate total number of rides per city
totalRidesDF = pd.DataFrame(combineDF.groupby("City")["Ride ID"].count())
totalRidesDF.columns = ["Total Rides"]
totalRidesDF
totalriderscity = totalRidesDF["Total Rides"]
```


```python
#calculate total number of drivers per city
totalDriversDF = pd.DataFrame(cityDF[["city","driver_count"]])
DriversDF = totalDriversDF.set_index("city")

totaldriverscity = DriversDF["driver_count"]
```


```python
#show city type
citytypeshow = pd.DataFrame(cityDF[["city","type"]])
citytypeDf = citytypeshow.set_index("city")
citytypeDf

citytype = citytypeDf["type"]
```


```python
# % of total fare by city type
fareCityDF = pd.DataFrame(combineDF.groupby("Type")["Fare"].sum())
fareCityDF.columns = ["Total Fare"]
fareDF = fareCityDF.reset_index()
```


```python
# % of Total Fares by City Type
labels = fareDF["Type"]
sizes = fareDF["Total Fare"]
colors = ["gold", "lightskyblue", "lightcoral"]
explode = (0, 0, 0.1)

plt.title("% of Total Fares by City Type")
plt.pie(x = sizes, explode=explode, labels= labels, colors=colors, autopct="%1.1f%%"
        ,shadow=True, startangle=130)
plt.savefig("percentfares.png")
plt.show()
```


![png](output_10_0.png)



```python
# % of total rides by city type
ridesCityDF = pd.DataFrame(combineDF.groupby("Type")["Ride ID"].count())
ridesCityDF.columns = ["Total Rides"]
ridesDF = ridesCityDF.reset_index()
```


```python
# % of Total Rides by City Type
labels = ridesDF["Type"]
sizes = ridesDF["Total Rides"]
colors = ["gold", "lightskyblue", "lightcoral"]
explode = (0, 0, 0.1)

plt.title("% of Total Rides by City Type")
plt.pie(x = sizes, explode=explode, labels= labels, colors=colors, autopct="%1.1f%%"
        ,shadow=True, startangle=130)
plt.savefig("percentrides.png")
plt.show()
```


![png](output_12_0.png)



```python
# % of total drivers by city type
driversCityDF = pd.DataFrame(cityDF.groupby("type")["driver_count"].sum())
driversCityDF.columns = ["Total Drivers"]
driversDF = driversCityDF.reset_index()
```


```python
# % of Total Fares by City Type
labels = driversDF["type"]
sizes = driversDF["Total Drivers"]
colors = ["gold", "lightskyblue", "lightcoral"]
explode = (0, 0, 0.1)

plt.title("% of Total Drivers by City Type")
plt.pie(x = sizes, explode=explode, labels= labels, colors=colors, autopct="%1.1f%%"
        ,shadow=True, startangle=130)
plt.savefig("percentdrivers.png")
plt.show()
```


![png](output_14_0.png)


### Your objective is to build a Bubble Plot that showcases the relationship between four key variables:
* Average Fare ($) Per City
* Total Number of Rides Per City
* Total Number of Drivers Per City
* City Type (Urban, Suburban, Rural)


```python
# combine files with mean, rides, and total fare
city_df.set_index("city", inplace = True)
city_df["average_fare"] = averagefarecity
city_df["ride_count"] = totalriderscity
```


```python
# separate by type
urban_df = city_df.loc[city_df["type"]=="Urban",:]
suburban_df = city_df.loc[city_df["type"]=="Suburban",:]
rural_df = city_df.loc[city_df["type"]=="Rural",:]
```


```python
# Urban scatterplot
# x = ride count
x = urban_df["ride_count"]

# y = average fare
y = urban_df["average_fare"]

# z = total drivers
z = urban_df["driver_count"]

colors = ["gold"]

urban_plot = plt.scatter(x= x, y=y, s=z*5, c=colors, alpha=0.5, edgecolor = "black", label = "Urban")

```


```python
# Suburban scatterplot
#x = ride count
x = suburban_df["ride_count"]

#y = average fare
y = suburban_df["average_fare"]

#z = total drivers
z = suburban_df["driver_count"]

colors = ["lightskyblue"]

suburbanplot = plt.scatter(x= x, y=y, s=z*5, c=colors, alpha=0.5, edgecolor = "black", label = "Suburban")


```


```python
# Rural scatterplot
# x = ride count
x = rural_df["ride_count"]

# y = average fare
y = rural_df["average_fare"]

# z = total drivers
z = rural_df["driver_count"]

colors = ["lightcoral"]

ruralplot = plt.scatter(x= x, y=y, s=z*5, c=colors, alpha=0.5, edgecolor = "black", label = "Rural")

```


```python
# Set title
plt.title('Pyber Ride Sharing Data')

# Set x-axis label
plt.xlabel('Total Number of Rides (Per City)')

# Set y-axis label
plt.ylabel('Average Fare ($)')

plt.legend(loc="best")
plt.grid()
plt.savefig("pyberbubble.png")
plt.show()
```


![png](output_21_0.png)


## Observations
* There are substantially more drivers and riders in the Urban areas.
* Although Rural areas have less drivers and riders their Average Fares are mostly higher.
* Urban area drivers make less on an average fare than Suburban and Rural areas, however the number of rides they have per city make up for the difference in average fare and they end up with a higher total fare.



