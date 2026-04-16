# Problem 007 — Fujimoto Infinity Algebra (FIA) Axiom Independence

- **ID**: 007
- **Category**: Algebraic / axiom independence
- **Source**: Rei-AIOS STEP 843 (2026-04-17)
- **Status**: open, axiom-theoretic
- **Discovered**: 2026-04-17
- **Last updated**: 2026-04-17

## Statement (informal)

藤本無限代数 (FIA, STEP 843) の 6 公理 FIA-1 〜 FIA-6 は **互いに独立** か?
すなわち、1 つの axiom を除去したとき、残り 5 つの axiom からそれを導出できるか?

## Statement (formal)

FIA 公理:
- **FIA-1** INFINITY + finite = INFINITY (additive absorbing)
- **FIA-2** INFINITY + INFINITY = INFINITY, INFINITY × INFINITY = INFINITY, ZERO + ZERO = ZERO (idempotence)
- **FIA-3** ZERO × finite = ZERO (multiplicative absorbing)
- **FIA-4** SELF op x = SELF for x ≠ NEITHER (Gödel fixed-point)
- **FIA-5** 0·∞, ∞-∞, ∞/∞, 0⁰ → NEITHER (indeterminate forms)
- **FIA-6** FLOWING + finite/ZERO = FLOWING, FLOWING × INFINITY = INFINITY

**Open questions**:

1. 各 FIA-i が他の 5 つから独立か? (**independence**)
2. FIA 全 6 公理を満たす model は一意か? (**categoricity**)
3. FIA は soundness 内部に矛盾を含まないか? (**consistency**)

## Background

STEP 843 で TypeScript 側 93/93 test で全 axiom を検証し、Lean 4 で公理化
(zero-sorry, 6 axioms). Concrete Cayley 表が唯一の model なので model は
存在する (inconsistency はなし).

しかし:
- **axiom 独立性** は未証明 — 例えば FIA-2 (idempotence) が FIA-1/4 から
  導出されるなら axiom 数を減らせる可能性あり.
- **categoricity** は一般に axiom 数 < モデル自由度 の場合 open.

## What is verified

| 観点 | 状態 |
|------|------|
| 6 axioms TypeScript 検証 | ✅ 93/93 pass |
| Lean 4 cross-verification | ✅ zero-sorry |
| Cayley model existence | ✅ explicit table |
| **axiom independence** | ❌ open |
| **categoricity** | ❌ open |
| **consistency** | ✅ implied by model |

## Candidate attack routes

- **Route A**: 各 FIA-i について、それを否定する別 model を構成
  (axiom -i を削除しつつ残りを満たす別 Cayley 表).
- **Route B**: Lean 4 で各 axiom の独立性を `by decide` or 具体 model で
  形式化.
- **Route C**: Goedel-Prover-V2 に独立性問題を入力し自動探索.

## Why it matters

- 独立性が示せれば FIA は **minimal** 公理系として公刊できる.
- 独立性が破れれば公理を減らせ、FIA は Rei の 19 axioms 全体の中で
  さらに核心に圧縮できる.

## References

- STEP 843: [fujimoto-infinity-algebra.ts](https://github.com/fc0web/rei-aios/blob/main/src/axiom-os/fujimoto-infinity-algebra.ts)
- Lean 4: [Step843FujimotoInfinityAlgebra.lean](https://github.com/fc0web/rei-aios/blob/main/data/lean4-mathlib/CollatzRei/Step843FujimotoInfinityAlgebra.lean)
