---
title: "MTH6139 Time Series" 
subtitle: "Coursework 1 - Airline Passngers Per Month Analysis" 
author: "Hesihan Suthakaran" 
date: "Spring term 2025" 
output: 
  html_document:
    toc: true
    toc_float: true
    theme: spacelab 
    highlight: tango
editor_options: 
  markdown: 
    wrap: 72
---

```{r, echo=FALSE}
# This code will display the QMUL logo at the top right of the page
# Do not change this code
htmltools::img(src = knitr::image_uri("images/QMlogo.png"),
               alt = 'logo',
               style = 'position:absolute; top:0; right:0; padding:10px; width:20%;')
```

# Section 1: Background

Commercial aviation began with the Wright Brothers' historic flight on
December 17th 1903, at Kitty Hawk, North Carolina. That brief
demonstration, powered flight demonstrated the possibility of
heavier-than-air travel and set the stage for decades of innovation.
When introduced in the 1920s-1930s, public aviation was still considered
a luxury with only a small percentage of the population being able to
afford flights. Now, it is considered one of the cornerstones of global
transportation with billions of passengers flying every year.

```{r, echo=FALSE, out.width="62.5%", fig.cap="Flights during the 50s and 60s"}
knitr::include_graphics("images/airlinepassengers.png")
```

## 1.1 Why we want to model airline passengers per year?

Modelling airline passengers per year is crucial for several reasons:

  -Predicting future passenger volumes helps airlines plan capacity,
schedules flights and manage crew resources efficiently.

  -Understanding trends and seasonal patterns allows for better planning
of airport logistics, maintenance scheduling, and fuel management.

## 1.2 General Factors that affect the number Airline Passengers Per Year

Here are some general factors that affect airline passengers per year:

  - Regulatory and Political Environment

  -Technological and Operational Advancements



# 2. Purpose of this Project

The purpose of this project is to forecast the number of airline
passengers per year by using the number of airline passengers per year
from January 1949 to December 1960 which is a total of 144 observations.
We are going to be forecasting for next 8 quarters.



# 3. Airline Passengers Time Series

Here are the steps to my project:

&nbsp;
1) We first start this project by decomposing the original airline
passenger time series. 
&nbsp;

&nbsp;
2) We will then load and use the prophet
function which is a forecasting tool created by meta. 

&nbsp;
3) This will
then be followed by a series of codes that will create a data frame for
this time series. 

&nbsp;
4) After, we will then make a forecasting model
that will forecast 8 quarters ahead.

&nbsp;
5) Create a plot showcasing the
historical and forecasted data, some specialised plots

&nbsp;
6) Create a linear regression of the forecasted model

```{r}
plot(decompose(AirPassengers))
```

This line of code above creates a plot that illustrates the decomposition of the time series. It decomposes this into 4 sections: 

&nbsp;

1) Observed:



-The observed chart shows the original "AirPassengers" time series, which is the number of passengers recorded between 1949 and 1960. This shows us that as as the years go by, the number of passengers has increased.

&nbsp;

2) Trend:



-The Trend graph appears to show that as the time goes on, the number of passengers per month has increased

&nbsp;

3) Seasonality


-There is a clear seasonal pattern in the dataset, with peaks occurring around the middle of each year, typically in the summer months. This suggests that more people travel during this period, likely due to vacations and holidays.

&nbsp;

4) Residual Error


-The residual error shows the AirPassengers time series having removed the trend and seasonality.

&nbsp;

Moving on to the next step, We will now be using the Prophet function which will have to be loaded in using the below piece of code. After that, we use the function to forecast time series data. Once this has been loaded in, we can then create a data frame which can be done using the following code:
```{r, message= FALSE}
library(prophet)
library(zoo)
AirPassengers_df <- data.frame(
    ds = as.Date(as.yearmon(time(AirPassengers))),  
    y = as.numeric(AirPassengers)                     
)
```

The next line of code creates a forecasting model using the Prophet function and data frame.

```{r, message= FALSE}
AirPassengers_forecasting_model = prophet::prophet(AirPassengers_df)
```

The lines of code here create a data frame forecasting future sunspots. We are going to be forecasting for the next 2 years.

```{r, message= FALSE}
forecasts_of_future_AirPassengers = prophet::make_future_dataframe(AirPassengers_forecasting_model, periods=8, freq="quarter")
```

This next piece of code uses the predict function to generate forecasts for future number of air passengers per month.
```{r, message= FALSE}
results_future_forecasts_of_AirPassengers = predict(AirPassengers_forecasting_model, forecasts_of_future_AirPassengers)
```

To create a plot that shows the forecasted and historical data of number of Airline passengers in a month. 
```{r}
plot(AirPassengers_forecasting_model, results_future_forecasts_of_AirPassengers)
```

# What does this graph show?
There are three components to this graph:
&nbsp;

1) The black spots show the data that has been observed, so the number of air passengers recorded monthly.

&nbsp;

2)  Blue line that represents the forecast generated by Prophet of the number Air passengers every month.

&nbsp;

3) Light blue shading represents the confidence intervals that the generated forecasts are expected to fall within.


# Trend and Seasonality of The Forecasted Model

To see the trend and seasonality of the forecasted model generated by Prophet, we will be using line of code found below. This will then create 2 graphs which will show the trend and seasonality of this model which can also be found below: 
```{r}
prophet_plot_components(AirPassengers_forecasting_model, results_future_forecasts_of_AirPassengers)
```

From these graphs, we can find some few things:

&nbsp;

-From it, we can observe that the trend has a stable increase from 1949 onwards. This shows that the function is working as that is what has happened in the real world; as mentioned before, there are billions of air passengers per year.

&nbsp;

-The bottom graph also shows the seasonality which correlates with the original time series that we cheked in step 1 of section 3.

We will now create a dyplot which is an interactive graph that shows the data we have with the forecasted data and compares them. The code for this plot can be found below, along with the actual graph:

```{r, message= FALSE}
dyplot.prophet(AirPassengers_forecasting_model, results_future_forecasts_of_AirPassengers)
```

We can see that the from the dqplot, there is a slight disrepancy between the forecasted data and the actual data, which means that the forecasted data may be slightly over/under predicted.

Finallly, we'll be discussing a linear regression for this model and the summary of this:

```{r, message= FALSE}
slm= lm(y~ds, data=AirPassengers_df)
summary(slm)
```
From this summary, we can figure out a few things:
&nbsp;

- Standard error is 3.035e-03 which is very low which suggests that the data is very precise

&nbsp;

- Beta of linear regression model is 8.729e-02 which shows that the increase in th number of Air passengers per unit is  8.729e-02

&nbsp;

- P value is 2e-16 which is very small and we can say near to 0, which means that this data is statistically signifcant.

# References

Used the Air Passenger dataset

&nbsp;

Why forecasting number of Air Passengers is important: <https://www.icao.int/sustainability/Documents/RTK%20ranking/ICAO_LTF_MODEL_DOC.pdf>

&nbsp;

Seasonality effect on number of Air Passengers:<https://link.springer.com/article/10.1007/s11042-023-15552-1>
