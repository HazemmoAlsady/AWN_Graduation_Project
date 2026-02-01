# 🧠 Awn – AI Data Processing & Assistance Classification

## 📌 Overview
This folder contains the **AI & Data Processing layer** of the **Awn Smart Humanitarian Platform**.  
Its main responsibility is to clean, preprocess, and analyze beneficiary data in order to:
- Classify the required assistance type
- Predict the level of need
- Prepare trained AI models for backend integration

> ⚠️ This layer does **not** include any UI or frontend logic.  
> It is designed to integrate with the Backend (ASP.NET Core API).

---

## 📂 Files Description

### 1️⃣ Cleaning_data.ipynb
#### 🎯 Purpose
Cleans raw beneficiary data and removes inconsistencies before any analysis.

#### 🛠️ What it does
- Removes null and missing values
- Fixes inconsistent or invalid records
- Standardizes column names and formats
- Handles obvious data errors

#### 📥 Input
- Raw dataset (CSV / Excel / Database export)

#### 📤 Output
- Cleaned dataset ready for preprocessing

#### 🔗 Backend Usage
- **This notebook must run first**
- Produces the base dataset for all next steps

---

### 2️⃣ Preprocessing_&_need_model.ipynb
#### 🎯 Purpose
Performs **data preprocessing and model training** for AI-based decision making.

This notebook prepares the data **before rule-based logic** and also trains ML models when needed.

#### 🧠 What happens inside
- Feature engineering (ratios, derived fields)
- Encoding categorical variables
- Handling missing values
- Scaling numerical features
- Training and evaluating ML models:
  - Need Level Prediction (Low / Medium / High)
  - Assistance Type Classification (Text + Structured Data)
- Saving trained models as `.pkl` files for production use

#### 📥 Input
- Cleaned dataset from `Cleaning_data.ipynb`

#### 📤 Output
- Trained ML pipelines
- Saved model files:
  - `need_level_rf_pipeline.pkl`
  - `need_level_target_encoder.pkl`
  - `assistance_type_rf_pipeline.pkl`
  - `assistance_type_target_encoder.pkl`

#### 🔗 Backend Usage
- Backend loads the saved `.pkl` models
- Backend sends user input data
- Model returns predictions directly (no preprocessing in backend)

---

### 3️⃣ Rule_Based_Assistance_Type_Classification.ipynb
#### 🎯 Purpose
Applies **deterministic rule-based logic** to classify assistance types without machine learning.

#### 🧠 Logic Used
- IF / ELSE conditions
- Rules based on:
  - Income level
  - Family size
  - Health status
  - Social status

#### 📌 Examples
- Low income + large family → `Financial Assistance`
- Chronic disease present → `Medical Assistance`
- School-aged children + low income → `Educational Assistance`

#### 📥 Input
- Preprocessed dataset

#### 📤 Output
- Column: `assistance_type`
- Possible values:
  - Financial
  - Medical
  - Food
  - Educational
  - Mixed

#### 🔗 Backend Usage
- Used when rule-based logic is sufficient
- Acts as:
  - A fast MVP solution
  - A fallback or reference for AI decisions

---

## 🔄 Suggested Processing Flow
```text
Raw Data
   ↓
Cleaning_data.ipynb
   ↓
Preprocessing_&_need_model.ipynb
   ↓
Rule_Based_Assistance_Type_Classification.ipynb
   ↓
ASP.NET Core Backend API
