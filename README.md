# Thermal Image-Based Electrical Tracking Severity Classification

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![XGBoost](https://img.shields.io/badge/Model-XGBoost-orange)
![Dataset](https://img.shields.io/badge/Dataset-1000%20Thermal%20Images-red)
![Standard](https://img.shields.io/badge/Standard-IEC60587-yellow)

A feature engineering and machine learning framework to classify electrical
tracking severity in thermal images into 4 levels using a novel Degradation
Severity Index (DSI).

---

## Overview

Electrical tracking is a progressive surface degradation phenomenon in
insulating materials. This project classifies the severity of tracking into
4 stages — **Surface Discharge, Dry Band formation, Localised Bright Spot, and Track Path Formation** — using thermal images
captured during IEC60587 standard tracking tests.

The core idea is to extract physically meaningful features from thermal
images, rank them using XGBoost gain, formulate a single Degradation
Severity Index (DSI), and use it to classify severity levels.

---

## Dataset

| Property          | Details                               |
|-------------------|---------------------------------------|
| Total Images      | 1000 thermal images                   |
| Classes           | 4 (Low, Medium, High, Very High)      |
| Images per class  | 250                                   |
| Source            | FLIR thermal camera                   |
| Standard          | IEC60587 Tracking Test                |
| Classification    | Based on temperature range per stage  |

---

## Methodology

### Step 1 — Feature Extraction

9 features are extracted from each thermal image and stored in a CSV file:

| Feature              | Category  |
|----------------------|-----------|
| Mean Temperature     | Physical  |
| Maximum Temperature  | Physical  |
| Area                 | Physical  |
| Solidity             | Shape     |
| Eccentricity         | Shape     |
| Width at Half Max    | Shape     |
| Entropy              | Texture   |
| Variance             | Texture   |
| Thermal Gradient     | Texture   |

The CSV contains 1000 rows (one per image) and 10 columns
(9 features + severity stage label).

---

### Step 2 — Feature Importance Using XGBoost Gain

XGBoost is used to compute the gain of each feature — a measure of how
much each feature contributes to making correct classification decisions.
The gain values are then normalized to sum to 1.

---

### Step 3 — DSI Formulation

A Degradation Severity Index (DSI) is computed for each image as a
weighted sum of its normalized features:

  DSI = Σ (normalized_gain_i × feature_value_i)

This converts 9 features into a single interpretable severity score.

---

### Step 4 — Severity Classification

An XGBoost classifier maps DSI values to one of 4 severity levels:

  Low → Medium → High → Very High

---

### Step 5 — Hotspot Analysis & Filter Study

- Hotspot regions are identified in thermal images from each stage
- Temperature distribution histograms are plotted around hotspots
- Median and bilateral filters are applied to the images
- Changes in statistical features and image quality metrics are
  analyzed and compared across the two filters

---

## Results

- Feature importance ranking via XGBoost gain
- Normalized gain weights for DSI formulation
- DSI value distribution across 4 severity stages
- Classification accuracy using DSI + XGBoost
- Temperature distribution before and after filtering
- Statistical feature variation across median and bilateral filters
- Image quality metric comparison between filters

---

## Tech Stack

- **Language:** Python
- **ML Model:** XGBoost
- **Image Processing:** OpenCV
- **Data Handling:** Pandas, NumPy
- **Visualization:** Matplotlib, Seaborn

---

## Project Structure

```
├── dataset/
│   ├── stage_1/               # Low severity (250 images)
│   ├── stage_2/               # Medium severity (250 images)
│   ├── stage_3/               # High severity (250 images)
│   └── stage_4/               # Very high severity (250 images)
├── features/
│   └── features.csv           # Extracted features with stage labels
├── src/
│   ├── feature_extraction.py  # Extract 9 features from images
│   ├── dsi_formulation.py     # Compute XGBoost gain and DSI
│   ├── classification.py      # XGBoost severity classification
│   └── hotspot_analysis.py   # Hotspot detection and filter study
└── README.md
```

---


