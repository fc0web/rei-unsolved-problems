# Problem 002 — tier2 Residual C2 (TAIL ∀n > 10⁸ Universal)

- **ID**: 002
- **Category**: Collatz-equivalent
- **Source**: Rei-AIOS Paper 106/107, Lean 4 Step828IsolatedTighten
- **Status**: open, Collatz-equivalent
- **Discovered**: 2026-04-16
- **Last updated**: 2026-04-17

## Statement (informal)

10⁸ を超えるすべての odd ISOLATED n (軌道 peak が 4 primary attractors
{9232, 13120, 4372, 1672} 以外) について、tier2 bound `K(n)·100 ≤ 444·bitLen(n)²`
は成立するか?

## Statement (formal)

```
C2 (TAIL universal, parameterized by N = 10⁸):
  ∀ n > 10⁸, odd,
    peak(n) ∉ {9232, 13120, 4372, 1672} ⇒ K(n)·100 ≤ 444·bitLen(n)²
```

Lean 4:
```lean
axiom C2_tail_universal :
  ∀ n > 10^8, Odd n → peak n ∉ ({9232, 13120, 4372, 1672} : Set ℕ) →
    K n * 100 ≤ 444 * (bitLen n)^2
```

## Background

C1 (Problem 001) は全 n > 235 を ISOLATED 条件下で支配する. C2 はその
10⁸ 超領域版. empirical には 10⁷/10⁸ stride で 8.46M integer を 0 violations
で verify したが, ∀ n > 10⁸ の全称は open.

## Rei-AIOS 最新証拠 (2026-04-17, STEP 852)

10⁹ ≤ n ≤ 10¹⁰ の 50,000 sparse sample で:

- **violations: 0**
- **max ratio: 0.1531** (Paper 106 baseline 0.4009 より 62% 低下)

すなわち n が大きくなるほど bound は **ゆるく** なる (ratio 減少).
これは C2 の成立 direction を強く支持するが, ∀ 証明ではない.

## What is verified

| Range | Odd ISOLATED n | Violations | Max ratio |
|-------|---------------|------------|-----------|
| [237, 10⁷] | 4,999,475 | 0 | — |
| [10⁷, 10⁸] stride 13 | 3,461,539 | 0 | 0.4009 at n=871 |
| [10⁹, 10¹⁰] stride 180,002 | 50,000 | **0** | **0.1531** |

## Candidate attack routes

- **Route A**: tombursey Lemma 9 basin-capture の direct port — 10⁸ ベース case
  は proven (decide), C2 は large-n drift argument.
- **Route B**: STEP 852 の ratio 減少パターンから asymptotic bound K/bl² → 0 を
  分離的に示す.
- **Route C**: Paper 108 の N^(-1/2) 法則 (STEP 855) と組み合わせて primary funnel
  領域を exclude しつつ進める.

## References

- [Paper 106](https://doi.org/10.5281/zenodo.19601546)
- STEP 852: [scripts/step852-tier2-10e10-sparse-sample.py](https://github.com/fc0web/rei-aios/blob/main/scripts/step852-tier2-10e10-sparse-sample.py)
- [Step828IsolatedTighten.lean](https://github.com/fc0web/rei-aios/blob/main/data/lean4-mathlib/CollatzRei/Step828IsolatedTighten.lean)
