# Project: Air Quality Prediction in Lombardy Region
---
# Background and Problem Statement
---
**Background**

The Lombardy region is one of the most polluted region in Europe. There is a large evidence that the agricultural section has a significant impact on air quality. The Lombardy region has several agicultural section then there are large amount of pollutants emitted to the air. The data scientists in the Lombardy region were assigned to build the model to predicting the pollutants quantity in the air. The model which predicted the pollutants in the air might help to decide the next actionable plan for balancing between agricultural products and air quality.

The data set is retrieved from Zenodo. [[Ref.]](https://zenodo.org/record/6620530#.Y1Fkf3bP1PY) The detail of the data set is in the exploration data analysis section. Air quality information [[Ref1.]](https://www.euronews.com/weather/copernicus-air-quality-index), [[Ref2.]](https://airindex.eea.europa.eu/Map/AQI/Viewer/#) is provided using the European Air Quality Index, which follows the definition of the European Environment Agency (EEA, eea.europa.eu).

**Problem Statement**
1. The data scientists were assign to build the time series model architecture for predicting each pollutants of each station in the Lombardy region.
2. Identify the bad air quality days from the model and comparing to the actual value.
3. Recommendation for usage and improving of the model.

# Conclusion
---
### 1. Model for predicting amount of pollutants in the air. 
- The best model for almost all pollutants in this project is LSTM model. 
- The models tend to be better models with dummying the categorical variables, adding the date time part and moving average to the model. We have to tune the model separately for each pollutants.

From the final model result, we can conclude the result into **2 scenarios**.

**Scenario 1:** NO$_{x}$, NO$_{2}$, NH$_{3}$, PM$_{10}$, and PM$_{2.5}$ model have similar/lower rmse to/than baseline rmse and the model could capture trend of the data.
- All 5 models have similar trend to some part in training set.

**Scenario 2:** Even though, SO$_{2}$ model has lower rmse than baseline rmse, the model quite can't capture the trend of the data.
- SO$_{2}$ training data doesn't have obvious trend, it might be hard for the model to predict the SO$_{2}$ quantity.

Model in **Scenario 1** is good score and can capture the trend in the data, e.g. NO$_{x}$, NO$_{2}$, PM$_{10}$ and PM$_{2.5}$ model.

### 2. Identify the bad air quality days from the model and comparing to the actual value.
The data of bad air quality days is following the Day_exceed_aqi column in table below. The predictions of bad air quality days are collect in pred_exceed_aqi column.

| pollutant | aqi | Condition | Day_exceed_aqi | Preds_exceed_aqi | Accuracy |   |
|-----------|:---:|:---------:|:--------------:|:----------------:|:--------:|--:|
| NOx       | 120 |    poor   |       153      |        102       |   66.67  |   |
| NO2       |  90 |   medium  |        6       |         0        |   0.00   |   |
| PM10      |  50 |    poor   |       128      |        78        |   60.94  |   |
| PM2.5     |  50 |    poor   |       54       |        36        |   66.67  |   |

From the table, we can conclude into 2 points.

2.1. There are 6 days of NO 2  quantity above aqi threshold and all days are peak of the data as shown in graph below. So, It might be hard for the model to capture these days.

2.2. The best models for predicting NO 𝑥 , PM 10  and PM 2.5  can predict the bad air quality correctly about 60 - 67% as shown in above dataframe. The model can capture some peak of the data which is useful data for planning the new strategies. The example of model that can capture the peak of the data is displayed in the graph below.

# Recommendation
---

**1. Model Usage**
- The models might predict the days that have bad air quality in agricultural area. Then we could plan the strategies for handing air quality.
- Example of strategy: We might set the limit of activities in the bad air quality by using campaign etc.
- For the further strategies for handling the air quality, we might have to consider with domain expert to find the strategies. 

**2. Air Quality Index**
- For the pollutants that have aqi to measure e.g. NO$_{x}$, NO$_{2}$, SO$_{2}$, PM$_{10}$, and PM$_{2.5}$, We recommend to use this aqi for theshold.
- For the pollutants that haven't aqi to measure e.g. NH$_{3}$, We might be discussing with the expert to set the threshold.

**3. Model improvement**
- Further tuning the model, for example, increasing layers of LSTM model.
- Do more work on feature engineering for example, additional of interaction term.
- The data which the model can't predict, we might use more robust model to predict. For example, N-BEATS, TFT, and DeepAR model.