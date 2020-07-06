
# PyBer Analysis Report
Analysis of ride-sharing app PyBer

#### Resources
* Data Sources: _city_data.csv_ and _ride_data.csv_.
* Software: Python 3.7.6, Visual Studio Code 1.45.1, Anaconda 4.8.3, Jupyter Notebook 6.0.3, Pandas library and MatplotLib library.
  
---
## Background and Results

#### Purpose
This analysis is intended to provide a summary per city category of Total Rides, Drivers and Fares and the Average Fares as well as a chart to display the total income of each type of city over time.

#### Technical Analysis
From the merged data from cities and rides, a new table has been built as a DataFrame containing the summary amounts grouped by the type of City, then filtered and aggregated by weeks to create a multi-line chart following the 'fivethirtyeight' style to display the total fare amount of the three different categories by date.

#### Results
It can be seen (_Summary Table_) that although the average fare per driver in urban cities are the lowest, over two and three times less compared to suburban and rural respectively, their high rides volume makes them the most profitable, two and nine times respectively, a behavior maintained over time (_Multi-line chart_).

Also the total sum fare that Urban cities are the most profitable with a $39,854.38 total revenue but due to the low number of drivers in rural cities, their average fare per driver is over 3 times higher than urban, therefore rural drivers generate more money each.  
* Summary Table:
  ![SummaryTable](./media/1.SummaryDF.png?raw=true)
* Multi-line chart:
  ![LineChart](./analysis/Fig8_challenge.png?raw=true)


#### Summary
The four-month period analysis shows that the average amounts are higher for rural followed by suburban and the urban, which make rural drivers generate more money each, but the total income has the reverse order as the number of rides are higher in urban, then suburban and the last rural.

---
## Challenges Encountered and Overcome

#### Challenges and Difficulties Encountered

The challenge faced was graphing the data as the requirements; being the x-axis the weekly dates, it made the chart to have more x labels (_Original x-axis_) than desired (_Desired x-axis_) and incorrectly formatted. 
* Original x-axis:
  ![OriginalXaxis](./media/2.OriginalXaxis.png?raw=true)

* Desired x-axis:
  ![DesiredXaxis](./media/3.DesiredXaxis.png?raw=true)

#### Technical Analyses Used

The x-axis values used to plot the chart are the indexes of the DataFrame which are a list of dates, specifically the last day of the week because the data had been binned weekly. A general built-in step function did not set the x axis ticks in the exact place for those last days of the week and graphed the chart incorrectly, so the following function was built to get a list of dates and return a list of the first date found in each month-year. This way, in our case, only the last day of the first week of each month will be added, and the chart will only display those marks in the x-axis.

``` python
    def firstDateEachMonth(listOfDates: list) -> list:

        firstsOfDates = []
        # Sort the list to have all the dates in order
        listOfDates.sort()
        iterator = 0

        for i in listOfDates:
            if iterator == 0:
                firstsOfDates.append(i)
            elif i.month != listOfDates[iterator-1].month:
                firstsOfDates.append(i) 
            iterator+=1

        return firstsOfDates
``` 

Adding the following commands when creating the plot to apply the function and format the axis properly (three letters month _"mmm"_), will set the major ticks of the x-axis as required:

``` python
    majorXTicks = firstDateEachMonth(binnedDataDF.index.tolist())
    ax.set_xticks(majorXTicks)
    ax.xaxis.set_major_formatter(mdates.DateFormatter("%b"))
```

---
## Recommendations and Next Steps

#### Recommendations for Future Analysis
Two additional analysis are suggested to complete the report. One is adding data for one or two years to research if there is any seasonal impact on the rides and drivers. The second is to make the analysis by city, adding in that case an extra dataset of public transport to check the impact of the public transport in the use of PyBer.  

#### Additional Analysis 1


* Description of Approach
  The chart generated displays a spike in the last week of February and a fall the following week, therefore analyzing data from a wider period could uncover some trends caused by the time of the year studied that cannot be determined in four-months range.
* Technical Steps
  Same steps as the ones done in the challenge will be followed only reading the bigger csv file and changing the range in the pivot table to be filtered with the loc function. Same line chart would show the variations over time.

#### Additional Analysis 2

* Description of Approach
  For this second suggestion an extra dataset containing the public transport quality, rate per capita or existence in each city would be necessary to find out if there is a relation between lack of public transport a higher use/fare of PyBer services.
* Technical Steps
  Having the new dataset, it should be properly merged to the PyBer DataFrame (on City column) to add the new extra information grouped by City. It will also help to decide on a grade to qualify public transport quality based on data obtained in the new dataset. Different combinations of Scatter Plots (displaying cities of same type in the same plot for better comparison) can uncover interesting behaviors.
  As examples: 
  - Total Number of Rides - Public Transport Grade with Circle sizes correlate with driver count per city. 
  - Total Number of Rides - Public Transport Grade with Circle sizes correlate with total fare per city. 
  