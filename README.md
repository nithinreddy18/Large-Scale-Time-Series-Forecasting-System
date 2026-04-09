# PeriShield AI: Large-Scale Time Series Forecasting System

PeriShield AI is an end-to-end, production-ready system designed to predict the daily demand for perishable retail goods. By utilizing advanced machine learning techniques, the product bridges the gap between historical sales data and predictive analytics to minimize retail waste and maximize profitability. 

![Project Overview](https://img.shields.io/badge/Status-Complete-success) ![License](https://img.shields.io/badge/License-MIT-blue)

---

## 🎯 Purpose and Problem Solved

### The Problem
Retailers dealing in perishable goods (such as fresh produce, dairy, and meats) constantly perform a balancing act:
- **Overstocking** leads to massive food waste, expiring inventory, and financial loss.
- **Understocking** leads to empty shelves, poor customer experience, and missed revenue.
- Demand behavior is highly complex, affected simultaneously by **day-of-week seasonality, holidays, promotions, store locations**, and non-linear historical trends.

### The Solution: PeriShield AI
PeriShield AI accurately forecasts demand across tens of thousands of SKU-to-Store combinations (handling scaling effectively to millions of records). It solves the problem by providing a robust hybrid AI pipeline that simultaneously captures immediate temporal context and overarching categorical trends. This gives supply chain teams actionable predictions to align inventory ordering with true market demand.

---

## 🛠️ Technical Stack & Architecture

This project is structured as a full-stack ML application combining cutting-edge data science with modern web development.

### Machine Learning & Data Science
- **Scikit-Learn & XGBoost** (Random Forest): Learns broad, tabular feature associations (e.g., how "medium-sized stores in the Northeast" respond to "Dairy" promotions).
- **PyTorch** (LSTM): Deep learning model built to interpret sequential dependencies, grasping lag features and rolling window statistics across time.
- **Meta-Learner / Hybrid Stacking**: An algorithmic ensemble that aggregates predictions from both the RF and the LSTM models to yield high-confidence, smoothed ultimate forecasts over time.
- **Pandas & NumPy**: For extensive data wrangling, missing-value imputation (forward fill, interpolation), and categorical encoding.

### Backend Development
- **FastAPI**: Serves the predictions asynchronously with incredibly low latency. Handles JWT-based authentication.
- **SQLAlchemy & PostgreSQL**: Manages persistent storage of generated inventory data, metrics, and forecasts.
- **Pydantic**: Enforces strict payload validation and response schema definition.
- **Pytest**: Full-suite automated unit testing across data splits and model classes.

### Frontend Development
- **React.js (Vite)**: A blazing-fast single-page application framework.
- **Tailwind CSS**: Utility-first styling for a beautiful, responsive, and luxury-tier design aesthetic.
- **Recharts**: Provides deep dive interactive data visualization components (e.g., overlaying predicted demand vs true historical demand).

### Infrastructure & Deployment
- **Docker & Docker Compose**: Containerizes the frontend, backend, and PostgreSQL database for immutable setup and 1-click execution.
- **NGINX**: Used as the reverse proxy for high-traffic environments. 

---

## 🚀 Setting Up the Project

Follow these detailed steps to set up this system on your local machine and execute end-to-end training and inference.

### Prerequisites
Make sure you have the following installed on your machine:
- **Python 3.9+**
- **Node.js 20+** and **npm**
- **Docker & Docker Compose** (Optional, but recommended for clean deployment)
- **Git**

### Phase 1: Environment & Repository Setup
1. Clone the repository and navigate into the root directory:
   ```bash
   git clone https://github.com/nithinreddy18/Large-Scale-Time-Series-Forecasting-System.git
   cd Large-Scale-Time-Series-Forecasting-System
   ```
2. Create and activate a Python virtual environment:
   ```bash
   python3 -m venv venv
   source venv/bin/activate  # On Windows use: venv\Scripts\activate
   ```
3. Install backend ML requirements:
   ```bash
   pip install -r requirements.txt
   ```

### Phase 2: Generating Data & Training the AI
*Note: Because this system focuses on "Large-Scale" forecasting, we generate realistic synthetic data locally to mimic large retail datasets.*

1. **Run the Data Generator**
   This script builds over `8.2 million` records of synthetic retail data across 100 stores and 100 SKUs spanning two years.
   ```bash
   export PYTHONPATH=.
   python ml/data_generator.py
   ```
2. **Train the ML Pipeline**
   This script trains the Random Forest, PyTorch LSTM, and the Stacking Meta-Learner, saving artifacts directly to `ml/artifacts/`.
   *Note: This process may take a few minutes depending on your CPU/GPU hardware.*
   ```bash
   python ml/train.py
   ```

### Phase 3: Running the Full Stack Application

#### Option A: Running Locally (Development Mode)
**1. Start the Backend API**
Keep your virtual environment activated and launch the FastAPI server:
```bash
uvicorn backend.main:app --host 0.0.0.0 --port 8000 --reload
```
You can now access the interactive swagger docs at: `http://localhost:8000/docs`.

**2. Start the Frontend Dashboard**
Open a new terminal window, navigate to the frontend folder, and launch Vite:
```bash
cd frontend
npm install
npm run dev
```
Navigate to `http://localhost:5173` to view the comprehensive analytics dashboard! Login with username `admin` and password `admin123`.

#### Option B: Docker Container Deployment (Production)
If you prefer a 1-click cloud-ready setup that boots the API, PostgreSQL, and React Nginx server automatically:
```bash
docker-compose up --build -d
```
- **App Dashboard**: `http://localhost:3000`
- **Backend API**: `http://localhost:8000/api`

---

## 🧪 Testing the Codebase
Test the system using the pre-configured PyTest suite to validate preprocessing boundary limits, categorical encoding integrity, and PyTorch gradients:
```bash
source venv/bin/activate
pytest tests/test_pipeline.py -v
```

## 📁 Project Directory Structure
- **`/ml`**: Core algorithms. Contains the dataset generator, LSTM model architecture, RF forecaster, and heavy data preprocessing routines.
- **`/backend`**: The FastAPI application serving inference routes, checking platform health, and authenticating users.
- **`/frontend`**: React source code, custom React hooks, routing, and beautiful Tailwind components.
- **`/docker`**: Standalone Dockerfiles bridging local execution to production deployment.
- **`/tests`**: Comprehensive unit tests validating behavior across edge cases.
