# MVDR-EcFA-ISAC

**Enhanced Firefly Algorithm with Minimum Variance Distortionless Response Refresh for Full-Duplex MIMO Integrated Sensing and Communication Systems**

## Overview

MVDR-EcFA-ISAC is a research-oriented implementation of an Enhanced Firefly Algorithm (EcFA) for solving the joint waveform design and resource allocation problem in Full-Duplex (FD) MIMO Integrated Sensing and Communication (ISAC) systems.

The proposed framework jointly optimizes:

* Downlink communication precoder
* Sensing waveform matrix
* Uplink transmit power allocation
* Uplink receive beamformers
* Radar receive beamformer

under communication fairness, radar sensing performance, and transmit power constraints.

Unlike conventional gradient-based optimization methods, EcFA employs a derivative-free swarm intelligence approach capable of handling highly coupled and non-convex optimization problems arising in FD-ISAC systems.

---

## Research Motivation

Future 6G networks are expected to support both wireless communication and environmental sensing using a shared hardware and spectrum infrastructure.

Full-Duplex ISAC enables:

* Simultaneous downlink transmission
* Simultaneous uplink reception
* Continuous radar sensing

within the same time-frequency resource.

However, this introduces severe coupling among:

* Communication beamforming
* Radar waveform design
* Uplink power allocation
* Self-interference suppression

making the resulting optimization problem highly non-convex.

EcFA is proposed to efficiently search for high-quality solutions without requiring gradient information.

---

## System Model

The considered system consists of:

* A Full-Duplex MIMO Base Station

  * Nt transmit antennas
  * Nr receive antennas

* M Downlink users

* P Uplink users

* One radar target and multiple clutter sources

The BS simultaneously:

1. Transmits communication signals
2. Transmits sensing waveforms
3. Receives uplink transmissions
4. Performs radar sensing

using a shared transmit covariance matrix

Rtx = VcVcᴴ + VsVsᴴ

---

## Optimization Objective

The objective combines communication fairness and sensing reliability.

### Communication Utility

Weighted minimum SINR:

F_comm = SmoothMin[
μ min(SINR_UL), (1-μ) min(SINR_DL)
]

where:

* μ controls UL/DL priority
* SmoothMin avoids non-differentiability

### Penalty Terms

The objective includes:

* Radar SINR penalty
* Downlink fairness penalty
* Uplink fairness penalty
* Transmit power penalty

### Final Objective

maximize:

J = λ_comm * F_comm
− λ_radar * P_radar
− λ_DL * P_DL
− λ_UL * P_UL
− λ_pow * P_pow

subject to:

* BS power constraint
* UL power bounds

---

## Enhanced Firefly Algorithm (EcFA)

### Firefly Encoding

Each firefly represents

X = {Vc, Vs, ρ, {up}, w}

where:

* Vc : communication precoder
* Vs : sensing waveform
* ρ : uplink powers
* up : uplink receive beamformers
* w : radar receive beamformer

---

### Brightness Evaluation

Brightness corresponds to the optimization objective:

B(X) = J(X)

Higher brightness indicates a better communication-sensing tradeoff.

---

### Distance Metric

Distance between fireflies is computed from:

* Communication covariance matrices
* Sensing covariance matrices
* Power allocation vectors

using Frobenius and Euclidean norms.

---

### Attraction Mechanism

Firefly movement follows:

X_i ← X_i + β(X_j − X_i) + αN

where:

* β is attractiveness
* α is adaptive randomization
* N is Gaussian perturbation

---

### Global Best Guidance

Each firefly is additionally attracted toward the current global best solution:

X_i ← X_i + βg(X_best − X_i)

This accelerates convergence.

---

### SDSS Local Perturbation

A small decaying perturbation is applied:

δ = 10^-3 (1 − t/Tmax)^2

to improve local exploration and avoid premature convergence.

---

### MVDR Refresh Operator

After every movement:

* Uplink combiners are updated via MVDR
* Radar combiner is updated via MVDR

This significantly reduces search dimensionality and improves stability.

---

### Mutation and Local Search

The algorithm periodically:

* Mutates the worst fireflies
* Performs local exploitation around the best solution

to maintain population diversity.

---

## Algorithm Flow

1. Initialize firefly population
2. Compute communication and radar metrics
3. Evaluate brightness
4. Firefly attraction
5. Global-best guidance
6. SDSS perturbation
7. MVDR refresh
8. Mutation
9. Local search
10. Update best solution
11. Repeat until Tmax

---

## Key Features

* Full-Duplex MIMO-ISAC optimization
* Fairness-aware communication design
* Joint radar and communication optimization
* Enhanced Firefly Algorithm
* MVDR-assisted beamformer refresh
* Global-best guidance mechanism
* SDSS adaptive perturbation
* Constraint handling through soft penalties

---

## Project Structure

```text
EcFA-ISAC/
│
├── run.m
├── EcFA_ISAC_function.m
├── brightness.m
├── compute_DL_SINR.m
├── compute_UL_SINR.m
├── Compute_Radar_SINR.m
└── README.md
```

---

## Expected Outputs

The framework reports:

* Best objective value
* Minimum DL SINR
* Minimum UL SINR
* Radar SINR
* Fairness metrics
* Convergence curves

---

## Citation

