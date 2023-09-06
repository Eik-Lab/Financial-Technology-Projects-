# US Company Bankruptcy Prediction Datase

Accounting Data of NYSE and NASDAQ Companies (1999-2018)

## Dataset description

We present a unique dataset for predicting bankruptcy among American public companies listed on the New York Stock Exchange and NASDAQ. This dataset includes accounting data from 8,262 distinct companies spanning the years 1999 to 2018.

In accordance with the Securities and Exchange Commission (SEC), a company in the American market is considered bankrupt under two circumstances. Firstly, when the firm's management files for Chapter 11 of the Bankruptcy Code, indicating an intention to "reorganize" its business. In this scenario, the company's management continues to oversee day-to-day operations, but major business decisions require approval from a bankruptcy court. Secondly, if the firm's management files for Chapter 7 of the Bankruptcy Code, indicating a complete cessation of operations and the company's closure.

In our dataset, the fiscal year before the bankruptcy filing under either Chapter 11 or Chapter 7 is marked as "Bankruptcy" (1) for the following year. Conversely, if the company doesn't experience these bankruptcy events, it is considered to be operating normally (0). The dataset is comprehensive, with no missing values, synthetic entries, or imputed data.

This resulting dataset consists of a total of 78,682 observations of firm-year combinations. To facilitate model training and evaluation, we divided the dataset into three subsets based on time periods. The training set includes data from 1999 to 2011, the validation set covers data from 2012 to 2014, and the test set encompasses the years 2015 to 2018. The test set serves as a means to assess the predictive performance of models in real-world scenarios involving unseen cases.

Link to dataset - https://www.kaggle.com/datasets/utkarshx27/american-companies-bankruptcy-prediction-dataset
