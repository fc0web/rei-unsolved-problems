# Problem 003 — tier2 Residual C3a/C3b (mod-8 = 3 / mod-8 = 7)

- **ID**: 003
- **Category**: Collatz-equivalent (hardest residual)
- **Source**: Rei-AIOS Paper 106, STEP 840 Mod-8 Refinement
- **Status**: open, Collatz-equivalent
- **Discovered**: 2026-04-16
- **Last updated**: 2026-04-17

## Statement (informal)

C3 は C1/C2 よりさらに細かく分けた **mod-8** サブクラスの tier2 bound 成立問題.
mod-4 = 3 の hard class が mod-8 = 3 (98.59% 収束) と mod-8 = 7 (93.90%, 最難) に
細分される.

## Statement (formal)

```
C3a (mod-8=3 residual):
  ∀ n > 10⁶ ISOLATED, n ≡ 3 (mod 8) ⇒ K(n)·100 ≤ 444·bitLen(n)²

C3b (mod-8=7 hardest residual):
  ∀ n > 10⁶ ISOLATED, n ≡ 7 (mod 8) ⇒ K(n)·100 ≤ 444·bitLen(n)²
  (empirical rate: 93.9%; ~6% は σ_k descent 補完が未検証)
```

Lean 4 (axiomatized in Step840Mod8Refinement):
```lean
axiom C3a_mod8_eq_3 : ∀ n > 10^6, Odd n → n % 8 = 3 → isolated n →
  K n * 100 ≤ 444 * (bitLen n)^2

axiom C3b_mod8_eq_7 : ∀ n > 10^6, Odd n → n % 8 = 7 → isolated n →
  K n * 100 ≤ 444 * (bitLen n)^2
```

## Background

STEP 838 で mod-4 = 3 の hard class が:

- **mod-8 = 3**: 98.59% descend within k ≤ 20
- **mod-8 = 7**: 93.90%, hardest sub-class

と判明. Paper 106 の conditional proof の residual portion の中で最も密度が高い
証明 gap がこの C3a/C3b.

## Why C3b is hardest

n ≡ 7 (mod 8) は:
- Syracuse 写像適用後 S(n) = (3n+1)/2^v₂ で v₂ = 1 最大連続発生頻度
- trailing ones t₁ ≥ 3 の比率が高い → 連続 up-step
- STEP 836/837 で σ_k recursion k ≤ 20 内収束率が 93.9% に停滞

## Candidate attack routes

- **Route A**: Chang 2603.25753 burst-gap 解析 (mod-8 = 7 の high-bit gap-return
  structure).
- **Route B**: STEP 851 Wieferich flow signature との cross: mod-8 = 7 primes
  全体の Ricci flow 計測.
- **Route C**: STEP 843 FIA Infinity Algebra で formal 対称性を破る (mod-8 上の
  8 residue の algebraic role を D-FUMT₈ に射影).

## References

- [Paper 106](https://doi.org/10.5281/zenodo.19601546)
- [Step840Mod8Refinement.lean](https://github.com/fc0web/rei-aios/blob/main/data/lean4-mathlib/CollatzRei/Step840Mod8Refinement.lean)
- STEP 838 data: [data/step838-mod8-stride-sample.json](https://github.com/fc0web/rei-aios/blob/main/data/step838-mod8-stride-sample.json)
