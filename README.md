# COVID-STXGB
The repository for Spatio Temporal eXtreme Gradient Boosting (STXGB) model presented in the __Spatiotemporal Prediction of COVID-19 Cases using Inter- and Intra-County Proxies of Human Interactions__ paper.


__We are working on this code to make it modular and hence more accessible for the public. While v1.0 was release with the paper, please make sure that you use the latest version__

You must cite the paper above if you are planning to use the code.


### 1. System Requirements:

- To run this code you need to install Python (3.7 or higher) and all the required packages listed in the `environment.yml` file.
- `environment.yml` also includes the versions of each library that we have used. We recommend creating a new conda environment using this file. [Read this](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-from-an-environment-yml-file) to see how do to so.
-  This code does not require any non-standard hardware to run.

### 2. Installation Guide

Technically, you do not need to install anything (other than the libraries mention above) to run our code. We have published the code in the form of Jupyter Notebooks to make it easier for anyone interested to replicate our work. 

There are three notebooks in this repository as follows:

#### Data Preparation

A notebook for generating training data for ML models

    Input Files needed to run this notebook are as follows:

    - US counties GeoJSON ('Contiguous_US.geojson')
      > [source](https://www.census.gov/geographies/mapping-files/time-series/geo/carto-boundary-file.html)

    - COVID-19 case data from John Hopkins ('time_series_covid19_confirmed_US.csv') 
      > [source](https://github.com/CSSEGISandData/COVID-19/tree/master/csse_covid_19_data/csse_covid_19_time_series)

    - Facebook Movement Range Data ('movement-range-2020-12-13.txt')
      > [source](https://data.humdata.org/dataset/movement-range-maps)

    - Social Connectedness Index Matrix file ('SCI_matrix.csv')
      > [source](https://data.humdata.org/dataset/social-connectedness-index)

    - Safegraph Mobility Data ('safegraph_mobility.csv')
      > [source](https://docs.safegraph.com/docs/social-distancing-metrics)

    - Daily Maximum and Minimum Temperature data (max_temp_df_2021.csv , min_temp_df_2021.csv)
      > [source](https://ftp.cpc.ncep.noaa.gov/GIS/GRADS_GIS/GeoTIFF/TEMP/) 

    - Flow Connectedness Index Matrices
     > Generated using the method described in the paper

__We have uploaded all of these files [here](https://drive.google.com/drive/u/1/folders/1laAZFCvsPLLaKDvg0isTMMr20kMe0x_r)__


#### STXGB Model

This notebook contains the code for developing STXGB models. STXGB has three variants (STXGB-FB, STXGB-SG, and STXGB-SGR) and first we define the features that we have used in each variant. Then for each prediction horizon, we train a separate model using XGBoost algorithm.

Input Files needed to run this notebook:


      - Output of Feature Generation Notebook (called 'all_features_v1.csv')    
      
      - US counties boundaries 
      
[Uploaded here](https://drive.google.com/drive/u/1/folders/1laAZFCvsPLLaKDvg0isTMMr20kMe0x_r)


#### Generate Prediction Intervals

This notebook is used to generate 95% prediction intervals. The XGBoost algorithm does not support interval prediction, therefore we have used Stochastic Gradient Boosting Regressor for generating interval predictions.


For each time step, we train 3 models. One for point predictions (which is only used for comparison with XGB predictions), one for lower quantile prediction, and one for upper quantile prediction. The latter two use a `quantile` loss (`alpha`= 0.025 and 0.975 respectively) whereas the first one uses `neg_root_mean_squared_error`.


Sections 1 and 2 of this notebook is very similar to the main notebook (`STXGB model`).


### 3. Demo

If you would like to replicate the STXGB model, e.g. with Facebook features (i.e. STXGB-FB) and for the 1-week prediction horizon, you should download the 'all_features_v1.csv' file and run the corresponding section of the `STXGB Model.ipynb` notebook (section 3 in this example)

      models = ['base', 'safegraph', 'facebook', 'safegraph_full']
      features = [base_features, safegraph_features, facebook_features, safegraph_full_features]

      # Setting Hyperparameters. Please refer to the SI for more information
      xgb_params = dict(learning_rate=np.arange(0.05,0.3,0.05), 
                           n_estimators=np.arange(100,1000,100), 
                           gamma = np.arange(1,10,1),
                           subsample = np.arange(0.1,0.5,0.05),
                           max_depth=[int(i) for i in np.arange(1,10,1)]) 



      for i in range(time_steps):

          training_df = covid_df.iloc[:(i+training_size)*num_counties,:]
          testing_df = covid_df.iloc[(i+training_size)*num_counties:(i+training_size+testing_size)*num_counties,:]

          for model,feature in zip(models, features):

              time_start = time.time()

              X_train = training_df[feature]
              y_train = training_df['LOG_DELTA_INC_RATE_T']
              X_test = testing_df[feature]
              y_test = testing_df['LOG_DELTA_INC_RATE_T'] 

              #scaling X
              scaler = MinMaxScaler()
              X_train = scaler.fit_transform(X_train)
              X_test = scaler.transform(X_test)
              .
              .
              .
              
The expected output of this cell is a (geopandas) geodataframe that contains, among others, the follwing columns per each model and forecast date:


      ['GEOID', 'y_test_', 'y_predicted_', 
        'delta_inc_test_',  'delta_inc_pred_',
        'delta_case_test_', 'delta_case_pred_',
        'error_y_', 'error_delta_inc_', 'error_delta_case_']
        
        
Each STXGB model was tuned and trained on a regular desktop machine (with a 6 core Ryzen 5 3600X CPU and 64GB of RAM) in approximately 12-13 minutes for a single prediction horizon, and thus, in almost one hour for all of the four prediction horizons.

### Acknowledgement

_This work was supported by the Population Council, Inc. and the University of Colorado Population Center (CUPC) funded by Eunice Kennedy Shriver National Institute of Child Health & Human Development of the National Institutes of Health (P2CHD066613). The content is solely the responsibility of the authors, and does not reflect the views of the Population Council, Inc. or official views of the NIH, CUPC, or the University of Colorado._
