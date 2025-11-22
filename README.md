
# Project Title

A brief description of what this project does and who it's for

Multi-Tenant Email Tagging System
NLP · Text Classification · Customer-Isolated Models

Author: B. Pavan Kumar
License: MIT

Table of Contents

Project Overview

Key Features

System Architecture

Implementation Details

Data Preparation

Pattern Rules

ML Pipeline

Guardrails

Customer Isolation Strategy

Error Analysis

Production-Ready Enhancements

How to Run

License

Project Overview

This project implements a tagging system for customer-support emails in a multi-tenant environment. Each customer has their own tag set and classification logic. The goals are:

Prevent any tag-leakage across customers

Achieve reliable tagging even with very small datasets

Use an interpretable hybrid system combining rules + ML

Provide realistic guardrails and design for production

Key Features

Pattern-based tagging engine: Keyword → tag map

ML fallback pipeline: TF-IDF + Multinomial Naive Bayes, per customer

Guardrails: custom stopwords + confidence threshold → needs_manual_review

Customer isolation: separate models and tag sets per customer

Explainability & safety: clear rule logic, predictable behavior

System Architecture
Incoming Email (subject + body)
            │
            ▼
Customer Isolation Layer → selects customer’s model & tag set
            │
            ▼
Pattern Rule Engine → if match → tag assigned
            │
            ▼
ML Fallback → if no rule match → model prediction + confidence
            │
            ▼
Guardrail Layer → if confidence < threshold → needs_manual_review
            │
            ▼
Final Tag (or manual review trigger)

Implementation Details
Data Preparation

Combined subject + body into single text field

Lowercased text, removed generic words via custom stopwords

Dataset example: 12 emails across 4 customers (CUST_A, CUST_B, CUST_C, CUST_D)

Pattern Rules

Strong keyword-to-tag mapping for each customer tag set. Example:

“invoice”, “charged” → billing

“mail merge”, “CSV” → mail_merge_issue

“CSAT”, “dashboard” → analytics_issue
Rules are applied before ML.

ML Pipeline

Vectorizer: TfidfVectorizer(stop_words=custom_stopwords)

Classifier: MultinomialNB()

Trained one model per customer_id → ensures isolation

Guardrails

Stopwords include: issue, problem, help, error, support, email

If ML model confidence < 0.5 → output needs_manual_review instead of forced tagging

Customer Isolation Strategy

Each customer_id gets its own classifier, trained only on that customer’s data

The pattern rule engine uses only that customer’s allowed tags

Prediction logic checks the customer_id, selects correct pipeline

Ensures no tags from one customer ever appear in another’s predictions

Error Analysis
ML-Only Evaluation

Leave-one-out per customer (3 emails each)

Training on 2 examples, testing on 1 → extremely small dataset

Result: ~0% accuracy — highlights ML alone is unreliable in this scenario

Hybrid System Results

With pattern rules + ML fallback + guardrails:

12 / 12 emails correctly tagged

All predictions via rule-based logic

No tag leakage, no misclassification

Demonstrates strong performance for small-data, multi-tenant use-case

Production-Ready Enhancements

Transformer / LLM Multi-Tenant Classifier

Shared backbone + customer-specific heads

Better generalization with limited data

Human Feedback Loop & Online Learning

Agent corrections feed back into training

Automatic rule discovery & retraining

Drift detection per customer

Monitoring, Explainability & Guardrails

Confidence dashboards

Tag schema validation

Keyword attribution and explanation

Manual review workflow

How to Run

Open email_tagger.ipynb in Google Colab or Jupyter Notebook

Install dependencies:

pip install pandas scikit-learn


Run all cells in order → dataset load → models train → pattern + ML logic → debug predictions

Modify test-cells to try your own email examples

License

MIT License (c) 2025 B. Pavan Kumar
Full license text available in LICENSE or at [link to license].