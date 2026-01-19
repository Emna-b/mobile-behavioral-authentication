## Mobile Behavioral Authentication – User Identification from Sensor Data

### Overview

This project aims to identify users based on mobile sensor behavior. Each user generates multiple sessions composed of raw motion sensor events. The task is to predict the correct user_id for unseen sessions using only behavioral signals.

The solution follows a time-aware, leakage-safe pipeline that aggregates noisy event-level data into stable session-level representations.
