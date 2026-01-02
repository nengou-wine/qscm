### Normalization Details

Where:

- **τ_norm = τ / τ_ref**
- **τ_ref = 100 seconds (default)**
- **ε (epsilon) ≈ 1e-6**

#### Meaning

- **τ (tau)** represents the actual decision time in seconds  
  (e.g. time from entry to checkout or purchase).

- **τ_norm** is a normalized, dimensionless value that allows decision time  
  to be compared fairly with other friction components (ρ and σ).

- **τ_ref (reference time)** defines what is considered a *natural thinking duration*  
  in the given context.  
  In this model, `τ_ref = 100s` reflects a high-involvement decision space  
  (e.g. wine selection), where thoughtful consideration is expected and respected.

  Examples:
  - τ = 25s  → τ_norm = 0.25  
  - τ = 60s  → τ_norm = 0.60  
  - τ = 180s → τ_norm = 1.80  

- **ε (epsilon)** is a numerically negligible constant added for stability,  
  preventing division-by-zero when all friction components approach zero.  
  It has no practical impact on the resulting value of Φ.

In short:

> τ_norm represents *how heavy the hesitation was*,  
> τ_ref encodes *the ethical baseline of human thinking time*,  
> and ε exists purely to keep the model mathematically safe.
