
# 💸 Finora — Smart Expense Tracker

**Track expenses. Understand spending. Make smarter financial decisions.**

Finora is a Python-based smart expense tracking application that combines machine learning with an interactive dashboard to help users record transactions, automatically categorize expenses, monitor monthly budgets, and analyze spending patterns.

Built with **Python, Streamlit, FastAPI, SQLite, and Scikit-learn**, Finora demonstrates how machine learning can be integrated into a full-stack data-driven application.

---

## ✨ Features

### 📊 Interactive Financial Dashboard
- View total tracked spending and transaction counts.
- Analyze expenses over a selected date range.
- Compare spending with the preceding period of equal duration.
- Visualize spending trends using interactive area charts.
- Explore category-wise spending using interactive pie charts.
- Review recent transactions and identify entries that need attention.

### 🤖 AI-Powered Expense Categorization
- Predict expense categories from transaction descriptions.
- Use a trained machine learning classifier to generate category suggestions.
- Display prediction confidence scores.
- Flag low-confidence predictions for manual review.
- Preview a prediction before saving a transaction.

### 💳 Transaction Management
- Add transactions with descriptions, amounts, and dates.
- Automatically generate category predictions when saving transactions.
- View saved transaction history.
- Search transactions by description.
- Filter transactions by category and review status.
- Export filtered transaction records to CSV.

### 🎯 Monthly Budget Management
- Set an overall monthly spending limit.
- Create individual budgets for expense categories.
- Update existing budgets for a selected month and category.
- Monitor spending against saved budget limits.
- View remaining budget and percentage utilized.
- Receive warnings when budget utilization reaches 80%.
- Identify budgets that have been exceeded.
- Delete budgets when they are no longer needed.
- Store budgets persistently in SQLite.

### 🗄️ Persistent Data Storage
- Store transactions in a local SQLite database.
- Store monthly and category budgets in the same database.
- Retain saved records across application restarts.

---

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| Python | Application logic and data processing |
| Streamlit | Interactive frontend and dashboard |
| FastAPI | REST API and backend services |
| SQLite | Persistent transaction and budget storage |
| Scikit-learn | Machine learning classification |
| Pandas | Data manipulation and analysis |
| NumPy | Numerical computing |
| Plotly Express | Interactive data visualizations |
| Joblib | Loading the trained ML model |
| Requests | Communication between frontend and backend |
| Pydantic | API request validation |
| Uvicorn | Running the FastAPI application |

---

## 🏗️ Application Architecture

Finora separates the frontend, API, business logic, machine learning model, and database.

```text
                  ┌─────────────────────────┐
                  │     Streamlit UI        │
                  │     app.py              │
                  └────────────┬────────────┘
                               │ HTTP Requests
                               ▼
                  ┌─────────────────────────┐
                  │       FastAPI            │
                  │       main.py            │
                  └────────────┬────────────┘
                               │
                               ▼
                  ┌─────────────────────────┐
                  │    Expense Service      │
                  │  expense_service.py     │
                  └─────────┬───────────────┘
                            │
                 ┌──────────┴───────────┐
                 ▼                      ▼
       ┌──────────────────┐   ┌──────────────────┐
       │ Trained ML Model  │   │  SQLite Database │
       │ expense_classifier│   │   expenses.db    │
       └──────────────────┘   └──────────────────┘
```

**Workflow:**

1. The user interacts with the Streamlit dashboard.
2. Streamlit sends HTTP requests to the FastAPI backend.
3. The backend validates incoming data and calls the expense service.
4. The trained ML model predicts an expense category and confidence score.
5. Transactions and budgets are stored in SQLite.
6. The dashboard retrieves saved data and displays spending insights.

---

## 📁 Project Structure

```text
Smart Expense Tracker/
│
├── app.py
├── main.py
├── expense_service.py
├── requirements.txt
├── README.md
│
├── models/
│   └── expense_classifier.joblib
│
└── expenses.db
```

**File descriptions:**

- `app.py` — Streamlit frontend, dashboard, charts, transaction forms, and budget management interface.
- `main.py` — FastAPI application, API endpoints, and request validation.
- `expense_service.py` — Machine learning predictions, transaction persistence, database initialization, and budget calculations.
- `requirements.txt` — Python dependencies.
- `models/expense_classifier.joblib` — Serialized, trained expense classification model.
- `expenses.db` — Local SQLite database created by the application.

> The database file is generated locally. It should generally not be committed to a public repository if it contains personal financial information.

---

## ⚙️ Installation and Setup

### Prerequisites

Install the following before running the project:

- Python 3.10 or a compatible version supported by your installed dependencies.
- Git.
- A trained expense classification model saved at `models/expense_classifier.joblib`.

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/finora-smart-expense-tracker.git
```

Navigate into the project directory:

```bash
cd finora-smart-expense-tracker
```

Replace `YOUR_USERNAME` and the repository name with your actual GitHub details.

### 2. Create a virtual environment

**Windows:**

```bash
python -m venv .venv
```

Activate it:

```bash
.venv\Scripts\activate
```

**macOS/Linux:**

```bash
python3 -m venv .venv
```

```bash
source .venv/bin/activate
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
```

```bash
pip install -r requirements.txt
```

### 4. Verify the machine learning model

Ensure the following file exists:

```text
models/expense_classifier.joblib
```

The expense service loads this model when imported. If the file is missing or incompatible, the backend will fail to start.

If you distribute the model separately, document how to obtain it and where to place it.

### 5. Start the FastAPI backend

Open a terminal in the project root and run:

```bash
python -m uvicorn main:app --reload
```

The backend should be available at:

- API root: http://127.0.0.1:8000/
- Health check: http://127.0.0.1:8000/health
- Interactive API documentation: http://127.0.0.1:8000/docs

Keep this terminal running.

### 6. Start the Streamlit frontend

Open a second terminal in the same project directory, activate the virtual environment, and run:

```bash
python -m streamlit run app.py
```

Streamlit will display a local URL, usually:

http://localhost:8501

Open that URL in your browser to use Finora.

### 7. Configure the API URL (optional)

By default, Streamlit connects to:

```text
http://127.0.0.1:8000
```

To use another backend URL, set the `EXPENSE_API_URL` environment variable before starting Streamlit.

**Windows PowerShell:**

```powershell
$env:EXPENSE_API_URL="http://127.0.0.1:8000"
python -m streamlit run app.py
```

**macOS/Linux:**

```bash
export EXPENSE_API_URL="http://127.0.0.1:8000"
python -m streamlit run app.py
```

---

## 🔌 API Endpoints

Finora exposes REST endpoints through FastAPI.

| Method | Endpoint | Description |
|---|---|---|
| GET | `/` | API welcome message |
| GET | `/health` | Check backend health |
| POST | `/predict` | Predict a category for a description |
| POST | `/transactions` | Create and save a transaction |
| GET | `/transactions` | Retrieve saved transactions |
| GET | `/budgets` | Retrieve budget status for a month |
| PUT | `/budgets` | Create or update a monthly/category budget |
| DELETE | `/budgets` | Delete a selected budget |

### Example: Predict an expense category

**Request**

```http
POST /predict
Content-Type: application/json
```

```json
{
  "description": "grocery store purchase"
}
```

The response includes the predicted category, confidence percentage, and whether manual review is recommended.

### Example: Create a transaction

```json
{
  "description": "grocery store purchase",
  "amount": 850,
  "transaction_date": "2026-10-21"
}
```

Send this body to `POST /transactions`.

### Example: Set a monthly budget

```json
{
  "month": "2026-10",
  "category": "Overall",
  "amount": 15000
}
```

Send this body to `PUT /budgets`.

To create a category-specific budget, replace `"Overall"` with the exact category name used by your saved transactions.

### Example: Retrieve budget status

```http
GET /budgets?month=2026-10
```

The response includes the configured budget, recorded spending, remaining amount, percentage used, and whether the limit has been exceeded.

For complete request and response schemas, use the interactive documentation at `/docs`.

---

## 🧠 Machine Learning Approach

Finora uses a trained Scikit-learn classification model to suggest categories based on transaction descriptions.

The prediction workflow is:

1. Accept a transaction description.
2. Generate class probabilities using the model.
3. Select the category with the highest predicted probability.
4. Calculate the prediction confidence.
5. Flag predictions below the configured confidence threshold for manual review.
6. Save the transaction with the resulting category and review status.

The current confidence threshold is configured in `expense_service.py`:

```python
CONFIDENCE_THRESHOLD = 0.40
```

A prediction below this threshold is labeled `Needs Review`.

**Important:** Model confidence is not a guarantee of correctness. Predictions should be checked, particularly for ambiguous merchant names and short descriptions.

---

## 💰 Budget Calculation

Finora calculates budget utilization using saved transactions for the selected calendar month.

**Remaining budget**

```text
Remaining = Budget Limit - Recorded Spending
```

**Budget utilization**

```text
Budget Utilization (%) =
    (Recorded Spending / Budget Limit) × 100
```

The dashboard displays progress and alerts based on the calculated utilization.

The overall budget includes all recorded transactions for that month. A category budget includes transactions assigned to that exact category.

These are tracking limits rather than transaction-blocking controls. Recording an expense can take spending above the configured budget.

---

## 📈 Example Use Case

Suppose a user configures the following budgets for October 2026:

| Budget | Monthly limit |
|---|---:|
| Overall | ₹15,000 |
| Food | ₹4,000 |
| Transport | ₹2,000 |
| Shopping | ₹3,000 |

As transactions are recorded, Finora calculates the spending against each applicable budget and displays the remaining amount and percentage used.

These values are illustrative, not preloaded application data.

---

## 🔒 Privacy and Security

Finora is designed as a local application prototype.

- Transaction and budget records are stored in a local SQLite database.
- Do not commit personal transaction data, database files, credentials, or API secrets to a public repository.
- Review your `.gitignore` before publishing.
- The current implementation does not provide user authentication or multi-user budget isolation.
- The default backend configuration is intended for local development, not direct public production deployment.
- AI-generated categories may be inaccurate and should be verified.

---

## 🧪 Troubleshooting

### Backend connection error

Ensure FastAPI is running:

```bash
python -m uvicorn main:app --reload
```

Check:

```text
http://127.0.0.1:8000/health
```

### Missing model file

Confirm that this file exists:

```text
models/expense_classifier.joblib
```

### Missing Python packages

Activate the correct virtual environment and run:

```bash
pip install -r requirements.txt
```

### Budget endpoints return 404

Make sure you have added the budget routes to `main.py` and restarted the backend.

### Budget table or data not appearing

Check that `initialize_database()` creates the `budgets` table and that the frontend and backend are using the same project directory and SQLite database.

### Incorrect category budget totals

Category budgets rely on exact category names. Check the categories stored in transaction history before creating a budget.

---

## 🚀 Future Improvements

Potential enhancements include:

- User registration, login, and account-specific data.
- Income tracking and cash-flow analysis.
- Savings goals and financial planning.
- Recurring transaction detection.
- Transaction import from CSV or bank statements.
- More advanced spending forecasts.
- Customizable expense categories.
- Improved model evaluation and retraining workflows.
- Unit and API integration tests.
- Deployment with production-grade configuration and monitoring.

---

## 👨‍💻 About the Project

Finora is a full-stack Python project that brings together machine learning, REST API development, data analysis, persistent storage, and interactive visualization.

It demonstrates practical skills in:

- Python application development.
- REST API design using FastAPI.
- Frontend development with Streamlit.
- Machine learning inference using Scikit-learn.
- Data processing with Pandas and NumPy.
- SQL and SQLite persistence.
- Data visualization with Plotly.
- Input validation and application integration.

---

## 📄 License

Choose a license before distributing this project publicly. For example, an MIT License allows reuse under its stated terms. Only add a license that you intend to use.

---

**Built with Python, machine learning, and a focus on smarter spending.** 💸
