# Mobile Behavioral Authentication – User Identification from Sensor Data
Overview

This project focuses on identifying users based on mobile sensor data by modeling behavioral movement patterns. Each user generates multiple sessions composed of raw sensor events (accelerometer, gyroscope, magnetometer, etc.). The objective is to predict the correct user_id for unseen sessions using motion behavior alone.

The solution follows a time-aware, leakage-safe pipeline that aggregates raw sensor events into robust session-level representations before classification.

Key Principles

Event → Window → Session aggregation

Time-aware processing

Sensor-specific feature extraction

Session-level train/validation split (no leakage)

Cross-validation for robustness (not model selection)

Data Description

Each row represents a sensor event

Multiple sensor streams per session

Sessions belong to a single user

Strong temporal structure and variable-length sessions

Methodology
1. Exploratory Data Analysis (EDA)

Analyzed number of events per session and sessions per user

Inspected raw sensor signals to understand noise and variability

Confirmed class balance was sufficient for stratified evaluation

Key insight:
Individual sensor events are noisy; user identity emerges only after temporal aggregation.

2. Preprocessing

Converted timestamps to datetime

Sorted events chronologically per session

Mapped sensor IDs to semantic names

Handled missing values consistently

3. Train / Validation Split

Performed session-level stratified splitting

Ensured no (session_id, user_id) pair appears in both sets

Prevented temporal and identity leakage

4. Feature Engineering

Pivoted sensor streams into time-aligned wide format

Applied fixed 2-second temporal windows

Extracted statistical features per window:

Mean, std, min, max

Skewness, kurtosis

Signal energy

Computed vector norms for 3-axis sensors

Aggregated window features to session-level (mean + std)

5. Modeling

Used a Random Forest classifier:

Handles non-linear interactions

Robust to noise and correlated features

Requires minimal feature scaling

Trained on session-level features only

6. Evaluation

Metrics used:

Accuracy

Macro F1-score

Top-3 accuracy

Top-5 accuracy

Evaluation strategies:

Single train/validation split

5-fold stratified cross-validation (robustness check)

Result:
Consistent performance across folds, indicating strong generalization and no overfitting.

7. Feature Importance & Iteration

Computed feature importance across CV folds

Aggregated importance at sensor level

Performed feature selection using cumulative importance

Observed:

Slight improvement in Top-K accuracy

Minor trade-off in Top-1 accuracy

The baseline model was retained for final submission due to its better balance between precision and stability.

8. Test Inference

Applied the exact same pipeline to test data

Aligned test features with training features

Generated session-level user predictions

Exported results in submission format

Key Findings

Behavioral patterns are user-specific and consistent

Temporal aggregation is essential for identity recognition

Motion sensors (accelerometer, magnetometer, gyroscope) contribute most

Random Forests perform well for this task without heavy tuning

Business Applications

Passive user authentication

Continuous identity verification

Fraud detection and anomaly monitoring

Behavioral biometrics without explicit user input

Reproducibility

Deterministic splits (random_state=42)

Clear separation between train, validation, and test

Feature alignment safeguards prevent inference-time errors

Files

notebook.ipynb – full end-to-end pipeline

submission.csv – final predictions

README.md – project summary
