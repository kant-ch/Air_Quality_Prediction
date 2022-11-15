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

# Exploration Data Analysis
---
We got the stations which have the most pollution in each air pollution quality, then we decided to select this station for modeling and predicting the air quality. In this project, we choose only 1 station for each air quality. The model we build in this project might be the model architecture for the future project.
|  IDStations | quality | Values |
|-------------|:-------:|:------:|
|         652 |     nox | 136.65 | 
|         501 |     no2 |  53.63 |
|         626 |     nh3 |  43.11 |
|         661 |    pm10 |  37.31 |
|        1297 |    pm25 |  27.29 |
| STA.IT1509A |     so2 |   9.98 | 
**In summary**, we choose station for each pollutants following the analysis in the EDA section.
- Station: 652 for air quality of NO<sub>x</sub>
- Station: 501 for air quality of NO<sub>2</sub>
- Station: 626 for air quality of NH<sub>3</sub>
- Station: 661 for air quality of PM<sub>10</sub>
- Station: 1297 for air quality of PM<sub>2.5</sub>
- Station: STA.IT1509A for air quality of SO<sub>2</sub>

# Modeling and Model evaluation
---
In the modeling and model evaluation section, we used the ordinary time series model which is ARIMA model and advance model such as recurrent neural network model (RNN) and Long short-term memory model (LSTM). This section contains 7 parts.
1. Function using in the modeling section
2. Model for predicting nitrogen oxide (NO<sub>x</sub>) quantity of station 652
3. Model for predicting nitrogen dioxide (NO<sub>2</sub>) quantity of station 501
4. Model for predicting ammonia (NH<sub>3</sub>) quantity of station 626
5. Model for predicting PM<sub>10</sub> of station 661
6. Model for predicting PM<sub>2.5</sub> of station 1297
7. Model for predicting sulfur dioxide (SO<sub>2</sub>) quantity of station STA.IT1509A

The modeling parts will have at least 3 models which are ARIMA, RNN and LSTM model. The train and test data of ARIMA and neural network model will split in the different method.
- For ARIMA model, The test data will be the small size (~5% of the data) because ARIMA model will predict the mean of the data in the long term.
- For RNN and LSTM model, The test data will be the default size of the split in the Scikit-learn (25% of the data).
- The last model for each pollutants may be the RNN or LSTM model and do feature engineering in the model. The feature engineering in this modeling part is dummying the categorical variable and adding the moving average.

The evaluation of the model will be one or more of the below list.
- Score of the model: Mean square error, Akaike Information Critera (for ARIMA model), Mean absolute error percentage [[Ref.]](https://neptune.ai/blog/select-model-for-time-series-prediction-task) and Root mean square error (Which we used to compare between baseline model)
    - Baseline model is the model predicting only mean of training data.
- Ablity for capture the trend in the time series data observed by time series graph
- If there are aqi index for pollutants, we will calculate the accuracy of the model that can predict pollutants correctly. For example, If there are 10 days that have NO<sub>x</sub> greater than 120 and the model can predict NO<sub>x</sub> > 120 7 days, The accuracy of the model is 70% for predicting the high pollution days.

# Conclusion
---
### 1. Model for predicting amount of pollutants in the air. 
- The best model for almost all pollutants in this project is LSTM model. 
- The models tend to be better models with dummying the categorical variables, adding the date time part and moving average to the model. We have to tune the model separately for each pollutants.

From the final model result, we can conclude the result into **2 scenarios**.

**Scenario 1:** NO<sub>x</sub>, NO<sub>2</sub>, NH<sub>3</sub>, PM<sub>10</sub>, and PM<sub>2.5</sub> model have similar/lower rmse to/than baseline rmse and the model could capture trend of the data.
- All 5 models have similar trend to some part in training set.

**Scenario 2:** Even though, SO<sub>2</sub> model has lower rmse than baseline rmse, the model quite can't capture the trend of the data.
- SO<sub>2</sub> training data doesn't have obvious trend, it might be hard for the model to predict the SO<sub>2</sub> quantity.

Model in **Scenario 1** is good score and can capture the trend in the data, e.g. NO<sub>x</sub>, NO<sub>2</sub>, PM<sub>10</sub> and PM<sub>2.5</sub> model.

### 2. Identify the bad air quality days from the model and comparing to the actual value.
The data of bad air quality days is following the Day_exceed_aqi column in table below. The predictions of bad air quality days are collect in pred_exceed_aqi column.

| pollutant | aqi | Condition | Day_exceed_aqi | Preds_exceed_aqi | Accuracy |
|-----------|:---:|:---------:|:--------------:|:----------------:|:--------:|
| NOx       | 120 |    poor   |       153      |        102       |   66.67  |
| NO2       |  90 |   medium  |        6       |         0        |   0.00   |
| PM10      |  50 |    poor   |       128      |        78        |   60.94  |
| PM2.5     |  50 |    poor   |       54       |        36        |   66.67  |

From the table, we can conclude into 2 points.

2.1. There are 6 days of NO<sub>2</sub>  quantity above aqi threshold and all days are peak of the data as shown in graph below. So, It might be hard for the model to capture these days.

2.2. The best models for predicting NO<sub>x</sub> , PM<sub>10</sub>  and PM<sub>2.5</sub>  can predict the bad air quality correctly about 60 - 67% as shown in above dataframe. The model can capture some peak of the data which is useful data for planning the new strategies. The example of model that can capture the peak of the data is displayed in the graph below.

# Recommendation
---

**1. Model Usage**
- The models might predict the days that have bad air quality in agricultural area. Then we could plan the strategies for handing air quality.
- Example of strategy: We might set the limit of activities in the bad air quality by using campaign etc.
- For the further strategies for handling the air quality, we might have to consider with domain expert to find the strategies. 

**2. Air Quality Index**
- For the pollutants that have aqi to measure e.g. NO<sub>x</sub>, NO<sub>2</sub>, SO<sub>2</sub>, PM<sub>10</sub>, and PM<sub>2.5</sub>, We recommend to use this aqi for theshold.
- For the pollutants that haven't aqi to measure e.g. NH<sub>3</sub>, We might be discussing with the expert to set the threshold.

**3. Model improvement**
- Further tuning the model, for example, increasing layers of LSTM model.
- Do more work on feature engineering for example, additional of interaction term.
- The data which the model can't predict, we might use more robust model to predict. For example, N-BEATS, TFT, and DeepAR model.