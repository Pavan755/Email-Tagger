🌟 Multi-Tenant Email Tagging System
🧠 NLP · 📩 Email Classification · 🛡️ Customer Isolation · ⚙️ Hybrid ML
<p align="left"> <img src="https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge"> <img src="https://img.shields.io/badge/ML-Naive%20Bayes-orange?style=for-the-badge"> <img src="https://img.shields.io/badge/NLP-TF--IDF-green?style=for-the-badge"> <img src="https://img.shields.io/badge/Notebook-Colab%20%7C%20Jupyter-lightgrey?style=for-the-badge"> <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge"> </p>
👤 Author
   B. Pavan Kumar

📘 Project Overview

This project builds an intelligent email tagging system designed for a multi-tenant environment.
Each customer has:

-Their own tags

-Their own model

-Their own rules

-This ensures zero tag-leakage and highly accurate tagging, even with a tiny dataset.

The system uses a hybrid classification approach:

🔹 Keyword-based rule engine (primary)

🔹 TF-IDF + Naive Bayes ML model (fallback)

🔹 Guardrails based on confidence scores & stopwords

This produces stable, predictable, explainable results.

⭐ Key Features

✨ Customer-specific tagging
✨ Rule-based deterministic prediction
✨ ML fallback for unseen patterns
✨ Zero cross-customer contamination
✨ Low-confidence guardrail (needs_manual_review)
✨ Fully explainable & easy to extend

🧠 Architecture Overview
Incoming Email
      │
      ▼
Customer Isolation Layer
      │
      ▼
Pattern Rule Engine (Primary)
      │
      ▼
ML Fallback Model
      │
      ▼
Guardrail Check (confidence + stopwords)
      │
      ▼
Final Tag OR needs_manual_review

🧩 Implementation Details
🔹 1. Data Processing

Combined subject + body into one text field

Applied custom stopwords:
"issue", "problem", "help", "error", "support", "email"

Lowercased text for uniformity

🔹 2. Pattern Rule Engine (Keyword → Tag)

Examples:

Keyword(s)	Output Tag
invoice, charged	billing
mail merge, CSV	mail_merge_issue
pending, resolved	status_bug
CSAT, dashboard	analytics_issue
delay, seconds, loading	performance

Rules fire before ML to ensure high precision and explainability.

🔹 3. Machine Learning Component

TF-IDF Vectorizer

Multinomial Naive Bayes

Separate model per customer

Predicts only within allowed customer tags

Confidence score extracted for guardrails

🔹 4. Guardrails

Ensures safe, controlled predictions:

Remove generic misleading words

If model confidence < 0.5:
→ Output: needs_manual_review

This prevents accidental wrong tagging.

🔐 Customer Isolation Strategy

Isolation is enforced at every level:

Independent ML model per customer_id

Customer-specific tag list

Pattern rules filtered by customer

Prediction pipeline selects tenant-aware components

This guarantees zero tag leakage between customers.

🧪 Error Analysis
🔸 ML-Only Evaluation

Performed leave-one-out on each customer (3 samples each):

ML trained on 2 samples → tested on 1

Dataset too small → accuracy near zero

This is expected with tiny datasets

🔸 Hybrid System Evaluation

Using rules + ML + guardrails:

12/12 predictions correct

All via rule-based logic

No tag leakage

Completely stable behavior

This demonstrates the strength of hybrid architecture.

🚀 Production-Ready Enhancements
🌐 1. Multi-Tenant Transformer Architecture

Shared encoder

Tenant-specific classification heads

High scalability + accuracy

🔁 2. Human Feedback Loop

Learn from agent corrections

Automated rule discovery

Weekly retraining

Drift detection

🛡️ 3. Monitoring, Explainability & Safety

Confidence dashboards

Keyword attribution

Tag schema validation

“Needs manual review” workflow

▶️ How to Run

Open email_tagger.ipynb in Google Colab or Jupyter

Install dependencies:

pip install pandas scikit-learn


Run all cells

Use the final prediction function to test new emails

📄 License
MIT License

Copyright (c) 2025 B. Pavan Kumar
