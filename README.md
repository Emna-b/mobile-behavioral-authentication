## Mobile Behavioral Authentication – User Identification from Sensor Data

Overview

This project aims to identify users based on mobile sensor behavior. Each user generates multiple sessions composed of raw motion sensor events. The task is to predict the correct user_id for unseen sessions using only behavioral signals.

The solution follows a time-aware, leakage-safe pipeline that aggregates noisy event-level data into stable session-level representations.

Key Principles

Event → Window → Session aggregation

Time-aware feature extraction

Sensor-specific modeling

Session-level train/validation split (no leakage)

Cross-validation for robustness

Data & EDA

Each row is a single sensor event

Sessions vary in length and sensor availability

Individual events are noisy and not user-discriminative

Key insight: User identity emerges only after temporal aggregation.

Methodology
Preprocessing

Converted timestamps and sorted events chronologically

Mapped sensor IDs to semantic names

Handled missing values consistently

Train / Validation Split

Performed session-level stratified splitting

Ensured no (session_id, user_id) overlap between sets

Feature Engineering

Pivoted sensor streams into time-aligned format

Applied fixed 2-second windows

Extracted statistical features (mean, std, min, max, skew, kurtosis, energy)

Computed vector norms for 3-axis sensors

Aggregated window features to session-level (mean + std)

Modeling

Used a Random Forest classifier

Chosen for:

Non-linear modeling capacity

Robustness to noise and correlated features

Minimal preprocessing requirements

Evaluation

Metrics:

Accuracy

Macro F1

Top-3 accuracy

Top-5 accuracy

Evaluation strategy:

Single session-level validation split

5-fold stratified cross-validation for stability

Result: Consistent performance across folds, indicating good generalization and no leakage.

Feature Importance

Computed cross-validated feature importance

Aggregated importance by sensor type

Feature selection improved Top-K accuracy with minor Top-1 trade-offs

Baseline model retained for final submission

Test Inference

Applied identical pipeline to test data

Aligned features with training set

Generated session-level user predictions

Key Findings

Motion behavior contains stable, user-specific signatures

Temporal aggregation is essential for identification

Accelerometer, magnetometer, and gyroscope are most informative

Random Forests perform strongly without heavy tuning
