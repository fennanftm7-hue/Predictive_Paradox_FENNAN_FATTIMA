# Power Demand Forecasting Report

### Key Insights
*	The analysis revealed that our model is mostly driven by Temporal Inertia (via rolling means and lags) This accounts for the most part of its predictive power. However, Apparent Temperature and Economic Indicators serve as important corrective features, allowing the model to adjust for changes caused by weather and long-term infrastructure growth.
 
### Data Handling & Pre-processing
*	**Missing Data**: Missing data was handled using linear interpolation. In linear interpolation, the preceding and succeeding values are considered to have a linear relation and consequently the missing value is filled in.

*	**Anomalies**: Anomalies were handled by setting a threshold value (of 25000 MW in my code). When I first graphed the demand data given with the ‘datetime’ values, I saw spikes occurring in my graph that needed to be ‘smoothened out’. Using the threshold, I removed the values that surpassed this ‘max’ value. The dropped values were then filled in with linear interpolation as well (imputation).
 
### Feature Engineering
I engineered features across three categories to provide the model with a holistic view of the grid:

**Temporal Features**: 
1.	hour: to check daily usage cycles (eg. peak usage in the evenings, low usage during early mornings).
2.	day: power demand drops due to low industrial and commercial activity on weekends as compared to weekdays.
3.	month & quarter: to check seasonality of power usage. Demand rises substantially in the hot summer months due to cooling loads as compared to the winter months.
  
**Memory Features**:
1.	1hour_lag: best indicator of what will happen now is from what happened 1 hour back. Power demand is highly inertial.
2.	nextday_leg : captures periodicity, as people generally follow the same routine every 24 hours.
3.	6h_rolling_mean: captures momentum. Gives us the trend of if the demand is steadily rising or falling over the last few hours.
 
**External Features**:
1.	Weather (shows daily changes) : temperature, humidity, apparent temperature, precipitation, cloud cover
2.	Economics (shows trends over the years): GDP, Urban population, Industry

I chose these features as they showed considerable effect on the demand from the heatmap created using seaborn. 

### Final Model Performance

*	**Primary Metric (MAPE): 4.65%**

*   **Feature Importance Ranking**:
1.	nextday_lag / 6h_rollingmean / 1hour_lag : what happened earlier is the strongest predictor of what will happen in the near future (inertia).
2.	temp / app_temp : higher temperatures show higher demands in power, showing a significant positive correlation in the heatmap. Apparent temperature is more relevant as it also factors in other features, such as humidity.
3.	Urban population / GDP :  explains the long- term trends of the grid capacity over the decade long span.


