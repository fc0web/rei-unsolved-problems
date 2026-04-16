# Problem 009 — Perelman Flow Convergence Conditions on Weighted Graphs

- **ID**: 009
- **Category**: Dynamical / graph
- **Source**: Rei-AIOS STEP 846 (2026-04-17)
- **Status**: open, dynamical
- **Discovered**: 2026-04-17
- **Last updated**: 2026-04-17

## Statement (informal)

Rei が STEP 846 で実装した discrete Ollivier-Ricci flow:

```
w_{t+1}(e) = w_t(e) · exp(-2·κ(e)·Δt)
```

は **どのような条件下で収束**するか? つまり、`|w_{t+1} - w_t|_∞ → 0` and
`|T_{w_∞}| finite` となる initial graph + curvature function の class は?

## Statement (formal)

以下のどれが最も弱い条件か:

(A) 有限 graph + bounded curvature ⇒ 収束?
(B) ∀ e, κ(e) ≥ 0 ⇒ 収束 (PFA-6 確認済)?
(C) 平均 κ ≥ 0 ⇒ 収束?
(D) Perron-Frobenius eigenvalue λ₁ < 1 ⇒ 収束?

または他の条件?

## Background

STEP 847 で Goldbach graphs は **S (stable)**, Collatz は **M (moderate)**,
Andrica は **E (explosive)** と分類. Andrica は全曲率が強い負 (−19 〜 −119)
で発散. この分類の数学的意味:

- Category S: 経験的に収束
- Category E: 経験的に発散 (per-edge > 1 の singularity)
- Category M: 中間 (有限 time で停止するが singularity ≈ 37%)

**open**: この 3-category 分類の **analytic threshold** は何か? 連続
Hamilton-Perelman flow との対応は?

## What is verified

| 観点 | 状態 |
|------|------|
| 6 PFA axioms | ✅ STEP 846 (40/40 test) |
| positive-κ convergence (PFA-6) | ✅ for uniformly non-neg κ |
| S/M/E classification (empirical) | ✅ STEP 847-849 |
| **収束条件の characterization** | ❌ open |
| 連続 Ricci flow との correspondence | ❌ open |

## Candidate attack routes

- **Route A**: Lyapunov 関数 `V(w) = Σ w(e) log w(e)` が κ ≥ 0 で monotone
  decreasing かつ bounded below を示す.
- **Route B**: Perron-Frobenius 固有値解析 (STEP 786 transfer operator 類似).
- **Route C**: Ollivier (2009) 連続版 Theorem 33 の discrete 対応を証明.

## Why it matters

- 一般 graph 上の Ricci flow は未だ解析的 characterization がない (連続
  Hamilton-Perelman は成功, discrete は個別例のみ).
- Rei の S/M/E 分類が理論化できれば、未解決問題 graph に apriori で
  解ける / 解けないを判定できる Ricci-flow-decidability 理論になる.

## References

- STEP 846: [perelman-flow-engine.ts](https://github.com/fc0web/rei-aios/blob/main/src/axiom-os/perelman-flow-engine.ts)
- [Paper 108 draft](https://github.com/fc0web/rei-aios/blob/main/papers/paper-108-ricci-flow-three-category-classification.md)
- Ollivier, Y. (2009). *Ricci curvature of Markov chains on metric spaces.*
  J. Funct. Anal. 256, 810–864.
