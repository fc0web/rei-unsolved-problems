# Problem 004 — gap_monotone λ₂ Carry-Propagation Decay

- **ID**: 004
- **Category**: Structural / linear algebra (Collatz-adjacent)
- **Source STEP**: Rei-AIOS STEP 627f (2026)
- **Status**: open, but **expected closable via Weyl perturbation theorem** (not Collatz-equivalent)
- **Discovered**: STEP 627f
- **Last updated**: 2026-04-17

## Statement (informal)

Rei の Collatz 解析で使われる **k-bit trailing-one residue (tro) 遷移行列** `P_k` (k = 8, 9, 10, ...) の第二固有値 `λ_2(P_k)` が **k 増加で単調減少** するか?

これが成立すれば spectral gap `gap(k) = 1 - λ_2(P_k)` が単調増加 → universal lower bound が立つ → tier2 を通じて Collatz 収束の構造が閉じる.

## Statement (formal)

```
gap_monotone :
  ∀ k ≥ 8,  λ_2(P_{k+1}) ≤ λ_2(P_k)
```

等価形:

```
∀ k ≥ 8,  gap(k+1) ≥ gap(k)  ∧  gap(k) ≥ ε_0 > 0
```

ここで `ε_0 ≈ 0.1` は実測値から推定される universal lower bound.

## Background

STEP 627f で Rei は bit k の carry 影響が `I_k ≤ C·exp(-0.35·k)` で指数減衰することを経験的に確認. Weyl 摂動定理により:

```
|λ_2(P_{k+1}) - λ_2(P_k)| ≤ 2·I_k
```

よって `I_k → 0` ならば λ_2 は Cauchy 列. あとは **単調性** を示せば完了.

## Why it matters

- **Collatz-equivalent ではない** — 純粋な線形代数 (carry propagation + Weyl perturbation)
- 閉じれば Collatz の 3 residual axioms (C1/C2/C3) のうち少なくとも **1 つが unconditional 化** する可能性
- Rei 全体の Lean 4 stack から **axiom が 1-2 個減る**

## What is verified (empirical)

- `I_k ≤ C·exp(-0.35·k)` for k ∈ [8, 14] (measurement)
- `gap(k) ≥ 0.1` for k ∈ [8, 12] (direct computation)
- Weyl 摂動定理は Mathlib にある (`Matrix.spectrum_perturbation`)

## What is unproven

- **bit influence I_k の指数減衰** の ∀k 証明 (carry-propagation 形式化)
- λ_2 の **厳密単調性** (non-strict なら Weyl で十分だが strict は別途)

## Candidate attack routes

- **Route A**: Carry propagation を 2-adic analysis で書き, Lean 4 形式化. Mathlib `Padic.Nat.log` 等を流用.
- **Route B**: Ising-like 簡略モデルへの還元 — 類似系で λ_2 単調性が証明されている
- **Route C**: tombursey drift lemma (STEP 841 bridge) との cross-application
- **Route D**: Goedel-Prover-V2 / DeepSeek-Prover-V2 で自動証明試行 (STEP 723 パイプライン)

## Independence assessment

STEP 627f で明記: **gap_monotone は Collatz 予想から独立**. 線形代数 + 2-adic 解析の範疇. **ZFC から閉じる見込み高**.

## References

- STEP 627f: `test/step627f-gap-monotone-test.ts`
- Rei Lean 4: existing Mathlib `Matrix.spectrum_*` API
- Weyl 1912 original eigenvalue perturbation
- Recommended reading: Kato, *Perturbation Theory for Linear Operators*
