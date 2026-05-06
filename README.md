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
"[https://github.com/toolsforexperiments/CQEDToolbox/tree/main/src/cqedtoolbox](https://github.com/Jianyaogu/CQEDToolbox/blob/Power-rabi-method-changing/src/cqedtoolbox/protocols/operations/fluxonium)"

---

## Workflow

### 1. Train ML model (before experiment/during cooling process)

Run:

ML for resonator spectrum vs flux.ipynb till the generating theoretical data and training model step

This step:

* generates synthetic dataset base on guessed params
* trains 1D CNN
* save the model to the path you need

Output:

fluxonium_inverse_cnn_better.pt

---

### 2. Collect experimental data

Measure:

* resonator spectroscopy vs flux

---

### 3. Preprocess experimental data

Run:

Generating experimental data for ML offset learning.ipynb (except the last cell)

This step:

* fits resonator frequency vs flux
* filters noisy points
* automatically (or by hand) selects one flux period
* resamples to fixed grid (121 points or same as the points in a flux period used for training model)

Output:

exp_res_spec_period_for_cnn_test.npz

---

### 4. Run ML prediction

Continue running the ML for resonator spectrum vs flux.ipynb to get fitted data from the model you saved

---

### 5. Extract offset (key result)

offset_fraction ∈ [0, 1]

Important:

* This is the ONLY reliable output
* Other parameters (EC, EL, EJ, g) are not uniquely determined, so might not be correct

---

### 6. Convert to physical flux

Using offset run the last cell of Generating experimental data for ML offset learning.ipynb

* determine zero flux point
* determine half flux point

---

## Notes

* Input must be roughly ~1 flux period (for auto period picking, it would be best to have >2 flux periods)
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
MIT. See LICENSE for details.
