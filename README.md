# PyBer_Analysis

## Project Overview

This projects entails performing an analysis for Pyber: a python based ride sharing app company. 

I have provided bubble charts, box and whisker plots, pie charts showing the relationships between type of city, the number of riders, 
the number of drivers,the percentage of total fares, riders and drivers by type of city.

However, the CEO has given me a brand-new assignment to create a multiple-line graph that shows the total weekly fares for each city type.

In this written report, I will summarize how the data differs by city type and how those differences can be used by decision-makers at PyBer.

This analysis and visualization will help Pyber improve access to ride sharing services and determine affordabilty for 
under-served neighbourhoods.

### Resources

Data Source: city_data.csv, ride_data.csv, PyBer_Challenge.ipynb.

Software: Python 3.7.6, Visual Studio Code 1.56.0, Anaconda 4.9.2, Jupyter Notebook 6.1.4, Matplotlip 3.3.2.


## Project Results:

The below information shows the results of the findings from the analysis;

### Rides Count

* Urban cities have the highest number of rides representing 68% of the total rides or 1,625 rides.
* Suburban cities have the second highest number of rides representing 26% of the total rides or 625 rides.
* Rural cities have the least number of rides representing 5% of the total rides or 125 rides.
* The total rides count is 2,375.

### Drivers Count

* Urban cities have the highest number of drivers representing 81% of the total drivers or 2,405 drivers.
* Suburban cities have the second highest number of rides representing 16% of the total drivers or 490 drivers.
* Rural cities have the least number of rides representing 5% of the total rides or 125 rides.
* The total drivers count is 2,973.

### Total Fares

* Urban cities have the highest total fares representing 63% of the total fares or $39,854 fares.
* Suburban cities have the second highest total fares representing 30% of the total fares or $19,356 fares.
* Rural cities have the least total fares representing 7% of the total fares or $4,328 fares.
* The total fares is $63,538.

### Average Fare per ride

* Rural cities have the highest average fare per ride representing 39% of the average fare per ride or $35 average fare per ride.
* Suburban cities have the second highest average fare per ride representing 34% of the average fare per ride or $31 average fare per ride.
* Urban cities have the least average fare per ride representing 27% of the average fare per ride or $24 average fare per ride.

### Average Fare per driver

* Rural cities have the highest average fare per driver representing 49% of the average fare per driver or $55 average fare per driver.
* Suburban cities have the second highest average fare per driver representing 36% of the average fare per driver or $40 average fare per driver.
* Urban cities have the least average fare per driver representing 15% of the average fare per driver or $17 average fare per driver.

The below image show the report summary per city;

![result_summary](https://user-images.githubusercontent.com/81701640/119040308-42f2e100-b983-11eb-85d2-f83f5e9e8d44.png)

### Multiple line plot of total weekly fares per city

![Multiple_line_graphs](https://user-images.githubusercontent.com/81701640/119040351-4f773980-b983-11eb-98c5-9d098fab807c.png)

### Code for the Analysis

```python

# Add Matplotlib inline magic command
%matplotlib inline
# Dependencies and Setup
import matplotlib.pyplot as plt
import pandas as pd

# File to Load (Remember to change these)
city_data_to_load = "Resources/city_data.csv"
ride_data_to_load = "Resources/ride_data.csv"

# Read the City and Ride Data
city_data_df = pd.read_csv(city_data_to_load)
ride_data_df = pd.read_csv(ride_data_to_load)

# Combine the data into a single dataset

pyber_data_df = pd.merge(ride_data_df, city_data_df, how="left", on=["city", "city"])

# Display the data table for preview

pyber_data_df.head()

#  1. Get the total rides for each city type
type_rides_count = pyber_data_df.groupby(["type"]).count()["ride_id"]

type_rides_count

# 2. Get the total drivers for each city type
type_drivers_count = city_data_df.groupby(["type"]).sum()["driver_count"]

type_drivers_count

#  3. Get the total amount of fares for each city type
type_fares_count = pyber_data_df.groupby(["type"]).sum()["fare"]

type_fares_count

#  4. Get the average fare per ride for each city type. 
type_avg_fare = pyber_data_df.groupby(["type"]).mean()["fare"]

type_avg_fare

# 5. Get the average fare per driver for each city type. 
driver_avg_fare = type_fares_count / type_drivers_count

driver_avg_fare

#  6. Create a PyBer summary DataFrame. 
pyber_summary_df=pd.DataFrame(
    { "Total Rides": type_rides_count,
      "Total Drivers": type_drivers_count,
      "Total Fares": type_fares_count,
      "Average Fare per Ride": type_avg_fare,
      "Average Fare per Driver": driver_avg_fare})
pyber_summary_df

#  7. Cleaning up the DataFrame. Delete the index name
pyber_summary_df.index.name = None

pyber_summary_df

#  8. Format the columns.

pyber_summary_df["Total Fares"] = pyber_summary_df["Total Fares"].map("${:,.2f}".format)
pyber_summary_df["Average Fare per Ride"] = pyber_summary_df["Average Fare per Ride"].map("${:,.2f}".format)
pyber_summary_df["Average Fare per Driver"] = pyber_summary_df["Average Fare per Driver"].map("${:,.2f}".format)

pyber_summary_df

# Deliverable 2. Create a multiple line plot that shows the total weekly of the fares for each type of city.

# 1. Read the merged DataFrame

pyber_data_df.head()


# 2. Using groupby() to create a new DataFrame showing the sum of the fares 
#  for each date where the indices are the city type and date.
type_dates_count = pyber_data_df.groupby(["date", "type"]).sum()["fare"]

type_dates_count

# 3. Reset the index on the DataFrame you created in #1. This is needed to use the 'pivot()' function.
type_dates_count = type_dates_count.reset_index()

type_dates_count

# 4. Create a pivot table with the 'date' as the index, the columns ='type', and values='fare' 
# to get the total fares for each type of city by the date. 
pyber_pivot_table = pd.pivot_table(type_dates_count, values='fare', index = 'date', columns ='type')

pyber_pivot_table.head(10)

# 5. Create a new DataFrame from the pivot table DataFrame using loc on the given dates, '2019-01-01':'2019-04-29'.
loc_pyber_pivottable = pyber_pivot_table.loc['2019-01-01':'2019-04-29']
loc_pyber_pivottable.head(10)

# 6. Set the "date" index to datetime datatype. This is necessary to use the resample() method in Step 8.
loc_pyber_pivottable.index = pd.to_datetime(loc_pyber_pivottable.index)


# 7. Check that the datatype for the index is datetime using df.info()
loc_pyber_pivottable.info()

# 8. Create a new DataFrame using the "resample()" function by week 'W' and get the sum of the fares for each week.
loc_pyber_pivottable_res=loc_pyber_pivottable.resample('W').sum()

loc_pyber_pivottable_res.info()
loc_pyber_pivottable_res.head(10)

# 9. Using the object-oriented interface method, plot the resample DataFrame using the df.plot() function. 

# Import the style from Matplotlib.
from matplotlib import style
# Use the graph style fivethirtyeight.
style.use('fivethirtyeight')

loc_pyber_pivottable_res.plot(figsize=(25,10))
plt.ylabel("Fare($USD)")
plt.title("Total Fare by City Type")
plt.xlabel("Months")

# Save Figure
plt.savefig("Analysis/PyBer_fare_summary.png")
plt.legend()

```

## Project Summary

The following recommendations to the CEO will help in addressing any disparities among the city types and provide further insight 
on ride sharing services across the cities;

Firstly, there is a potential to improve the companies revenue by increasing the fares in urban cities. From the analysis, we saw that the Urban cities had the least
average fares per ride and per driver. If the charge per mile is increased, the company will earn more revenue and in turn increase the incentive for the drivers to complete more
trips.

Secondly, there is an opportunity to re-assign some drivers from the urban areas to the rural areas. This can be seeen in the count of drivers and rides, there is a gap identified 
when comparing this element in the urban centre. A lot of the drivers are inactive and should be re-deployed to the rural area which shows a higher potential for returns as seen in the 
average fares per ride.

Finally, Suburban cities have shown a strong trajectory and potential to grow as seen in the various matrices analysed above. More resources need to be deployed to the suburban cities
to expand to the level of operation to the levels seen in the Urban cities data, and take advantage of the average fares per ride and drivers in the suburban cities.

**Nnaemeka Enukorah**

**25th May 2021**


