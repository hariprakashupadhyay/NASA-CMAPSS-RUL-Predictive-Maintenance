# NASA C-MAPSS RUL Predictive Maintenance

A deep learning-based predictive maintenance system that estimates the Remaining Useful Life (RUL) of aircraft engines using the NASA C-MAPSS FD001 dataset.

## Project Overview

Predictive maintenance aims to estimate how much useful operating life remains before an equipment failure occurs.

This project uses historical engine sensor data to learn degradation patterns and predict the Remaining Useful Life of an aircraft engine.

The project implements an end-to-end machine learning workflow:

- Data loading and preprocessing
- Exploratory data analysis
- Sensor analysis and selection
- RUL calculation
- Feature engineering
- Sequence generation
- Machine learning model comparison
- LSTM-based time-series modeling
- RUL capping experiments
- Official test-set evaluation
- Prediction error analysis
- Reusable inference pipeline
- Model and preprocessing artifact saving

## Dataset

The project uses the NASA C-MAPSS FD001 dataset.

The dataset contains simulated aircraft engine degradation data recorded over multiple operating cycles.

The project uses:

- `train_FD001.txt`
- `test_FD001.txt`
- `RUL_FD001.txt`

The dataset itself is not included in this repository.

## Machine Learning Approach

Several models were evaluated during the project:

1. Linear Regression
2. Random Forest
3. XGBoost
4. XGBoost with time-series features
5. LSTM

The LSTM model was selected as the final approach because the problem is inherently sequential and engine degradation develops over time.

## Input Features

The final LSTM uses 15 selected sensor features:

```text
sensor_2
sensor_3
sensor_4
sensor_6
sensor_7
sensor_8
sensor_9
sensor_11
sensor_12
sensor_13
sensor_14
sensor_15
sensor_17
sensor_20
sensor_21
```

## Model Configuration

The final LSTM model uses a sequence length of 30 cycles.

### Architecture

- LSTM: 64 units
- Dropout: 0.2
- LSTM: 32 units
- Dropout: 0.2
- Dense: 16 units
- Output: 1 unit
- Optimizer: Adam
- Loss: Mean Squared Error
- Metric: Mean Absolute Error

## RUL Capping

Different RUL caps were evaluated during the project.

The 125-cycle RUL cap produced the best performance on the official FD001 test set.

## Final Results

The final 125-capped LSTM achieved:

| Metric | Result |
|---|---:|
| Test MAE | 12.93 cycles |
| Test RMSE | 17.16 cycles |
| Test R² | 0.8295 |

### Model Comparison

| Model | Test MAE | Test RMSE | Test R² |
|---|---:|---:|---:|
| Original LSTM | 17.10 | 23.74 | 0.6736 |
| 125-Capped LSTM | **12.93** | **17.16** | **0.8295** |
| 140-Capped LSTM | 13.18 | 17.64 | 0.8197 |

Compared with the original uncapped LSTM:

- MAE improved by approximately 24.4%
- RMSE improved by approximately 27.7%

## Error Analysis

The final model was evaluated across different RUL ranges:

- 0–30 cycles
- 31–60 cycles
- 61–90 cycles
- 91–120 cycles
- 121–150 cycles

This analysis helps identify where the model performs well and where prediction errors remain higher.

## Inference Pipeline

The project includes a reusable inference pipeline that:

1. Accepts raw engine sensor data
2. Sorts the data by cycle
3. Checks that at least 30 cycles are available
4. Selects the required sensor features
5. Applies the saved scaler
6. Extracts the latest 30-cycle sequence
7. Generates the predicted RUL
8. Applies the RUL limits
9. Returns the predicted Remaining Useful Life

## Project Structure

```text
NASA-CMAPSS-RUL-Predictive-Maintenance/
│
├── README.md
├── NASA_CMAPSS_RUL_Predictive_Maintenance.ipynb
├── requirements.txt
├── .gitignore
│
├── NASA-CMAPSS-RUL-Predictive-Maintenance/
│
├── README.md
├── NASA_CMAPSS_RUL_Predictive_Maintenance.ipynb
├── requirements.txt
├── .gitignore
│
├── models/
│   ├── .gitkeep
│   └── final_rul_lstm_125.keras
│
├── artifacts/
│   ├── .gitkeep
│   ├── lstm_scaler.pkl
│   ├── preprocessing_info.json
│   └── project_config.json
│
└── results/
    ├── .gitkeep
    ├── final_rul_predictions.csv
    └── final_model_performance.csv
```
