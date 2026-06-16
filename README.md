# Version-1.0
# 🔄 Rephon Computing

### 🧠 Simulate the Physics of Rephasing Computation

![PyPI](https://img.shields.io/pypi/v/rephon)
![License](https://img.shields.io/badge/license-MIT-green)
![Python](https://img.shields.io/badge/python-3.9%2B-blue)

🚀 A product of **CETQAC — The Centre of Excellence for Technology, Quantum and AI (Canada)**
👨‍🔬 Founded by **Dr. Zuhair Ahmed** · 2026

---

# 📦 Installation

```bash
pip install rephon
```

✅ That's it.

Dependencies:

* 🔢 NumPy
* ⚙️ SciPy

---

# 🌟 What is Rephon Computing?

Rephon Computing is a computational paradigm whose physical substrate is the spontaneous rephasing of an inhomogeneously broadened ensemble of two-level systems, the phenomenon behind:

* 🔊 Phonon Echo
* 💡 Photon Echo
* 🧲 Spin Echo

The phonon echo was first experimentally measured at Bell Laboratories in 1976 and has been reproduced for nearly fifty years.

The **Rephon Computing Simulator (RCS)** is a classical state-vector emulator of a Rephon machine.

It implements all **15 Rephon Gates** directly from the optical Maxwell-Bloch equations.

✨ No pseudocode.
✨ No hand-waving.
✨ Physics-first computation.

With RCS you can:

* 🏗️ Build Rephon circuits
* ⚡ Execute native Rephon algorithms
* 🎯 Simulate realistic noise
* 🔬 Explore rephasing-based computing before hardware exists

---

# 🧠 The Core Idea

Imagine millions of oscillators that begin perfectly synchronized.

⏳ As time passes they gradually lose alignment and the signal disappears.

However, the phase of every oscillator still contains information about the original input.

Apply a conjugating pulse and something remarkable happens:

🔄 The ensemble spontaneously rephases.

📡 The original signal reconstructs itself.

The state of the machine is represented by:

```math
\psi(\Delta)
```

where:

* 🌈 Δ = frequency detuning
* 📊 N = W/δ spectral cells
* 🔹 Each spectral cell is called a **Rephon**
* 📦 W = inhomogeneous bandwidth
* 🎯 δ = single-emitter linewidth

This architecture naturally performs:

* ⚡ Fourier Transforms
* 🔍 Correlations
* 🔗 Convolutions
* ↩️ Time Reversals

all as native physical operations.

---

# 🚀 Why Use Rephon Computing?

### ⚡ Constant-Time Transforms

An N-point DFT or correlation occurs in a single Rephon cycle.

```text
Rephon: O(1) Physical Depth
Digital: O(N log N)
```

### 🎯 Analog Precision

Information lives in continuous phase space instead of fixed-width binary registers.

### 🛡️ Built-In Static Error Cancellation

Static manufacturing imperfections are naturally cancelled during rephasing.

Only dynamic noise remains.

### 🧪 Real Physical Substrate

Potential implementations already exist using:

* 💎 Rare-earth doped crystals
* 🧲 Spin ensembles
* 🔬 Coherent optical media

---

# ⚖️ Honest Scope

### 📘 Linear Regime

Native problems lie within classical P.

Advantages come from:

* ⚡ Throughput
* 🔋 Energy Efficiency
* 🛡️ Error Immunity

Not from exponential complexity-class speedups.

### 📙 Nonlinear Regime

Using the NL gate, the system becomes a universal approximator of temporal maps.

Its exact computational power remains an open research question.

🔬 This package allows exploration of both regimes.

---

# 🚀 Quick Start

## 1️⃣ Create a Rephon Machine

```python
import numpy as np
from rephon import RephonState

rcs = RephonState(
    N=128,
    W=1e9,
    T2=1e-3,
    T1=100e-3
)
```

## 2️⃣ Generate an Input Signal

```python
k = np.arange(128)

f = np.exp(-((k - 64)**2)/(2*12**2)) \
    * np.exp(2j*np.pi*k*5/128)
```

## 3️⃣ Run a Canonical Rephasing Circuit

```python
signal = (
    rcs
    .W(f)
    .E(50e-6)
    .Pi()
    .E(50e-6)
    .M()
)

print(np.argmax(np.abs(signal)))
```

---

# ⚡ Compute a DFT in One Cycle

```python
from rephon import rephon_dft

spectrum = rephon_dft(f)
```

✅ Fidelity 1.0 versus NumPy DFT

---

# 🎯 RESONANCE Algorithm

Single-shot matched filtering.

```python
from rephon import RESONANCE

template = f
signal = f * np.exp(2j*np.pi*k*20/128)

xcorr = RESONANCE(template, signal)

print(np.argmax(np.abs(xcorr)))
```

---

# 🎛️ The Fifteen Rephon Gates

## 🌌 Family 1 — Evolution

| Gate  | Purpose                      |
| ----- | ---------------------------- |
| ⏳ E   | Free evolution and dephasing |
| 🎚️ Φ | Global phase shift           |

---

## 🔄 Family 2 — Refocusing

| Gate    | Purpose                             |
| ------- | ----------------------------------- |
| 🔄 Π    | Broadband conjugation and rephasing |
| 🎯 Π(B) | Band-selective rephasing            |

---

## 🎛️ Family 3 — Rotation & Write

| Gate | Purpose                            |
| ---- | ---------------------------------- |
| 🧭 R | Frequency-selective Bloch rotation |
| ✍️ W | Write data into the machine        |

---

## 🎨 Family 4 — Spectral Shaping

| Gate     | Purpose                       |
| -------- | ----------------------------- |
| 🎨 S     | Spectral phase mask           |
| 📉 A     | Amplitude window              |
| 🌈 CHIRP | Adiabatic broadband inversion |

---

## 🚀 Family 5 — Multi-Pulse, Nonlinear & Readout

| Gate        | Purpose                          |
| ----------- | -------------------------------- |
| 📡 STIM     | Stimulated echo correlation      |
| 🛡️ CPMG    | Coherence protection             |
| ⚔️ XY16     | Robust dynamical decoupling      |
| 🎯 GRAPE_OC | Optimal control synthesis        |
| 🧠 NL       | Nonlinear Maxwell-Bloch dynamics |
| 📤 M        | Terminal readout                 |

---

# 🏗️ Fluent Circuit Builder

```python
from rephon import RephonCircuit
import numpy as np

f = np.exp(-np.linspace(-3,3,256)**2).astype(complex)

out = (
    RephonCircuit(N=256, W=1e9)
    .W(f)
    .CPMG(4, 1e-6)
    .E(50e-6)
    .Pi()
    .E(50e-6)
    .M(SNR_dB=30)
)
```

---

# 🌩️ Realistic Noise

```python
from rephon import RephonState
from rephon import RephonNoiseModel

noise = RephonNoiseModel(
    T2_noise_frac=0.05,
    spectral_diffusion_sigma=1e5,
    readout_SNR_dB=25,
    seed=42
)
```

Features:

* 📉 T₂ spread
* 🌫️ Spectral diffusion
* 📡 Detector noise
* 🎲 Reproducible random seeds

---

# 📚 Included Algorithms

| Function             | Description          |
| -------------------- | -------------------- |
| ⚡ rephon_dft()       | N-point DFT          |
| 🎯 RESONANCE()       | Matched filtering    |
| 🔗 rephon_convolve() | Circular convolution |

---

# 🔬 Physics Foundation

The machine state is the off-diagonal density matrix element:

```math
\psi_k = \tilde{\rho}_{eg}(\Delta_k)
```

Linear dynamics:

```math
\frac{d\psi_k}{dt}
=
i\Delta_k\psi_k
-
\frac{\psi_k}{T_2}
+
i\frac{\Omega(t)}{2}
```

Nonlinear dynamics are governed by the full Maxwell-Bloch equations integrated using:

✅ Fourth-Order Runge-Kutta (RK4)

The Rephon Transform Theorem shows that:

* ⚡ DFT
* 🔍 Correlation
* 🔗 Convolution
* ↩️ Time Reversal

can all emerge from a single write → evolve → conjugate → read sequence.

---

# 📖 Citation

If you use Rephon Computing in your research:

```bibtex
@software{rephon2026,
  author    = {Ahmed, Zuhair},
  title     = {Rephon Computing: Simulate the Physics of Rephasing Computation},
  year      = {2026},
  version   = {1.0.0},
  publisher = {CETQAC},
  note      = {Centre of Excellence for Technology, Quantum and AI, Canada}
}
```

---

# 📜 License

MIT License © 2026 Dr. Zuhair Ahmed

🏛️ CETQAC — Centre of Excellence for Technology, Quantum and AI (Canada)

⭐ If you find Rephon Computing useful, consider starring the repository and contributing to the future of rephasing-based computation.
