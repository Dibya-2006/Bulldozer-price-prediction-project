# 🚜 Predict the Auction Sale Price of Bulldozers

This project is a machine learning regression case study aimed at predicting the sale price of bulldozers. It utilizes the **Kaggle Blue Book for Bulldozers** dataset to build a model that can predict future auction prices based on historical data and equipment characteristics.

## 1. Problem Statement

**How well can we predict the future sale price of a bulldozer, given its characteristics and previous examples of how much similar bulldozers have been sold for?**

## 2. Data Source

The data is sourced from the [Kaggle Blue Book for Bulldozers competition](https://www.kaggle.com/c/bluebook-for-bulldozers/data).
The dataset is split into three parts:

* **Train.csv:** The training set, which contains data through the end of 2011.
* **Valid.csv:** The validation set, which contains data from January 1, 2012 - April 30, 2012.
* **Test.csv:** The test set (labels are not provided to the public), containing data from May 1, 2012 - November 2012.

## 3. Features

The dataset contains over 50 variables including `SalesID`, `MachineID`, `ModelID`, and various equipment specifications.

> 📖 **Data Dictionary:** You can view a detailed description of all features in this [Google Sheets Data Dictionary](https://docs.google.com/spreadsheets/d/18ly-bLR8sbDJLITkWG7ozKm8l3RyieQ2Fpgix-beSYI/edit?usp=sharing).

---

## 4. Data Preprocessing & Engineering

Since this is time-series data, significant preprocessing was required:

### 📅 Time-Series Enrichment

I enriched the dataset by parsing the `saledate` column into multiple date-time attributes (Year, Month, Day, Day of Week, Day of Year). This allows the model to capture seasonal trends in auction prices.

### 🔢 Handling Categorical Data

Machine Learning models cannot process strings/objects directly. I converted all "object" columns into **Pandas Categorical** types, assigning numerical codes to each unique category.

### 🛠 Handling Missing Values

* **Numerical Features:** Missing values were filled with the **median** of the column (to avoid outliers affecting the mean). A binary column was added to track if a value was originally missing.
* **Categorical Features:** Missing categories were replaced with `0` (Pandas Categorical assigns `-1` to missing values by default, so I shifted these to 0). A binary indicator column was also added here.

---

## 5. Modeling

Given the size and complexity of the dataset (many features and rows), I chose the **Random Forest Regressor**. This ensemble method is robust to outliers and handles high-dimensional data effectively.

### Evaluation Metrics

Since the goal is to minimize the error between predicted and actual prices, I built an evaluation function to track:

* **MAE** (Mean Absolute Error)
* **$R^2$** (Coefficient of Determination)
* **RMSLE** (Root Mean Squared Log Error) — *The primary metric for this Kaggle competition.*

### Hyperparameter Tuning

I optimized the model using **RandomizedSearchCV** to find the best combination of hyperparameters (e.g., `n_estimators`, `max_depth`, `min_samples_leaf`) to improve generalization and reduce overfitting.

---

## 6. Results & Feature Importance

After training the optimized model, I generated predictions for the `test.csv` dataset.

I also calculated **Feature Importance** to identify which attributes (e.g., Year Made, Product Size, Enclosure) had the highest impact on the final sale price.

---

## How to use this repo

1. Clone the repository.
2. Download the data from Kaggle and place it in the `data/` folder.
3. Run the Jupyter Notebook: `Predicting_Bulldozer_Prices.ipynb`.
