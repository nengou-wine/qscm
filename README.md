## Formula

QSCM defines the **Silence Index (Φ)** as the inverse of total decision friction:

```text
Φ = 1 / (ρ + τ_norm + σ + ε)


This formulation intentionally uses a simple inverse-sum structure
to ensure that each source of friction is **explicit, comparable, and falsifiable**.

---

### Variables

- **ρ (rho)**  
  Branching density / structural dispersion  
  (e.g. query → multiple-page click dispersion in search results)

- **τ_norm (normalized decision latency)**  
  Human decision time, normalized to a reference duration

- **σ (sigma)**  
  Real-world uncertainty or drop-off probability  
  (e.g. checkout started but not completed)

- **ε (epsilon)**  
  A sufficiently small constant for numerical stability

---

### Normalization

Decision time is normalized as follows:
τ_norm = τ / τ_ref

Where:

- **τ (tau)**  
  Median decision time in seconds  
  (e.g. entry → checkout or purchase)

- **τ_ref (reference time)**  
  Reference decision duration  
  Default: **100 seconds**

- **ε (epsilon)**  
  Assumed to be sufficiently small (≈ 1e-6)

This normalization makes decision latency **dimensionless** and
directly comparable with ρ and σ.

---

### Interpretation

- Larger **Φ**  
  → Lower overall friction  
  → A more “silent” and decisive information path

- Smaller **Φ**  
  → Higher friction  
  → Increased noise, hesitation, or operational mismatch

---

## QSCM — Quality of Silence Computational Model

QSCM is a minimal, falsifiable model for measuring **decision friction**
in large-scale information spaces.

It defines a single scalar value — the **Silence Index (Φ)** —
which quantifies how smoothly a user can move from intent to decision.

The model is intentionally simple:
- each source of friction is explicit,
- each component is measurable,
- and the result is reproducible from real logs.

This repository provides:
- the formal definition of Φ,
- normalization rules,
- and a reference implementation for verification.

## Formula

QSCM defines the **Silence Index (Φ)** as the inverse of total decision friction:

Φ = 1 / (ρ + τ_norm + σ + ε)

This formulation intentionally uses a simple inverse-sum structure
to ensure that each source of friction is **explicit, comparable, and falsifiable**.

### Variables

- **ρ (rho)**  
  Branching density / structural dispersion  
  (e.g. query → multiple-page click dispersion in search results)

- **τ_norm (normalized decision latency)**  
  Human decision time, normalized to a reference duration

- **σ (sigma)**  
  Real-world uncertainty or drop-off probability  
  (e.g. checkout started but not completed)

- **ε (epsilon)**  
  A sufficiently small constant for numerical stability

### Interpretation

- Larger **Φ**  
  → Lower overall friction  
  → A more “silent” and decisive information path

- Smaller **Φ**  
  → Higher friction  
  → Increased noise, hesitation, or operational mismatch

### Practical Meaning

- **τ_norm** reflects *how heavy the hesitation was*
- **τ_ref** encodes the *ethical baseline of human thinking time*
- **ε** exists purely to keep the model mathematically safe

## Acceptance Tests (Reference Values)

The following examples provide reproducible reference values.

Assume:
- τ_ref = 100
- ε = 0.000001

### Case A — Low friction
ρ = 0.20  
τ = 30s → τ_norm = 0.30  
σ = 0.10  

Φ ≈ 1 / (0.20 + 0.30 + 0.10 + ε) ≈ **1.67**

### Case B — Medium friction
ρ = 0.40  
τ = 80s → τ_norm = 0.80  
σ = 0.20  

Φ ≈ 1 / (1.40) ≈ **0.71**

### Case C — High friction
ρ = 0.80  
τ = 150s → τ_norm = 1.50  
σ = 0.40  

Φ ≈ 1 / (2.70) ≈ **0.37**

# example.py
# Reference implementation of QSCM Silence Index

EPSILON = 1e-6
TAU_REF = 100.0

def silence_index(rho, tau, sigma, tau_ref=TAU_REF, epsilon=EPSILON):
    tau_norm = tau / tau_ref
    return 1.0 / (rho + tau_norm + sigma + epsilon)

if __name__ == "__main__":
    cases = {
        "A": (0.20, 30, 0.10),
        "B": (0.40, 80, 0.20),
        "C": (0.80, 150, 0.40),
    }

    for name, (rho, tau, sigma) in cases.items():
        phi = silence_index(rho, tau, sigma)
        print(f"Case {name}: Φ = {phi:.2f}")


### Practical Meaning

- **τ_norm** reflects *how heavy the hesitation was*  
- **τ_ref** encodes the *ethical baseline of human thinking time*  
- **ε** exists purely to keep the model mathematically safe

---

## Acceptance Tests (Reference Benchmarks)

The following reference cases are provided to allow **instant validation**
of any implementation of QSCM.

All values are illustrative but internally consistent.

---

### Case A: Low Friction (Ideal / Silent Path)

A highly focused information space with minimal branching and hesitation.

- ρ = 0.10
- τ = 30s
- τ_ref = 100s → τ_norm = 0.30
- σ = 0.05
- ε ≈ 0

Φ = 1 / (0.10 + 0.30 + 0.05)  
Φ ≈ **2.22**

**Expected interpretation**  
- Very low friction  
- Near-silent decision flow  
- Strong alignment between structure and user intent

---

### Case B: Moderate Friction (Typical / Balanced)

A realistic decision environment with some exploration and uncertainty.

- ρ = 0.40
- τ = 80s
- τ_ref = 100s → τ_norm = 0.80
- σ = 0.30
- ε ≈ 0

Φ = 1 / (0.40 + 0.80 + 0.30)  
Φ ≈ **0.67**

**Expected interpretation**  
- Moderate friction  
- Thoughtful but slightly noisy decision process  
- Acceptable operational balance

---

### Case C: High Friction (Noisy / Broken Path)

A fragmented structure with excessive hesitation and drop-off.

- ρ = 1.20
- τ = 180s
- τ_ref = 100s → τ_norm = 1.80
- σ = 0.90
- ε ≈ 0

Φ = 1 / (1.20 + 1.80 + 0.90)  
Φ ≈ **0.25**

**Expected interpretation**  
- Severe friction  
- Cognitive overload or structural mismatch  
- System requires redesign or pruning

---

These benchmarks serve as **falsifiable reference points**.
Any correct implementation should reproduce values
within a small numerical tolerance.

