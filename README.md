# Fluxonium Offset Inverse Model

Machine learning pipeline for extracting **flux offset** in fluxonium qubits from **resonator spectroscopy vs flux data**.

---

## Overview

This project builds an end-to-end workflow:

1. Train a neural network on simulated fluxonium spectra
2. Process experimental resonator spectroscopy data
3. Automatically extract one flux period
4. Infer flux offset using a trained 1D CNN
5. Convert offset into physical zero / half flux points

---

## Environment & Dependencies

### Python packages

pip install -r requirements.txt

### Required external packages

* cqedtoolbox
* labcore

Install CQEDToolbox:

pip install -e path/to/CQEDToolbox
"https://github.com/toolsforexperiments/CQEDToolbox/tree/main/src/cqedtoolbox"

---

## Workflow

### 1. Train ML model (before experiment)

Run:

ML model/ML for resonator spectrum vs flux.ipynb

This step:

* generates synthetic dataset
* trains 1D CNN

---

### 2. Collect experimental data

Measure:

* resonator spectroscopy vs flux

---

### 3. Preprocess experimental data

Run:

ML model/Generating experimental data for ML offset learning.ipynb

This step:

* fits resonator frequency vs flux
* filters noisy points
* automatically selects one flux period
* resamples to fixed grid (121 points)

Output:

exp_res_spec_period_for_cnn_test.npz

---

### 4. Run ML prediction

pred = predict_single_curve(fr_resampled, model_path)

---

### 5. Extract offset (key result)

offset_fraction ∈ [0, 1]

Important:

* This is the ONLY reliable output
* Other parameters (EC, EL, EJ, g) are not uniquely determined

---

### 6. Convert to physical flux

Using offset:

* determine zero flux point
* determine half flux point

---

## Notes

* Input must be exactly one flux period
* Automatic period detection uses:

  * autocorrelation (period estimation)
  * sliding window scoring (best segment selection)
* Model is designed for offset extraction, not full parameter recovery

---

## Key Idea

We only solve:

flux offset → experimental calibration

---

## License

MIT License
