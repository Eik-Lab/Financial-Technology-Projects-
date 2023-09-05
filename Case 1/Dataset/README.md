# Open datasets

**Search:** https://www.kaggle.com/datasets?search=electricity+price



<br>

# Electricity price forecasting with DNNs (+ EDA)

Link to dataset - https://www.kaggle.com/code/dimitriosroussis/electricity-price-forecasting-with-dnns-eda

## Dataset description
- Electricity has transitioned from a basic need to a tradeable commodity due to deregulation, exemplified by Spain's Electric Power Act 54/1997.
- The project aims to predict next-hour electricity prices using deep neural network architectures and XGBoost, employing past electricity prices and other features like weather conditions.
- The study involves meticulous data cleaning, time series analysis, and feature engineering, serving as a comprehensive guide for aspiring data scientists.
- Five different neural network architectures (LSTM, stacked LSTM, CNN, CNN-LSTM, Time Distributed MLP) were initially tested for both univariate and multivariate forecasting.
- Additional approaches like Encoder-Decoder architecture and XGBoost regressor are also explored.
- Models use 25 previous time-steps of all features after applying Principal Component Analysis (PCA).
- Adam optimizer is used in all deep learning architectures, with learning rate determined through preliminary tests.
