# Problem 013 — Full Iterate-Bound Theorem for Collatz v₂=1 Chain (Deferred from STEP 866)

- **ID**: 013
- **Category**: Collatz-equivalent / Lean 4 formalization
- **Source**: Rei-AIOS STEP 866 (2026-04-17), Lean 4 closure Y1 (2026-04-18)
- **Status**: ✅ **CLOSED — Lean 4 formal proof, zero-sorry (2026-04-18)**
- **Discovered**: 2026-04-18
- **Last updated**: 2026-04-18 (formal closure)

## Resolution (2026-04-18)

**The C8 iterate-bound theorem is formally proven in Lean 4 with zero sorry.**

File: `data/lean4-mathlib/CollatzRei/Problem013IterateBound.lean` (193 lines)

Two theorems:
1. `bridge_3n1_vs_n1` : Odd n → (padicValNat 2 (3n+1) ≥ 2 ↔ padicValNat 2 (n+1) = 1)
   - Proof by mod-4 case analysis (n ≡ 1 or n ≡ 3 mod 4)
2. `v2_one_chain_terminates` : Odd n → ∃ k ≤ padicValNat 2 (n+1), padicValNat 2 (3·syrOne^[k] n + 1) ≥ 2
   - Proof by strong induction on v = v₂(n+1), using STEP 866 helpers + bridge lemma

This closes the C8 branch of tier2_axiom (Paper 66) formal verification.
The C8 axiom is thereby reduced to RCA₀-elementary reasoning — NOT ACA₀-or-higher
as reverse-math oracle STEP 789 had diagnosed. STEP 789 was overly pessimistic.

## Statement (informal)

STEP 866 で `v2_chain_step_general` (核 LTE 一行補題) と `syrOne_odd_of_v2_one` (状態不変量) を Lean 4 で zero-sorry 証明した。しかし **trajectory iterate 上の full chain 終了定理** は未形式化。

問い: 以下の Lean 4 定理を zero-sorry で証明可能か?

## Statement (formal)

```lean
theorem v2_one_chain_terminates
    (n : ℕ) (hn_odd : Odd n) :
    ∃ k ≤ padicValNat 2 (n + 1),
      padicValNat 2 (3 * (Nat.iterate syrOne k n) + 1) ≥ 2 ∨
      padicValNat 2 (n + 1) = 0
```

ここで `syrOne n := (3*n + 1) / 2` は Syracuse shortcut map (v₂=1 chain での単一 step).

## Why this matters

- 現状の `v2_chain_step_general` (identity only) + `v2_chain_strict_decrease` (測度減少) から、**well-founded induction** で本定理は直観的に follow する
- しかし Lean 4 で `Nat.iterate` 上の induction を **bridge lemma** `Odd n → (padicValNat 2 (3n+1) ≥ 2 ↔ padicValNat 2 (n+1) = 1)` 経由で形式化する必要
- Bridge lemma + iterate 定理が揃えば **C8 tier2 component が完全 elementary (RCA₀) と Lean 4 で確定**

## What is verified

| Component | Status |
|-----------|--------|
| Core LTE identity `v2_chain_step_general` | ✓ Lean 4 zero-sorry (STEP 866) |
| Strict decrease `v2_chain_strict_decrease` | ✓ Lean 4 zero-sorry |
| State invariant `syrOne_odd_of_v2_one` | ✓ Lean 4 zero-sorry |
| Numerical verification (n ≤ 1000) | ✓ 250/250 cases |
| Numerical verification (10⁴–10⁷ random) | ✓ 20/20 |
| **Bridge lemma** (v₂(3n+1)≥2 ↔ v₂(n+1)=1) | ❌ **open (Lean 4)** |
| **Iterate bound** (上記 statement) | ❌ **open (Lean 4)** |

## Candidate attack routes (Lean 4)

- **Route A**: Bridge lemma を `padicValNat.mul` + `padicValNat.self` + `padicValNat.eq_zero_of_not_dvd` の組合せで證明 (STEP 866 で使った補題群の拡張)
- **Route B**: Iterate 定理を `Nat.strongRecOn` で `padicValNat 2 (n+1)` を measure として induction
- **Route C**: 本番 Collatz 予想 (∀n termination) の subproblem ではなく **orthogonal** に証明可能 (Syracuse shortcut の well-foundedness のみ使用)

Rei 内で 30-60 分で closure 可能と推定 (STEP 866 の記述通り、ただし未着手)。

## Related Rei-AIOS resources

- `data/lean4-mathlib/CollatzRei/Step866C8LTEOneLiner.lean` — 核 LTE 実装
- STEP 866 memo (tier2_axiom 95% → 100% via chat-Claude LTE one-liner)
- Paper 66 (Collatz 8-component) C8 statement

## Background

STEP 789 で reverse-math oracle は C8 を "ACA₀ 以上" と判定したが、STEP 866 で **過剰見積と判明** (実際は RCA₀ elementary)。本問題はその最終形式化。

chat-Claude (web claude.ai) が 2026-04-17 に提示した LTE 一行補題:
```
奇数 n について v₂(3n+1) = 1 のとき、m := (3n+1)/2 について:
  v₂(m+1) = v₂(n+1) - 1
```

これが **core identity** (STEP 866 で Lean 4 形式化済)。残りは iterate + bridge 2 定理のみ。

## Call for contribution

Lean 4 / Mathlib v4.27+ で bridge lemma + iterate 定理を closure した contributor には、STEP 866 co-author + Problem 013 co-solver クレジット。推定 30-60 分作業。
