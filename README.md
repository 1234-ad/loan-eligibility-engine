# Loan Eligibility Engine – AWS + n8n Project

## 🏗️ Architecture Overview
- **User Upload**: CSV to S3
- **Lambda**: Reads and writes to PostgreSQL
- **n8n Workflows**:
  - A: Loan Product Discovery (daily web crawler)
  - B: User-Loan Matching (triggered via webhook)
  - C: Email Notification (SES)

## 📁 Files Included
- `loan_products.csv` – sample scraped data
- `docker-compose.yml` – run n8n locally
- `workflow_a_loan_product_discovery.json`
- `workflow_b_user_loan_matching.json`
- `workflow_c_user_notification.json`
- `README.md` – this guide

## 🚀 Setup Instructions

### Step 1: PostgreSQL Tables (RDS)
```sql
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    name TEXT,
    email TEXT,
    monthly_income NUMERIC,
    credit_score INTEGER,
    employment_status TEXT,
    age INTEGER
);

CREATE TABLE loan_products (
    product_id SERIAL PRIMARY KEY,
    product_name TEXT,
    interest_rate NUMERIC,
    min_income NUMERIC,
    min_credit_score INTEGER
);

CREATE TABLE matches (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(user_id),
    product_id INTEGER REFERENCES loan_products(product_id),
    matched_on TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Step 2: Launch n8n
```bash
docker-compose up -d
```
Access at [http://localhost:5678](http://localhost:5678)

### Step 3: Load Workflows
Import the three workflows in the UI.

### Step 4: Trigger Flow
- Upload `users.csv` to S3
- Lambda will load data
- Trigger Webhook URL for matching

---
Built with ❤️ using n8n, AWS, and Python.
