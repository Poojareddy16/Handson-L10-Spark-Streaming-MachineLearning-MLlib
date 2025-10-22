# Handson-L10-Spark-Streaming-MachineLearning-MLlib

## 📘 Overview
This project implements a **real-time analytics pipeline** for a ride-sharing platform using **Apache Spark Structured Streaming** and **Spark MLlib**.  
It processes live trip data, performs machine-learning-based fare prediction, and analyzes fare trends over time.

The system consists of three main components:
1. **Data Generator (`data_generator.py`)** — continuously streams simulated ride data over a socket connection.  
2. **Task 4 (`task4.py`)** — trains and applies a regression model to predict real-time fares.  
3. **Task 5 (`task5.py`)** — performs time-windowed fare-trend prediction with feature engineering.

---

## ⚙️ Repository Structure

```bash
Handson-L10-Spark-Streaming-MLlib/
├── data_generator.py  
├── task4.py  
├── task5.py  
├── training-dataset.csv  
├── models/  
│   ├── fare_model/  
│   └── fare_trend_model_v2/  
├── images/  
│   ├── task4_output.png  
│   └── task5_output.png  
└── README.md  
```
---

## 🚀 Setup Instructions

### 1️⃣  Environment
Run everything inside **GitHub Codespaces** (recommended) or any environment with Python ≥ 3.8.

```bash
pip install pyspark faker numpy pandas
```

### 2️⃣ Start the Data Generator
```bash
python data_generator.py --host localhost --port 9999
```
This script uses Faker to produce random trip records:
```bash
{"trip_id":"...","driver_id":12,"distance_km":8.45,"fare_amount":32.6,"timestamp":"2025-10-22 00:10:00"}
```
Keep this terminal open — it streams data continuously to Spark.

---
## 🧩 Task 4 — Real-Time Fare Prediction

### 🎯 Objective
Train a Linear Regression model that learns the relationship between distance_km (feature) and fare_amount (label),
then apply it to incoming streaming data to predict fares in real time and compute deviation.

---
### 🧠 Approach

1. Load training-dataset.csv
2. Assemble features using VectorAssembler (distance_km)
3. Train LinearRegression model and save → models/fare_model
4. Load the saved model during streaming
5. Read streaming data from socket (localhost:9999)
6. Predict fare_amount and compute deviation = |actual − predicted|
7. Output results to console (outputMode("append"))

---
### ▶️ Run
```bash
python task4.py --host localhost --port 9999
```
---

### 🖼️ Sample Output
```bash
| distance_km | fare_amount | predicted_fare | deviation |
|--------------|-------------|----------------|------------|
| 4.32         | 18.10       | 17.85          | 0.25       |
| 7.55         | 33.60       | 34.02          | 0.42       |

```

<img width="683" height="449" alt="task4" src="https://github.com/user-attachments/assets/29a57e91-2bf3-4903-bb97-3722602ad0ef" />

---


## 🧮 Task 5 — Time-Based Fare Trend Prediction

### 🎯 Objective

Use 5-minute time windows to analyze average fare trends and predict the next window’s average fare.

---
## 🧠 Approach
1. Group training-dataset.csv into 5-minute windows and compute avg_fare
2. Create cyclic time features: hour_of_day and minute_of_hour
3. Train a LinearRegression model → models/fare_trend_model_v2
4. For streaming data:
   - Apply identical 5-minute windowing and feature creation
   - Use the trained model to predict next_avg_fare
5. Print results showing window start/end, actual avg_fare, and predicted next_avg_fare

---

## ▶️ Run
```bash
python task5.py --host localhost --port 9999 --window 5
```
---

### 🖼️ Sample Output
```bash
| window_start        | window_end          | avg_fare | predicted_next |
|----------------------|--------------------|-----------|----------------|
| 2025-10-22 00:00:00 | 2025-10-22 00:05:00 | 45.7      | 46.2           |
| 2025-10-22 00:05:00 | 2025-10-22 00:10:00 | 46.1      | 46.0           |
```

<img width="610" height="452" alt="task5(1)" src="https://github.com/user-attachments/assets/689c609b-73c7-4cde-ac1a-07f43fc9a9c4" />

---

<img width="605" height="443" alt="task5(2)" src="https://github.com/user-attachments/assets/ebd0b8e7-b8b4-4eed-a813-738413ae5fb8" />


---

## 📊 Results & Observations
---
Task 4: Model successfully predicted fares in real time with small deviations (< 1 unit avg).
Task 5: Model accurately followed fare-trend patterns using time-window aggregation.
Demonstrated Spark Structured Streaming’s integration with MLlib for continuous ML inference.

--------------------------------

### 🧾 Submission Checklist

✅ .py scripts for Task 4 and Task 5  
✅ Generated output screenshots included in README  
✅ Proper folder structure and model paths  
✅ Final repository pushed to GitHub  


