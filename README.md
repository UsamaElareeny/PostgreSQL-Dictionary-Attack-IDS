# PostgreSQL Dictionary Attack Detection using Flow-Based Machine Learning

## 📌 Overview

This project focuses on detecting **PostgreSQL dictionary attacks** using **flow-based network traffic analysis** and **supervised machine learning** techniques. Instead of relying on payload inspection, the approach leverages **statistical flow features** extracted from network traffic to identify malicious authentication behavior.

The system is evaluated in a controlled virtual environment using **Kali Linux** as the attacker and **Metasploitable2** as the victim machine.

---

## 🧪 Experimental Setup

- **Attacker:** Kali Linux
- **Victim:** Metasploitable2 (PostgreSQL service on port 5432)
- **Attack Type:** Dictionary-based login attack
- **Attack Tool:** Metasploit (`auxiliary/scanner/postgres/postgres_login`)
- **Traffic Capture:** tcpdump, Wireshark
- **Flow Extraction:** CICFlowMeter

---

## 🔄 Workflow

The complete pipeline followed in this project is:

1. Generate PostgreSQL dictionary attack traffic using Metasploit
2. Capture network traffic in PCAP format
3. Convert PCAP files to flow-level features using CICFlowMeter
4. Analyze TCP streams in Wireshark to identify successful logins
5. Label flows as:
   - `1` → Successful dictionary attack
   - `0` → Benign / normal traffic
6. Preprocess and clean the dataset
7. Perform feature selection
8. Train and evaluate machine learning models
9. Save trained models and artifacts
10. Predict attacks on unseen flow data

A visual representation of this workflow is included in the report.

---

## 📁 Repository Structure

├── results/
│ ├── model.joblib # Trained Random Forest model
│ ├── scaler.joblib # Feature scaler
│
├── test/
│ └── test.csv # New flow data for prediction
│
├── main.ipynb # Main Jupyter notebook (end-to-end pipeline)
├── feature_importance.csv # Feature importance scores
├── Data_clean.csv # Cleaned and scaled dataset
├── Data_selected.csv # Dataset with selected features
├── Data.csv # Original flow dataset
├── project workflow.png # Workflow diagram used in report
├── README.md # Project documentation
└── requirements.txt # Python dependencies

## 🧹 Data Preprocessing

The following preprocessing steps are applied:

- Removal of non-generalizable identifiers:
  - IP addresses
  - Port numbers
  - Timestamps
- Handling of missing and infinite values
- Feature scaling using `StandardScaler`
- Removal of highly correlated features
- Exclusion of protocol-specific identifiers

---

## 🎯 Feature Selection

Feature selection is performed in two stages:

1. **Correlation filtering** to remove redundant features
2. **Random Forest feature importance** to select the most informative behavioral features

Only the top-ranked features are used for model training and prediction.

---

## 🤖 Machine Learning Models

The following supervised learning models are evaluated:

- **Logistic Regression**
  - Baseline model
  - Interpretable and efficient
- **Random Forest**
  - Captures non-linear relationships
  - Robust to noise and feature interactions

Class imbalance is handled using balanced class weights.

---

## 📊 Evaluation Metrics

Model performance is evaluated using:

- **Recall** (primary metric)
- Precision
- F1-score
- Balanced accuracy

Recall is emphasized to minimize false negatives, as undetected attacks pose a higher security risk.

---

## 🚀 Prediction on New Data

To classify new network traffic:

1. Convert PCAP to CSV using CICFlowMeter
2. Apply the same preprocessing steps
3. Align features with the trained model
4. Scale features using the saved scaler
5. Predict attack or benign behavior using the trained model

The output is a binary classification for each flow.

---

## ⚙️ Requirements

Create a virtual environment and install dependencies:

```bash
pip install -r requirements.txt
```
