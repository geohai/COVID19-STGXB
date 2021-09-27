# COVID-STXGB
The repository for Spatio Temporal eXtreme Gradient Boosting (STXGB) model presented in the __Predicting County-Level COVID-19 Cases using Spatiotemporal Machine Learning Through Social Media and Cell-Phone Data as Proxies for Human Interactions__ paper.



You must cite the paper above if you are planning to use the code.

There are three notebooks in this repository as follows:

## Data Preparation

A notebook for generating training data for ML models

Input Files needed to run this notebook:

> US counties GeoJSON ('Contiguous_US.geojson')
Uploaded to Drive: https://drive.google.com/drive/u/1/folders/1k6f2UcTTdDui_Y5wR17EKIMripRx8qjQ

> COVID-19 case data from John Hopkins ('time_series_covid19_confirmed_US.csv')
download from: https://github.com/CSSEGISandData/COVID-19/tree/master/csse_covid_19_data/csse_covid_19_time_series

> Facebook Movement Range Data ('movement-range-2020-12-13.txt')
download from: https://data.humdata.org/dataset/movement-range-maps

> Social Connectedness Index Matrix file ('SCI_matrix.csv')
Uploaded to Drive: https://drive.google.com/drive/u/1/folders/1k6f2UcTTdDui_Y5wR17EKIMripRx8qjQ


> Safegraph Mobility Data ('weekly_df_updated_dates_feb20.csv')
Uploaded to Drive: https://drive.google.com/drive/u/1/folders/1k6f2UcTTdDui_Y5wR17EKIMripRx8qjQ

> max_temp = pd.read_csv(input_directory + 'max_temp_df.csv', dtype={'GEOID':str})
Should be produced for the desired date

> min_temp = pd.read_csv(input_directory + 'min_temp_df.csv', dtype={'GEOID':str})
Should be produced for the desired date

> Flow Connectedness Index Matrices
Uploaded to Drive: https://drive.google.com/drive/u/1/folders/1Z5KTcz5LZjGG_SepZAYJ8BgJ8ZACz6nd


## STXGB Model

This notebook contains the code for developing STXGB models. STXGB has three variants (STXGB-FB, STXGB-SG, and STXGB-SGR) and first we define the features that we have used in each variant. Then for each prediction horizon, we train a separate model using XGBoost algorithm.

Input Files needed to run this notebook:


> Output of Feature Generation Notebook (called 'all_features_v1.csv')

We have provided this file for you, so you don't need to run the Data Preparation notebook. 

> US counties ('Conterminous_US_counties.geojson')
Uploaded to Drive: https://drive.google.com/drive/u/1/folders/1k6f2UcTTdDui_Y5wR17EKIMripRx8qjQ



## Generate Prediction Intervals

This notebook is used to generate 95% prediction intervals. The XGBoost algorithm does not support interval prediction, therefore we have used Stochastic Gradient Boosting Regressor for generating interval predictions.


For each time step, we train 3 models. One for point predictions (which is only used for comparison with XGB predictions), one for lower quantile prediction, and one for upper quantile prediction. The latter two use a `quantile` loss (`alpha`= 0.025 and 0.975 respectively) whereas the first one uses `neg_root_mean_squared_error`.


Sections 1 and 2 of this notebook is very similar to the main notebook (`STXGB model`).


## Acknowledgement

_This work was supported by the Population Council, Inc. and the University of Colorado Population Center (CUPC) funded by Eunice Kennedy Shriver National Institute of Child Health & Human Development of the National Institutes of Health (P2CHD066613). The content is solely the responsibility of the authors, and does not reflect the views of the Population Council, Inc. or official views of the NIH, CUPC, or the University of Colorado._
