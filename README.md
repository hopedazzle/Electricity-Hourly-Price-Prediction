# Electricity-Hourly-Price-Prediction
Predicted day-ahead market and hourly market prices for wholesale electricity, using system operator demand forecasts, weather operations, and reservoir water levels.



PROBLEM STATEMENT:

* Electricitv Hourlv Price Prediction:
  In the Calif. Electricity  grid operator's footprint (CAiSO), predict Day-ahead Market and Hourly 
  Market prices for wholesale electricity, using system operator demand forecasts, weather observations, and reservoir water levels.
  

TWO TARGETS, 27 FEATURES

Altogether,  there are 16 wBather features, one water level feature, four datetime features, fDur 
Electricity demand forecast features, one realtimB spot settlement price feature, and twD  targBt 
variables:   DAM (Day Ahead Market) and HASP (Hour Ahead Scheduling Process).  All regressions  
will be pairs of mDdels.  Each target variable will take the other target as a feature.


DATA COLLECTION

There are four types of data that come from three different sources.

1.    Electricity  wholesale prices.  The California  Independent  System Operator (CAISO) is a 
non-profit entity that is tasked with operating the electrical "grid" as a public good   The 
primary  concern for CAISO is operations  without any outages, and to always match supply with 
demand (also called "load") for energy, which cannot be realistically stored in large amounts.

CAlSo maintains a publicly  accessible repository of many types of information,  in the interests 
of transparency.   The open access system is known as OASIS, and can be queried via API for 
downloads  of information.   2 requests  per second, and no more than 31 days of info can be made 
to CAISO.  Any more and the requester IP address will be blocked.

an automated loop to request both price and load (demand)  forecast info from CAISO... 
automatically  generating  the correct string format url + query  syntax, and building in a 5 
second delay between  successive  monthly data requests going back 40 months to Jan. 1, 2016.  Data 
is on hourly, five-minute, and fifteen minute intervals, depending on the particular datum in 
question.

2.    California Dept. of Water Resources  (CA DWR) maintains a database of statewide metrics 
relating to reservoirs, infloW/DUtflow, water levels, snowpack water content, and more.  As one 
feature,    included  an hourly sum of the 47 reservoir's water content, measured  in acre-feet.   
There was no date range restriction on the api queries to CADWR.

Total water capacity may be an excellent feature to explain some electricity  price movement, since 
water levels dictate hydroelectric power production.   A good rainfall season like 2018-2019  can 
lower electricity  prices well into the fall because far less gas-fired generation needs to be 
dispatched.

3     NOAA hourly weather station historical data.  Very large zip tar files are available  by year 
for download.  These files unzip into 50 GB directories of many .csv files, each named with an 
11-digit numerical code identifying a particular  weather station or buoy. Selected four particular 
weather stations in California:   San Diego, Riverside, Redding, and Fresno for inclusion of 4 
weather related statistics.  The four locations were chosen for their completeness of data, and for 
their geographic  spread in California.

The four extracted statistics are:  surface temperature,  wind speed, cloud ceiling height, and 
horizontal visibility.   These are all metrics that affect either the demand for, or supply of, 
electricity.  Temperature motivates electricity  usage for air-conditioning and electr1c  heating, 
wind speed indicates wind farm productivity, and cloud ceiling + visibility may indicate the amount 
of potential solar generation available.



 THREE REGRESSION  MODELING  ESTIMATORS



1.     ARIMA
2.    SARIMAX
3.    RNN
Auto regressive integrated  moving average
ARIMA with Seaso na! effect, and eXogenous  variables Recurrent Neural Network

Looking to explore the relative strengths  and weaknesses of these three methods, both in 
predictive  performance,  as well as in ease of implementation, and computational  demand.


CONCLUSIONS  ABD ASSESSMENT

Tne hour ahead market proved to be more difficult for these methods to predict than the day ahead 
market.  ARIMA showed surprisingly good performance, matching that of the recurrent neural 
networks.  The RNNs were much faster to fit than ARIMA, and MUCH faster than SARIMAX, which was the 
worst performer overall.

ARIMA & SARIMAX

a)   Performance.   ARIMA showed a surprisingly  strong performance  (DA market), giV’en that no 
exogenous  features  go into the prediction.   Electricity  usage is highly patterned, and costs do 
not change drastically, so this makes sense.  SARIMAX was a worse performer, and given the addition 
of seasonality  and exogenous  features, this should not be the case.  Both model5 (as well as 
RNNs) fared poorly for the HA market.

b)   Deficiencies.   The Statsmodel package is not intuit“iVe,  and difficult to set up for 
train/test splits to get predictions on a test set fitted on a train set.  Extremely poor 
computational  efficiencies,  especially for SARIMAX.


RNNs

The Recurrent Neural Network5 took effort to configure, with a number of parameters and 
architectural  choice having meaningful  effect on predictive  performance,  including:
- number of perceptrons per layer
- number of hidden layers
- number of "lookback" periods (recurrent aspect Df RNN)
- learning rate
- regularization (early stopping, dropout layers, L1, and L2)

Witn that, however, the fit speed was quite fast relative to the compute  intensity of ARIMA, and 
most especially  SARIMAX.  Performance equaled that of ARIMA, and was easier to work with to 
extract predictions and apply scoring like MSE and r-squared.   Given that RNNs are still young 
with room for improvement,  this appears to be the far richer direction to explore.



Tne Day Ahead market appears to show  promise for prediction,  more so than the Hour Ahead market.  
 While SARIMAX should perform better than ARIMA, it fails to do so for the parameters attempted, 
and the long fit times make it unattractive.   RNNs show great promise, matching


ARIMA results, but faster fit times, and more easily extractable  predictions.  Overall, a 
successful first cut at this problem, with a number of rich options to pursue for future 
refinement.


FUTURE WORK ITEMS

1.    Expand data set back more years to get more train-test sets, fill in missing hours by going 
back to the CAISO API (snipped Some days to come in under CAISO 31-day hard restriction per query)

2.    Extend ARE MA grid-search,  run SARIMAX  grid search

3.    Do a systematic exploration of all RNN node/layer/Ir/look-bk combinations

4.    Try the Facebook Prophet time-series  ml tool

5.    Cast the dataset into "tabular" format and remove  the time index to allow for other 
estimators like random forest, etc.




