# Predictive-Analytics-Dicoding
By: Noer Hanifah Suganda

# A. Background

A UK-based e-commerce company headquartered in London has been operating since 2007, offering products such as gifts and homewares for various customer segments, including individuals and small businesses. The dataset used in this analysis includes 500,000 transactions over one year, with 8 columns of information.

Although most transactions proceed smoothly, there is a small percentage of transactions that are canceled. These cancellations typically occur due to insufficient stock, causing customers to cancel the transaction in order to receive all products at once. Transaction cancellations not only disrupt operational flow but also impact revenue and customer satisfaction.

Given this situation, it is crucial to identify the factors influencing transaction cancellations and to develop a predictive model that can detect transactions at risk of being canceled. By applying machine learning approaches, the company can take proactive steps to optimize inventory management, reduce financial losses, and enhance the overall customer experience.

# B. Business Understanding

In the world of e-commerce, maintaining daily revenue is crucial. Fluctuating daily revenue can make it difficult for companies to manage inventory, plan promotions, and organize operations. If a company can predict when revenue is likely to drop or rise, they can respond more quickly—such as by increasing stock or adjusting promotions—thereby minimizing losses and maximizing sales opportunities.

**Problem Statement:**

- How to build a predictive model based on time series to forecast daily revenue from e-commerce transactions?
- What type of model has the best accuracy?

**Goals:**

- Developing a forecasting model that can predict daily sales revenue with measurable accuracy using the Mean Absolute Error (MAE) and Root Mean Squared Error (RMSE) metrics.
- Comparing several model algorithms to find the best accuracy in predicting daily sales revenue.

**Solution Statement**

To forecast daily revenue from e-commerce transactions, I use three solution approaches, each with measurable evaluation metrics (such as MAE and RMSE) so that the resulting solutions can be assessed objectively.

- Using the ARIMA model to model univariate time series data. ARIMA utilizes historical revenue patterns to directly capture trends and seasonality through its parameters (p, d, q). ARIMA provides a classic time series model that directly relies on historical patterns.

- Using Random Forest Regressor for daily revenue forecasting by utilizing engineered features (such as lag revenue, moving averages, etc.). This approach allows the model to capture non-linear relationships and interactions between features that traditional time series models may not be able to capture.

- Developing a deep learning model based on LSTM, designed to capture long-term dependencies in time series data. This approach is effective when daily revenue patterns have complex non-linear dynamics.

# C. Data Understanding

Source: https://www.kaggle.com/datasets/gabrielramos87/an-online-shop-business

This dataset contains more than 500,000 rows of data and 8 columns. Below is the description of each column:

1. TransactionNo (categorical): A unique six-digit number that defines each transaction. If the code contains the letter "C," it indicates that the transaction is a cancellation.
2. Date (numeric): Indicates the date when the transaction occurred.
3. ProductNo (categorical): A unique character string consisting of five or six digits, used to identify a specific product.
4. Product (categorical): The name of the product or item being sold.
5. Price (numeric): The price per unit of each product, expressed in British pounds (£).
6. Quantity (numeric): The quantity of products purchased in each transaction. Negative values indicate canceled transactions.
7. CustomerNo (categorical): A unique five-digit number used to identify each customer.
8. Country (categorical): The name of the country where the customer is located.

**Data Conditions**
- Duplicates: There are 5,200 duplicate rows which were removed.
- Missing Values: 55 null rows in the CustomerNo column were deleted.
- Data Types: The Date column was originally a string, and CustomerNo was a float; both columns were converted to their appropriate data types.
- Outliers: Outliers in Price and Quantity were identified using the IQR method and removed using a mask combination.

**Exploratory Data Analysis (EDA)**

Visualization & Univariate Analysis:
- Numeric: Histograms and boxplots for the Price and Quantity features (as well as Date converted to numeric form) were used to visualize the data distribution and detect outliers.
  
![image](https://github.com/user-attachments/assets/7219bd8b-38ac-44c6-8421-fce05330cee6)
![image](https://github.com/user-attachments/assets/d5782c87-346a-47ea-a9cf-11b13129caaa)
![image](https://github.com/user-attachments/assets/068aefc8-4e5e-425d-b815-ae77a650fd59)
![image](https://github.com/user-attachments/assets/84c053a3-b07e-44ae-9cec-9e3402c7bb84)
![image](https://github.com/user-attachments/assets/b4e8f759-1a0e-4b3c-83ac-4e702f348d11)
![image](https://github.com/user-attachments/assets/d2109128-7a95-4434-9c31-b8a872d697b9)

- Categorical: Frequency analysis (using value_counts) was performed on features such as ProductName, CustomerNo, TransactionNo, and Country.

![image](https://github.com/user-attachments/assets/392826e9-2c4d-4efa-8797-c6c4c126c4dd)
![image](https://github.com/user-attachments/assets/c28240ad-50e2-4584-b93f-77880586725c)
![image](https://github.com/user-attachments/assets/c5e259ee-8123-44c8-8156-b2f124943992)

Target Variable Analysis:
A target variable, IsCancelled, was created, determined based on whether the TransactionNo contains the letter "C" or if the Quantity is negative. The visualization of the target distribution (using a countplot) shows the comparison between canceled and non-canceled transactions.

![image](https://github.com/user-attachments/assets/70bb9da8-022c-4f26-add8-d950db4168d0)

Multivariate Analysis:
- Correlation Heatmap: The heatmap showing the correlation between Price and Quantity indicates a weak linear relationship.

![image](https://github.com/user-attachments/assets/2a424b36-7720-44ff-aab0-a67a404fa8b4)

- Pairplot: The pairplot illustrates that most normal transactions have a positive Quantity, while canceled transactions are marked by a negative Quantity.

![image](https://github.com/user-attachments/assets/a187e8ac-e682-48da-bdf7-d2cc461f9578)

- Correlation Heatmap between Numeric Features

![image](https://github.com/user-attachments/assets/7792b19b-4b06-4a2b-b8e3-17d57c995e70)

Quantity has a dominant influence on Revenue (with a high correlation of 0.98), while Price only has a very weak linear relationship with Revenue (-0.075).

Price and Quantity are also weakly negatively correlated (-0.21), meaning that when Price increases, Quantity might slightly decrease, but the change is not significant.


![image](https://github.com/user-attachments/assets/60d6a194-5a6b-49e2-b14c-57b99bd626f6)

Price vs. Revenue Relationship:
No strong linear pattern is observed; many points are scattered randomly. A correlation of -0.075 indicates almost no linear relationship. This means that the price of the items does not directly determine the amount of revenue generated.

Quantity vs. Revenue Relationship:
A clear diagonal line is visible, indicating a strong positive relationship (correlation ≈ 0.98). The higher the Quantity, the higher the Revenue generated.

Price vs. Quantity Relationship:
The spread of points does not form a clear linear pattern. There is a tendency that items with higher Price are sometimes purchased in lower Quantity, but the relationship is not very strong. This aligns with the weak negative correlation (-0.21).

Conclusion:
- Quantity is the key determinant of Revenue (due to the very high correlation).
- Price has only a very weak linear relationship with Revenue and tends to negatively affect Quantity (more expensive items are bought in smaller quantities, but not significantly).
- Revenue primarily varies based on how much of the product is purchased (Quantity).


# D. Data Preparation

Here’s the translation with the symbols preserved:

---

**1. Creating Cancellation and Revenue Labels**
A column **IsCancelled** was created to mark whether the transaction was canceled (if **TransactionNo** contains 'C' or if **Quantity** < 0).

The **Revenue** column is calculated as the product of **Price** and **Quantity** for non-canceled transactions, while for canceled transactions, **Revenue** is set to 0.

**2. Selecting Only Successful (Non-Cancelled) Transactions**
For forecasting revenue, we only accumulate non-canceled transactions. This is stored in **df\_success**.

**3. Aggregating Daily Revenue**
The data is then grouped by date to get the total daily revenue.

**4. Creating Time Series Features**
Lag features (e.g., **Lag1**, **Lag7**, **Lag30**) were created to represent the revenue of the previous day as input for the model.

Moving average features (e.g., **MA7**, **MA30**) were also created to capture short-term and long-term trend patterns.

**5. Scaling Numeric Features**
Numeric features such as **Revenue**, **lag**, and **moving average** were scaled using **StandardScaler** to ensure uniform value ranges, making it easier for model training.

**6. Splitting Data into Training and Testing Sets**
The time series data was split chronologically (without shuffling) into training and testing sets (e.g., 80% training and 20% testing) to preserve the time order.

# E. Model Development 

Three algorithms used:

### **1. Random Forest Regressor**

#### **Modeling Steps**:

* Additional features such as lag and moving average have been created to capture temporal patterns.
* **Data Splitting**: The data is split chronologically into training and testing sets.
* **Model Training**: The **Random Forest Regressor** model is trained on the training data with scaled features.
* **Prediction and Evaluation**: The model is used to predict revenue on both the training and testing data, then evaluated using metrics such as **MAE**, **RMSE**, and **R²**.

#### **Key Parameters**:

* **n\_estimators**: The number of trees in the ensemble (100).
* **max\_depth**: The maximum depth of each tree (10) to reduce complexity and prevent overfitting.
* **random\_state**: Ensures reproducibility of results.

#### **Advantages**:

* Capable of capturing non-linear relationships and interactions between features very effectively.
* More robust against noise and overfitting due to the use of multiple trees (ensemble learning).
* Can leverage engineered features like lag and moving averages, enriching the model’s information.

#### **Disadvantages**:

* Requires more computational resources, making training slower.
* The internal interpretation of the model is less transparent because it is an ensemble of many trees.
* If parameters are not well-tuned, the model may still overfit the training data.

  ![image](https://github.com/user-attachments/assets/16fa79c8-511c-45b9-b3d2-e3527ccf8a59)

  ![image](https://github.com/user-attachments/assets/db169969-f242-435e-a478-bd7af08607fd)

**2. ARIMA**
Here’s the translation for the **ARIMA** modeling steps and details:

---

### **Modeling Steps**:

* The data is sorted by date, and the **Date** column is set as the time series index.

* The data is split chronologically (e.g., 80% training, 20% testing) to maintain the time sequence.

* **Determining Order (p, d, q)**: **ACF/PACF** analysis is used to determine the value of **p** (autoregressive order) and **q** (moving average order). **d** represents the number of differencing required to make the data stationary.

* **Fitting the Model**: An **ARIMA** model with a specific order (e.g., ARIMA(1,1,1)) is fitted on the training data.

#### **Key Parameters**:

* **p**: The number of lags in the autoregressive component.
* **d**: The number of times differencing is applied for stationarity.
* **q**: The number of lags in the moving average component.
  Example: ARIMA(1,1,1)

#### **Advantages**:

* Suitable for univariate time series data where trend and seasonality patterns are clear.
* Simple model that is easy to interpret.
* Many references and tuning methods are available.

#### **Disadvantages**:

* Only models one variable (univariate), so it cannot explicitly use external features or engineered features like lag and moving averages.
* Sensitive to the determination of order parameters and requires stationary data.
* Not capable of capturing complex non-linear relationships.
![image](https://github.com/user-attachments/assets/922284cc-a598-4498-a4de-cb1b05616a7a)

**3. LSTM (Long Short-Term Memory)**
Here’s the translation for the **LSTM model** steps and details:

---

### **Modeling Steps**:

* The data is reshaped into 3D format (samples, time\_steps, features) as required for LSTM input.
* **Building the Model**: The **LSTM model** is built using the Sequential API, with one LSTM layer and one Dense layer as the output layer.
* **Model Compilation**:
  The model is compiled using the Adam optimizer and the **MSE (Mean Squared Error)** loss function because the target is a continuous value.
* **Model Training**:
  The model is trained for a certain number of epochs (50 epochs) with a batch size of 32 and validated on the testing data.
* **Prediction and Evaluation**:
  The model generates revenue predictions on the testing data, which are then evaluated using metrics such as **MAE** and **RMSE**.

#### **Key Parameters**:

* **Number of LSTM Units**: 50 units, which determines the complexity of the temporal representation.
* **Activation Function**: ReLU, to capture non-linearity.
* **Epochs**: The number of training iterations (50).
* **Batch Size**: The number of samples processed before updating the parameters (32).
* **Learning Rate**: Parameter in the Adam optimizer (e.g., 0.001).

#### **Advantages**:

* Highly effective for time series data with long-term dependencies.
* Capable of capturing non-linear relationships and complex patterns in the data.
* Suitable for large and dynamic datasets.

#### **Disadvantages**:

* Requires a large amount of data and longer training time compared to traditional models.
* Requires more careful hyperparameter tuning.
* The interpretation of deep learning models like LSTM is more difficult compared to traditional models.
  ![image](https://github.com/user-attachments/assets/d09c07b8-4db0-4036-b310-4f6b9da0140f)

# F. Model Evaluation 

After developing the model with three approaches (ARIMA, RandomForestRegressor, and LSTM), the following results were obtained:

**- RandomForestRegressor:**

* **Testing MAE**: 0.7630
* **Testing RMSE**: 1.0317

This model provides relatively low error on the testing data, indicating that the daily revenue predictions are closer to the actual values. Since **Random Forest** utilizes feature engineering (lag, moving average) to capture non-linear patterns in the time series data, the results support **Goal 1** (forecasting daily revenue with low error).

**- ARIMA:**
* **MAE**: 1.3104
* **RMSE**: 1.6240

Although ARIMA is a classic time series approach, the error is higher, making this model less optimal compared to **Random Forest** for this dataset. The ARIMA results show that the univariate approach does not adequately capture the complex patterns in the data, meaning **Goal 1** is not optimally achieved when using ARIMA alone.

**- LSTM :**

* **MAE**: 0.9514
* **RMSE**: 1.2049

The **LSTM model**, which is a deep learning approach for time series, shows fairly good performance, but its error is still slightly higher than that of **Random Forest**. The LSTM results support **Goal 1**, as LSTM can capture long-term dependencies. However, compared to **Random Forest**, the performance of LSTM has not yet reached the highest level of accuracy.

![image](https://github.com/user-attachments/assets/178df41c-bdcb-47ec-9b05-38a23ae4525a)

**Conclusion**
In the context of forecasting daily revenue, the Random Forest Regressor model shows the best performance based on MAE and RMSE metrics. LSTM provides fairly good results and is an interesting alternative, especially if the data has complex long-term dependency patterns. However, if the primary goal is prediction accuracy and generalization, Random Forest is more recommended.
