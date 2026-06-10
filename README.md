# Boston House Price Prediction

## Tools Requirements

- Python 3.8+
- pip
- virtualenv (optional)
- Jupyter Notebook
- Required Python packages listed in `requirements.txt`
## Create a new Invirnment
```bash
conda create -p venv python==3.7 -y
```

## Project Overview
This project builds a simple regression model to predict house prices using the classic **Boston Housing** dataset. The workflow demonstrates typical steps in a machine‑learning pipeline, from data loading to model persistence.

## Steps Implemented
1. **Load the dataset**
   ```python
   from sklearn.datasets import fetch_openml
   boston = fetch_openml(name="Boston", version=1, as_frame=True)
   ```
   - Loaded into a Pandas DataFrame (`boston.data`).
   - Target variable stored in `boston.target`.

2. **Feature selection**
   - Selected a subset of features (e.g., `['RM', 'LSTAT', 'PTRATIO']`).
   - Adjust this list to match the columns you actually used.

3. **Train‑test split**
   ```python
   from sklearn.model_selection import train_test_split
   X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
   ```

4. **Scaling**
   ```python
   from sklearn.preprocessing import StandardScaler
   scaler = StandardScaler()
   scaler.fit(X_train)
   ```
   - The scaler is later saved to `scaler.pkl`.

5. **Model training**
   ```python
   from sklearn.linear_model import LinearRegression
   model = LinearRegression()
   model.fit(X_train, y_train)
   ```
   - The trained model is saved to `regmodel.pkl`.

6. **Model persistence**
   ```python
   import pickle
   with open('regmodel.pkl', 'wb') as f:
       pickle.dump(model, f)
   with open('scaler.pkl', 'wb') as f:
       pickle.dump(scaler, f)
   ```

7. **Evaluation metrics**
   ```python
   from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score
   mae = mean_absolute_error(y_test, reg_pred)
   mse = mean_squared_error(y_test, reg_pred)
   rmse = np.sqrt(mse)
   r2 = r2_score(y_test, reg_pred)
   # Adjusted R² (only if n > p + 1)
   n = len(y_test)
   p = X_test.shape[1]
   adj_r2 = 1 - (1 - r2) * (n - 1) / (n - p - 1) if (n - p - 1) > 0 else float('nan')
   ```
   - Printed MAE, MSE, RMSE, R² and Adjusted R².

8. **Making a prediction on a new sample**
   ```python
   sample = X[0].reshape(1, -1)               # raw sample
   scaled = scaler.transform(sample)          # apply same scaling
   pred = model.predict(scaled)
   print(f"Prediction for first house: {pred[0]:.3f}")
   ```

## Files Created
- `regmodel.pkl` – Serialized LinearRegression model.
- `scaler.pkl`   – Serialized StandardScaler used during training.
- `README.md`    – This documentation file.

## How to Run
```bash
# Activate virtual environment if not already active
source venv/Scripts/activate   # Windows PowerShell

# Install requirements (if needed)
pip install -r requirements.txt

# Launch the notebook
jupyter notebook implimentation.ipynb
```

Open the notebook and execute the cells sequentially. The model will be trained, evaluated, and saved. You can later reload the model and scaler with `pickle.load` to make predictions on new data.

---
