# COVID-STXGB
The repository for Spatio Temporal eXtreme Gradient Boosting (STXGB) model presented in the __Predicting County-Level COVID-19 Cases using Spatiotemporal Machine Learning Through Social Media and Cell-Phone Data as Proxies for Human Interactions__ paper.

You must cite the paper above if you are planning to use the code.

There are two notebooks in this repository as follows:

## 1. Feature Generation

A notebook to generate training data for ML models

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


## 2. SpatioTemporal Autoregressive models

Machine Leaning Models for COVID-19 forecasts

Input Files needed to run this notebook:


> Output of Feature Generation Notebook (called 'combined_df.csv')

> The second output of Feature Generation Notebook which includes lagged data ('combined_df_lagged_y.csv')

> US counties ('Conterminous_US_counties.geojson')
Uploaded to Drive: https://drive.google.com/drive/u/1/folders/1k6f2UcTTdDui_Y5wR17EKIMripRx8qjQ

> COVID-19 case data from John Hopkins ('time_series_covid19_confirmed_US.csv')
download from: https://github.com/CSSEGISandData/COVID-19/tree/master/csse_covid_19_data/csse_covid_19_time_series

> COVID FOrecast Hub predictions
Download from https://github.com/reichlab/covid19-forecast-hub/tree/master/data-processed/COVIDhub-ensemble
