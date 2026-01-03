# QSCM: Quality of Silence Control Model

Measuring decision friction in large-scale information spaces using ρ, τ, and σ.

> **Status**  
> This project is published during an active scaling phase  
> (e.g., 200k+ URLs observed, expanding toward much larger spaces). [cite: 2026-01-01]  
> The goal is not to claim completion, but to provide a **falsifiable** measurement protocol.

---

## TL;DR

- **Formula:**  
  Φ = 1 / (ρ + τ_norm + σ + ε)

- **Inputs:**  
  Exportable logs (GSC / GA4)

- **Stance:**  
  Published for **falsification**, not validation

- **Fast check:**  
  Run `example.py` and confirm Acceptance Tests

---

## Formula

QSCM defines the **Silence Index (Φ)** as the inverse of total decision friction:

```text
Φ = 1 / (ρ + τ_norm + σ + ε)
```

This formulation intentionally uses a simple inverse-sum structure to ensure that
each source of friction is explicit, comparable, and falsifiable.
[cite: 2026-01-01]

## Variables
ρ (rho)
Branching density / structural dispersion
(e.g. query → multiple-page click dispersion).
[cite: 2026-01-01]

τ_norm (normalized decision latency)
Human decision time, normalized to a reference duration.
[cite: 2026-01-01]

σ (sigma)
Real-world uncertainty or drop-off probability
(e.g. checkout started but not completed).
[cite: 2026-01-01]

ε (epsilon)
A sufficiently small constant for numerical stability (≈ 1e-6).
[cite: 2025-12-29]

## Normalization
Decision time is normalized as follows:

```text
τ_norm = τ / τ_ref
```

Where:

τ (tau)
Median decision time in seconds
(e.g. entry → checkout or purchase).
[cite: 2026-01-01]

τ_ref (reference time)
Reference decision duration (Default: 100 seconds).
[cite: 2026-01-01]

This normalization makes decision latency dimensionless and directly comparable
with ρ and σ.
[cite: 2026-01-01]

## Interpretation
Larger Φ
Lower overall friction → A more “silent” and decisive information path.
[cite: 2026-01-01]

Smaller Φ
Higher friction → Increased noise, hesitation, or operational mismatch.
[cite: 2026-01-01]

## Practical Meaning
τ_norm reflects how heavy the hesitation was.
[cite: 2026-01-01]

τ_ref encodes the ethical baseline of human thinking time.
[cite: 2025-12-27, 2026-01-01]

ε exists purely to keep the model mathematically safe
(avoid division by zero).
[cite: 2025-12-29]

## Acceptance Tests (A / B / C)
Implementation is considered valid if it reproduces the following values
(rounded to 3 decimals).

Assumptions:
τ_ref = 100, ε = 1e-6

| Pattern | ρ (rho) | τ (sec) | σ (sigma) | Expected Φ |
|--------|--------:|--------:|----------:|-----------:|
| A (Low friction) | 1.1 | 25 | 0.05 | 0.714 |
| B (Structural noise) | 4.5 | 180 | 0.15 | 0.155 |
| C (Operational mismatch) | 2.2 | 60 | 0.60 | 0.294 |

Quick Start
bash
python example.py
You should see the Acceptance Tests printed with matching outputs.

Feedback Loop (Issues)
This project prioritizes falsifiability over confirmation.
[cite: 2026-01-01]

If your measurements contradict this model, please open an Issue with your context
(e.g., e-commerce vs. content media, session definition, conversion definition).

License
MIT License

example.py
python

```python
def calculate_phi(rho, tau_sec, sigma, tau_ref=100.0, epsilon=1e-6):
    """
    Calculate the Silence Index (Phi) based on QSCM.
    """
    tau_norm = tau_sec / tau_ref
    denom = rho + tau_norm + sigma + epsilon
    return round(1.0 / denom, 3)

def main():
    tests = [
        ("A: Low friction", 1.1, 25, 0.05, 0.714),
        ("B: Structural noise", 4.5, 180, 0.15, 0.155),
        ("C: Operational mismatch", 2.2, 60, 0.60, 0.294),
    ]

    print("--- QSCM Acceptance Tests ---")
    for name, rho, tau, sigma, expected in tests:
        got = calculate_phi(rho, tau, sigma)
        print(f"[{'PASS' if got == expected else 'FAIL'}] {name}: {got}")

if __name__ == "__main__":
    main()
