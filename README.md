# Diamond Price Prediction using Linear Regression

## Project Overview

This project uses the Diamonds dataset to predict diamond prices using a Linear Regression model.

The objective is to:
- Build a regression model to estimate diamond prices
- Understand which features drive price changes
- Evaluate model performance
- Analyze prediction errors from a business perspective

---

## Dataset

- Source: diamonds.csv
- Target: price

### Features Used

Numerical:
- carat
- depth
- table
- x, y, z

Categorical:
- cut
- color
- clarity

---

## Data Preprocessing

- Selected relevant numerical and categorical features
- Converted categorical variables using `pd.get_dummies()`
- Ensured all features are numeric before training

---

## Train-Test Split

- 75% training / 25% testing
- random_state = 42

---

## Model

- LinearRegression from sklearn

---

## Model Performance

- RMSE: ~1124
- R² Score: ~0.92

### Interpretation

- The model predictions are off by about 1124 on average
- The model explains 92% of the variance in diamond prices
- Performance is strong but not fully reliable at extreme values

---

## Feature Impact (Coefficients)

### Positive Impact

- carat (~ +11225)
  - Largest contributor to price increase

- clarity
  - IF: +5425
  - SI2: +2756
  - Higher clarity increases price

### Negative Impact

- color
  - J: -2382
  - E: -220
  - Lower color grades decrease price

---

## Error Analysis

- Large prediction errors are concentrated in high-priced diamonds
- The model struggles with extreme values
- Both overprediction and underprediction are present

### Key Insight

Linear Regression cannot fully capture non-linear pricing behavior, especially at higher price ranges.

---

## Business Impact

### Underprediction (High Risk)

- Diamonds may be undervalued
- Leads to direct revenue loss

### Overprediction

- Diamonds may be overpriced
- Can result in lower sales or unsold inventory

---

## Limitations

- Assumes a linear relationship between features and price
- Cannot capture non-linear patterns
- May produce unrealistic predictions (e.g., negative values)

---

## Future Improvements

- Apply log transformation to price
- Use tree-based models (Random Forest, Gradient Boosting)
- Improve feature engineering

---

## Tech Stack

- Python
- Pandas
- NumPy
- Scikit-learn
