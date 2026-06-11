# Boston House Price Prediction

🚀 **Live Application:** [https://boston-house-pricing-x2tj.onrender.com/](https://boston-house-pricing-x2tj.onrender.com/)


## 🚀 Project Overview
This project is an end-to-end Machine Learning web application that predicts the price of houses in Boston based on various features. It uses a Linear Regression model trained on the classic Boston Housing dataset. Users can interact with a sleek, user-friendly web interface to input parameters (like the number of rooms or pupil-teacher ratio) and instantly receive a predicted house price.

## ⚙️ How it Works
1. **The Machine Learning Model**: A `LinearRegression` model is trained on historical housing data using `scikit-learn`. The input features are mathematically standardized using a `StandardScaler` to improve the model's accuracy.
2. **Model Serialization**: Both the trained model (`regmodel.pkl`) and the scaler (`scaler.pkl`) are saved to the hard drive using Python's `pickle` library. This allows the web app to use the model instantly without having to retrain it every time the server starts.
3. **The Web Application**: A lightweight **Flask** web server (`app.py`) loads the saved model and scaler. When a user submits data through the HTML form on the website (`home.html`), the Flask app captures the input, scales it, passes it through the model, and renders the predicted price back onto the webpage using Jinja templating.

## 🧠 What I Learned
By building this project from scratch, I gained hands-on experience in several crucial areas of software engineering and data science:
- **Machine Learning Pipelines**: Loading data, train-test splitting, feature scaling, model training, and evaluating accuracy metrics (MAE, MSE, RMSE, R²).
- **Backend Integration**: Saving trained ML models into `.pkl` files and successfully connecting them to a live backend server.
- **Web Development (Flask)**: Building routing logic (`@app.route`), handling HTTP `POST` requests, and linking Python logic to HTML frontends.
- **Containerization (Docker)**: Writing a `Dockerfile` to package the entire application, its code, and its dependencies into a single, isolated container. This completely eliminates the "it works on my machine" problem!
- **Cloud Deployment**: Using **Render** to automatically build and host the Dockerized application live on the internet directly from a GitHub repository.

## 🛠️ Tools & Requirements
- Python 3.8+ (App is Dockerized with Python 3.11)
- `scikit-learn`, `pandas`, `numpy`, `Flask`, `gunicorn`
- Docker Desktop (for local container testing)

---

## 💻 Local Setup & Installation

### Option 1: Running with Python (Virtual Environment)
```bash
# 1. Create and activate a virtual environment
conda create -p venv python=3.8 -y
conda activate ./venv

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run the application
python app.py
```
*Note: Open your browser and go to `http://localhost:5000`*

### Option 2: Running with Docker (Recommended)
```bash
# 1. Build the Docker image
docker build -t house-price-app .

# 2. Run the Docker container
docker run -p 5000:5000 house-price-app
```
*Note: Open your browser and go to `http://localhost:5000`*

---

## ☁️ Cloud Deployment (Render)
This project is configured to be easily deployed on [Render.com](https://render.com) using Docker:
1. Push the code (including `.pkl` files) to a GitHub repository.
2. Create a new **Web Service** on Render and connect the repository.
3. Set the Runtime to **Docker**.
4. Click **Deploy**. Render automatically builds the container using the included `Dockerfile` and serves it live to the public.

---

## 🐛 Recent Fixes & Troubleshooting
- **NumPy Compatibility (`numpy._core`)**: Upgraded the Dockerfile base image to `python:3.11-slim` to fix a `ModuleNotFoundError` caused by version mismatches between the host machine (which trained the model using a newer NumPy version) and the container.
- **Missing Scaler in app.py**: Fixed a `NameError: name 'scaler' is not defined` issue by ensuring `scaler = pickle.load(open('scaler.pkl', 'rb'))` is executed before any predictions are made.
