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

### Practical Meaning

- **τ_norm** reflects *how heavy the hesitation was*  
- **τ_ref** encodes the *ethical baseline of human thinking time*  
- **ε** exists purely to keep the model mathematically safe
