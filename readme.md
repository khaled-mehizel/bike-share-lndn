# bike-share-lndn-dashboard
 Analyzing and visualizing bike share data in London

# Overview
This is a readme detailing the process of cleaning and analyzing a dataset about the usage of Bike Share in London as well as creating a versatile and interactive dashboard in Tableau.

The purpose of the study is to predict future usage of the service.

Timeframe of the study: 2015 - 2017
# Resources and tools used
- Dataset provided by [Hristo Mavrodiev](https://www.kaggle.com/hmavrodiev), Powered by TfL Open Data.
- Contains OS data © Crown copyright and database rights 2016 and Geomni UK Map data © and database rights [2019].
- Decided to use Excel and Power Query to clean the data this time.
- Used Tableau for the visualization.

# Data Cleaning in Excel
- First order of business is to format the .csv file as a table and set the headers.
- Modified header names to make them less vague.
- Checking the data allows us to see that the timestamps are properly formatted, but I'd like to format them anyway just in case, ill-formatted timestamps always cause issues.
- Noticed two records that had float values for the temperature and wind speeds, rounded it to the closest 0.5 since the numerical precision of the data is 0.5.
- Formatted the humidity column into a percentage. Had to divide it by 100, used the division operator in a new column, then copied and pasted the values themselves in the old column. This is so it's populated by the values themselves rather than functions.
- Verified the data types of the rest of the columns.


# Final touches in Power Query
- Replaced the integers used as codes for weather states and seasons with the states and season names themselves. Used Power Query instead of **REPLACE()** for performance purposes. A python dictionary and **map()** function would've been faster!


# Visualizing in Tableau
## Moving Average
- After importing the workbook into Tableau, the first order of business is to create two user parameters for our moving average. This enables the user to look at the average based on the period and time duration that they choose. 

- The parameters are useless until we add a measure. We just put them into the **DATETRUNC()** function, along with the Time field.

- We make a line chart with the measure and the sum of bike share usages.

- We then create the set for the moving average period, but we keep it empty and fill it with a **MIN()** and **MAX()** with an **IF()** statement in them:
        
      `{ MIN(IF[Moving Period Average Set] THEN [Moving Average Period] END) }`
      `{ MAX(IF[Moving Period Average Set] THEN [Moving Average Period] END) }`
    These are the min and max months of the currently selected time frame.
- This populates our Moving Period Averages Set, but we'll have to add an action to tell Tableau to select the dates from the set when we select values in the line chart.

- To add visual feedback to the selection:
  - We add a reference band into the Moving Average Period field. It starts from Min Month and ends with Max Month.
  - We make an "In Range" Measure that when added to Colors mark, will make the selected portion of the chart a different color.
- Now, we add a quick calculation to the count of bike rides, making it a moving average. And we modify it so that the start is the *Moving Average Duration + 1*
- Now the chart will adjust according to the user preference, where one can look for the average in any timeframe they wish.

## Total Number of Bike Rides Card
- We start with a calculated field that sums the count of bike rides in the chosen time frame, and we turn it into a card.

## Wind Speed vs Temperature Heatmap
- We have so many records of both wind speed and temperatures so we should bin them.
- We add the fields into the rows and columns, and we make the main label the count of bike rides.
- The heatmap is also interactive and will change according to time frame thanks to the In Range measure. (Couldn't they just automate this like in Power BI?)
## Tooltip Charts
These bar charts will be included in tooltips for when the user hovers over a point in the visualizations.
  ### Weather
  This one will display a bar chart with information that shows which weather conditions had the most rides.
  ### Hour
  This one will display a bar chart with information that shows which hours had the most bike rides.
# Formatting
Because I'm horrible with colors, I decided to go with the Disco Elysium palette again, very strong orange and green.

## Building the dashboard
Self explanatory, really. We just make sure to update the Action so that it includes the dashboard, making it truly interactive.

# Insights
- The service is used the most at 8am and 5pm, likely as commuting vehicles to get to and from work.
- The service is very rarely used in unfavorable weather conditions (heat, freezing temperatures, rainfall, snowfall) and only as a last resort.
- The line chart drops as the months go in the year, meaning that the service is indeed used less during winter.
- It can be observed that the service is used slightly more during Christmas holidays.

# Recap
In this project, we:
- Acquired a dataset off Kaggle.
- Used a combo of Excel and Power Query to clean it up.
- Used Tableau to visualize the data and extract meaningful insights.

Also, they really don't let you save your work offline in Tableau Public? In the year 2023 I have to jump through 3 hoops to save my own work on my own device? Unbelievable