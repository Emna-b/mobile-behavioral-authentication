## Mobile Behavioral Authentication – User Identification from Sensor Data

### Overview

This project aims to identify users based on mobile sensor behavior. Each user generates multiple sessions composed of raw motion sensor events. The task is to predict the correct user_id for unseen sessions using only behavioral signals.

The solution follows a time-aware, leakage-safe pipeline that aggregates noisy event-level data into stable session-level representations.

### Core Design Principles

Event-level → window-level → session-level aggregation

Time-aware processing of sequential data.

Sensor-specific feature extraction.

Session-level train/validation split (no data leakage).

Cross-validation for robustness and stability.

### Methodology Overview

#### Data Understanding & Key EDA Insights
Each row corresponds to a single sensor event.

Sessions vary significantly in length and sensor coverage.

Individual sensor events are highly noisy and not user-discriminative.

##### Key insight:
User identity cannot be reliably inferred from individual events. Temporal aggregation is required to suppress noise and reveal stable, user-specific behavioral patterns.

#### Preprocessing:
Converted timestamps and sorted events chronologically.

Mapped sensor identifiers to semantic sensor names.

Standardized missing value handling across sensors.

#### Train / Validation Split:
Performed session-level stratified splitting.

Ensured no (session_id, user_id) overlap between sets.

#### Feature Engineering:
Pivoted sensor streams into a time-aligned wide format.

Applied fixed 2-second temporal windows.

Extracted statistical descriptors (mean, std, min, max, skewness, kurtosis, energy)

Computed vector norms for 3-axis motion sensors.

Aggregated window features to session-level statistics (mean + std)

#### Modeling Approach
A Random Forest classifier was used due to its ability to:

Capture non-linear behavioral patterns.

Handle correlated and heterogeneous features.

Remain robust to noise and missing values.

Perform well without heavy feature scaling or tuning.

#### Evaluation Strategy
Performance was assessed using:

Accuracy (Top-1 identification).

Macro F1 (balanced multi-class performance).

Top-3 and Top-5 accuracy (ranking quality).

Evaluation consisted of:
A session-level hold-out validation split.

5-fold stratified cross-validation to verify stability.

Results showed consistent performance across folds, indicating good generalization and absence of overfitting.

#### Feature Importance & Iteration
Feature importance was extracted across cross-validation folds.

Importance was aggregated at both feature and sensor levels.

Accelerometer, magnetometer, and gyroscope signals were most informative.

Feature selection slightly improved Top-K accuracy with minimal impact on Top-1 performance.

The baseline model was retained for final submission due to its stronger overall balance.

#### Test-Time Inference
The identical preprocessing and feature pipeline was applied to test data.

Feature spaces were aligned to the training set.

Final session-level user predictions were generated for submission.

#### Key Findings
Mobile motion data contains stable, user-specific behavioral signatures.

Temporal aggregation is essential for reliable identification.

Non-linear models effectively capture behavioral dynamics.

The approach generalizes well across sessions and users.


