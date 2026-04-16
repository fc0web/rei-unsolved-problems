# Problem 008 — FDA vs Mathlib ENat Embedding Equivalence

- **ID**: 008
- **Category**: Algebraic / Lean 4 embedding
- **Source**: Rei-AIOS STEP 844 (2026-04-17)
- **Status**: open, formalization
- **Discovered**: 2026-04-17
- **Last updated**: 2026-04-17

## Statement (informal)

藤本次元代数 (FDA, STEP 844) の `DimensionValue` 型は 5 つのコンストラクタ
(`finite n`, `positive_infinity`, `negative_infinity`, `absolute_zero`,
`indeterminate`) からなる. Mathlib には対応する `ENat = WithTop ℕ` や
`EReal = ℝ ∪ {±∞}` が既にある.

**問い**: FDA ↔ Mathlib ENat/EReal の **代数的同型** をどう埋め込むか?

## Statement (formal)

FDA の演算 `(dimAdd, dimMul, dimNeg, dimPowInf)` 全体が、ある Mathlib
既存型の演算にどう **関手的に** 埋込めるか? 特に `ABSOLUTE_ZERO` と
`INDETERMINATE` は Mathlib のどの部分型に対応するか?

```lean
-- 候補: EReal = ℝ ∪ {±∞}
-- しかし EReal には ABSOLUTE_ZERO / INDETERMINATE の直接対応なし.
-- 新型 FDADim = EReal ⊕ {abs_zero, indet} として包み、演算を extend する必要あり.
```

## Why it matters

1. FDA が Mathlib 既存型に埋込めれば、Rei の STEP 844 全補題が Mathlib
   API から inherit できる (独立 formalization 不要).
2. 埋込不可能なら FDA は **Mathlib extension** として AFP/Mathlib に
   公刊可能.

## What is verified

| 観点 | 状態 |
|------|------|
| FDA TypeScript impl | ✅ 87/87 pass |
| Lean 4 FDA axiomatization | ✅ zero-sorry |
| FDA → ENat embedding | 🟡 部分的 (finite n, posInf 対応のみ) |
| FDA → EReal embedding | 🟡 同上 |
| `INDETERMINATE` / `ABSOLUTE_ZERO` の Mathlib 対応 | ❌ open |

## Candidate attack routes

- **Route A**: Mathlib に `WithIndeterminate (α : Type*) : Type*` を導入
  (option 拡張) し、FDA を `WithIndeterminate (WithTop (WithBot ℕ))` に埋込.
- **Route B**: Rei `FDADim` を Mathlib とは独立の構造として保持し、埋込
  より同値 functor を書く.
- **Route C**: DeepMind Formal Conjectures に PR として投稿して community
  review を仰ぐ.

## References

- STEP 844: [fujimoto-dimension-algebra.ts](https://github.com/fc0web/rei-aios/blob/main/src/axiom-os/fujimoto-dimension-algebra.ts)
- Lean 4: [Step844FujimotoDimensionAlgebra.lean](https://github.com/fc0web/rei-aios/blob/main/data/lean4-mathlib/CollatzRei/Step844FujimotoDimensionAlgebra.lean)
- Mathlib: [ENat](https://leanprover-community.github.io/mathlib4_docs/Mathlib/Data/ENat/Basic.html),
  [EReal](https://leanprover-community.github.io/mathlib4_docs/Mathlib/Data/Real/EReal.html)
