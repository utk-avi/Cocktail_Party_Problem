# 🍸 Cocktail Party Problem — ICA-Based Speech Separation

This project demonstrates the **Cocktail Party Problem** — separating multiple mixed audio signals into their individual source signals using **Independent Component Analysis (ICA)** implemented from scratch in Python.

Given a set of mixed recordings (e.g., multiple people speaking simultaneously), ICA attempts to recover the original independent sources without knowing the mixing process.

---

## 📌 Features

* Uses **Laplace prior** via the `sign` nonlinearity to update the unmixing matrix.
* Implements **stochastic gradient ascent** ICA without external ML libraries.
* Normalizes audio and saves both mixed and separated signals as `.wav` files.
* Simple and readable Python implementation.
* Demonstrates the classical blind source separation setup.

---

## 📁 Project Structure

```
project/
│── mix.dat                # Input mixed audio signals (M samples × N channels)
│── code.py                # Main program (your code)
│── output/                # Folder where results are saved
│     ├── mixed_*.wav      # Audio files for each mixture channel
│     ├── split_*.wav      # Recovered separated sources
│     └── W.txt            # Learned unmixing matrix
│── README.md              # This file
```

---

## 🎧 How It Works

### 1. **Load & Normalize Audio**

The file `mix.dat` contains mixed signals.
They are normalized to the range `[-1, 1]` to prevent clipping.

### 2. **Estimate Unmixing Matrix W**

ICA is applied using the update rule:

[
W \leftarrow W + \eta\left( (W^T)^{-1} - \text{sign}(Wx)x^T \right)
]

* Uses a schedule of decreasing learning rates (`anneal` list).
* Updates one sample at a time (stochastic gradient ascent).
* Produces an unmixing matrix `W`.

### 3. **Compute Separated Signals**

Recovered sources are:

[
S = X W^T
]

These are normalized and saved as audio files.

---

## ▶️ Running the Project

### 1. Install dependencies

```bash
pip install numpy scipy
```

### 2. Ensure your `mix.dat` file is present

This file should be shaped:

```
# rows = time samples
# columns = mixed audio channels
```

### 3. Run the program

```bash
python main.py
```

### 4. Check the `output/` folder

You will find:

* `mixed_0.wav`, `mixed_1.wav`, …
* `split_0.wav`, `split_1.wav`, …
* `W.txt` — the learned unmixing matrix

---

## 📊 Algorithm Details

This is a **basic ICA implementation** using:

* **Laplace density** → score function `sign(y)`
* **Stochastic gradient updates**
* **Annealing learning rates**
* **No whitening stage** (the update rule compensates for it)

This demonstrates core ICA behavior without relying on high-level libraries such as scikit-learn.

---

## 🎤 Example Use Case: The Cocktail Party Problem

Imagine placing multiple microphones in a noisy room where several people are speaking simultaneously. Each microphone captures a mixture of all voices.

ICA attempts to **recover the original voices individually**.

This project showcases exactly that.

---

## 🧠 References

* A. Hyvärinen & E. Oja — *Independent Component Analysis: Algorithms and Applications*
* The classic “cocktail party problem”

---
