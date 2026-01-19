## Mobile Behavioral Authentication – User Identification from Sensor Data

### Overview

This project aims to identify users based on mobile sensor behavior. Each user generates multiple sessions composed of raw motion sensor events. The task is to predict the correct user_id for unseen sessions using only behavioral signals.

The solution follows a time-aware, leakage-safe pipeline that aggregates noisy event-level data into stable session-level representations.

### Core Design Principles

-Event-level → window-level → session-level aggregation
-Time-aware processing of sequential data.
-Sensor-specific feature extraction.
-Session-level train/validation split (no data leakage).
-Cross-validation for robustness and stability.

### Methodology
####Preprocessing:
-Converted timestamps and sorted events chronologically.
-Mapped sensor identifiers to semantic sensor names.
-Standardized missing value handling across sensors.

####Train / Validation Split:
-Performed session-level stratified splitting.
-Ensured no (session_id, user_id) overlap between sets.

####Feature Engineering:
-Pivoted sensor streams into a time-aligned wide format
-Applied fixed 2-second temporal windows
-Extracted statistical descriptors (mean, std, min, max, skewness, kurtosis, energy)
-Computed vector norms for 3-axis motion sensors
-Aggregated window features to session-level statistics (mean + std)

####Modeling Approach
A Random Forest classifier was used due to its ability to:
-Capture non-linear behavioral patterns
-Handle correlated and heterogeneous features
-Remain robust to noise and missing values
-Perform well without heavy feature scaling or tuning
