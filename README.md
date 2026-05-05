# Diamond Price Prediction using Linear Regression (v2)

## Project Overview

This project uses the Diamonds dataset to predict diamond prices using a Linear Regression model.

The objective is to:

- Build a regression model to estimate diamond prices
- Understand which features drive price changes
- Evaluate model performance
- Analyze prediction errors from a business perspective
- Improve prediction stability using log transformation

---

## Dataset

- Source: `diamonds.csv`
- Target: `price`

---

## Features Used

### Numerical Features

- `carat`
- `depth`
- `table`

### Categorical Features

- `cut`
- `color`
- `clarity`

The physical dimension columns `x`, `y`, and `z` were removed from the final model.

---

## Data Cleaning

The dataset was cleaned by removing invalid or extreme dimension values:

- Removed rows where `x`, `y`, or `z` were equal to 0
- Removed extreme `y` values above 30

---

## Data Preprocessing

- Selected relevant numerical and categorical features
- Converted categorical variables using `pd.get_dummies()`
- Used `drop_first=True` to avoid duplicate dummy variable traps
- Applied log transformation to the target variable:

```python
y = np.log(y)
```

- Converted predictions back to the original dollar scale using:

```python
preds_actual = np.exp(preds)
```

---

## Train-Test Split

- 75% training / 25% testing
- `random_state = 42`

---

## Model

```python
LinearRegression()
```

---

## Model Performance

After converting predictions back to the original price scale and capping extreme predictions at `$20,000`:

- RMSE: `~1653.95`
- R² Score: `~0.827`

### Interpretation

- The model predictions are off by about `$1,654` on average
- The model explains about `82.7%` of the variance in diamond prices
- Performance is decent, but weaker than the earlier raw-price model
- The log model is more stable conceptually, but still struggles with extreme predictions

---

## Feature Impact

Because the target variable was log-transformed, the coefficients are in log scale.

This means the coefficients should be interpreted as multiplicative effects on price, not direct dollar increases.

### Strongest Positive Feature

#### `carat`: `+2.197664`

A 1-unit increase in carat multiplies predicted price by:

```python
np.exp(2.197664)
```

Approximately:

```text
9x
```

This confirms that carat is the strongest driver of diamond price.

---

## Coefficients

```text
carat            2.197664
clarity_IF       1.043844
clarity_VVS1     0.956948
clarity_VVS2     0.942512
clarity_VS1      0.897893
clarity_VS2      0.832847
clarity_SI1      0.738373
clarity_SI2      0.551734
cut_Ideal        0.096474
cut_Very Good    0.059271
cut_Premium      0.054679
cut_Good         0.048817
table            0.005626
depth           -0.000504
color_F         -0.050737
color_E         -0.057926
color_G         -0.130353
color_H         -0.265753
color_I         -0.419101
color_J         -0.576223
```

---

## Key Coefficient Insights

### Positive Impact

- Higher `carat` strongly increases predicted price
- Higher clarity grades increase predicted price
- Better cut grades slightly increase predicted price

### Negative Impact

- Lower color grades reduce predicted price
- `color_J` has the strongest negative impact among color features
- `depth` has almost no meaningful impact in this model

---

## Error Analysis

The model produced some extreme predictions after converting log predictions back to the original price scale.

Example issue:

```text
Maximum predicted price: over $7,000,000
Maximum actual price: around $18,787
```

Because of this, predictions were capped at `$20,000` before evaluating RMSE and R².

### Key Insight

Log transformation helps stabilize the target, but converting predictions back with `np.exp()` can still create extreme prediction errors when the model overestimates in log scale.

---

## Business Impact

### Underprediction

- A diamond may be priced too low
- This can lead to direct revenue loss

### Overprediction

- A diamond may be priced too high
- This can reduce competitiveness
- Inventory may move slower

### Operational Insight

This model is useful for learning regression behavior and feature impact, but it should not be used as a final production pricing model without additional controls.

---

## Limitations

- Linear Regression still assumes a mostly linear relationship
- The target was log-transformed, so coefficients are not direct dollar amounts
- `np.exp()` can create extreme predictions when log predictions are too high
- Predictions needed to be capped at `$20,000`
- The model struggles with high-value diamonds
- The model cannot fully capture non-linear luxury pricing behavior

---

## Future Improvements

- Try `RandomForestRegressor`
- Try `GradientBoostingRegressor`
- Compare uncapped vs capped prediction performance
- Add interaction features
- Build a separate model for high-priced diamonds
- Evaluate errors by price range
- Use cross-validation for more stable evaluation

---

## Tech Stack

- Python
- Pandas
- NumPy
- Scikit-learn
