# 🧠 Awn – AI Data Processing & Assistance Classification

## 📌 Overview
This folder contains the **AI & Data Processing layer** of the **Awn Smart Humanitarian Platform**.  
Its main responsibility is to prepare beneficiary data, classify assistance types, and decide whether AI models are required or rule-based logic is sufficient.

> ⚠️ This layer does **not** include any UI or frontend logic.  
> It is designed to integrate with the Backend (ASP.NET Core API).

---

## 📂 Files Description

### 1️⃣ Cleaning_data.ipynb
#### 🎯 Purpose
Cleans and prepares raw beneficiary data for further processing and decision-making.

#### 🛠️ What it does
- Removes null and missing values
- Standardizes column names and data formats
- Fixes inconsistent or invalid values
- Normalizes key attributes (income, family size, governorate, etc.)

#### 📥 Input
- Raw dataset (CSV / Excel / Database export)

#### 📤 Output
- Cleaned and normalized dataset (`cleaned_df`)
- Ready for rule-based or AI-based processing

#### 🔗 Backend Usage
- **This notebook must run first**
- The backend consumes the cleaned data for:
  - Assistance type classification
  - Need level evaluation

---

### 2️⃣ Rule_Based_Assistance_Type_Classification.ipynb
#### 🎯 Purpose
Determines the **type of assistance required** using deterministic rule-based logic instead of machine learning.

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
- Cleaned dataset from `Cleaning_data.ipynb`

#### 📤 Output
- New column: `assistance_type`
- Possible values:
  - Financial
  - Medical
  - Food
  - Educational
  - Mixed

#### 🔗 Backend Usage
- Used as a fast and transparent alternative to ML models
- Suitable for MVP phase
- Rules can be:
  - Reimplemented directly in backend business logic
  - Or kept as a reference for AI decisions

---

### 3️⃣ Preprocessing_&_need_model.ipynb
#### 🎯 Purpose
Prepares data for predicting the **Need Level** of each case and evaluates whether an AI model is required.

#### 🧠 What happens inside
- Feature engineering
- Encoding categorical variables
- Scaling numerical features
- Analysis to decide:
  - Rule-based logic is sufficient
  - OR an ML model is needed for need scoring

#### 📥 Input
- Cleaned data
- Output from rule-based assistance classification

#### 📤 Output
- Preprocessed dataset ready for training
- Clear decision regarding:
  - ❌ No model needed
  - ✅ Need Score ML model required

#### 🔗 Backend Usage
- If an ML model is trained:
  - Backend calls it via REST API
- If rule-based logic is sufficient:
  - Backend relies only on predefined rules

---

## 🔄 Suggested Processing Flow
```text
Raw Data
   ↓
Cleaning_data.ipynb
   ↓
Rule_Based_Assistance_Type_Classification.ipynb
   ↓
Preprocessing_&_need_model.ipynb
   ↓
ASP.NET Core Backend API
