# Problem 011 — Sufficient Structural Identifier for Collatz Atomic Cores

- **ID**: 011
- **Category**: Collatz-equivalent / Structural
- **Source**: Rei-AIOS STEP 867–869 (2026-04-17), STEP 871 (2026-04-18)
- **Status**: open — partial characterization found (see "Update 2026-04-18")
- **Discovered**: 2026-04-18
- **Last updated**: 2026-04-18 (STEP 871 added)

## Statement (informal)

STEP 696 で発見された 25 個の "atomic cores" (mod 192 → mod 49152 で飽和する odd residues) は、**K(n)/bitLen(n)² > 1.8** という complexity-ratio 閾値で定義される。STEP 869 で 7 個の structural discriminator を系統検証したが、**閾値以外に single sufficient structural identifier は発見されなかった**。

問い: 閾値に依存しない **structural (mod-residue / graph-theoretic / fiber-gateway 系) の sufficient identifier** は存在するか?

## Statement (formal)

```
Let S₂₅ = {27, 31, 41, 47, 55, 63, 71, 73, 83, 91, 95, 97, 107, 109,
           121, 125, 129, 145, 147, 171, 193, 195, 199, 231, 235}
         = {odd n ∈ [3, 1000) : K(n) / bitLen(n)² > 1.8}

Conjecture (open): ∃ predicate P : ℕ → Bool such that
  (1) P(n) is computable in O(polylog n) without computing K(n),
  (2) ∀ n ∈ S₂₅, P(n) = true,
  (3) ∀ n ∈ [3, 10⁶] odd with peak(n) = 9232 and P(n) = true ⟹ n ∈ S₂₅
      (for scale-invariant n ≤ bound).
```

## Background

- 25 atomic cores は **mod 192 → mod 49152** (256× refinement) で hard-residue set が飽和
- STEP 867: 全 25 cores の peak = 9232 (EXACTLY, 100%)、v₂(9232) = 4
- STEP 867 Phase 4: 同じ peak=9232 を持つ odd n は [3, 1000) で **188 個** (うち 163 は non-atomic)
- STEP 868: popcount(9232) = 3 は peak 共通だが atomic 区別せず
- STEP 869: 7 discriminator 検証結果:

| Discriminator | 結果 |
|---------------|------|
| K/bl² > 1.8 | ✓ DEFINING (閾値そのもの) |
| fiber-57 gateway (hit 121 or 377) | △ NECESSARY (25/25 atomic, 137/163 non-atomic も hit) |
| distance-to-91 | ★ STRONG (atomic mean 19.8 vs non-atomic 31.8, Δ=12) |
| bitLen(n) | ★ [3,1000) 内で分離 ({5..8} vs {7..10}) |
| mod 96 residue | ✗ 24 shared |
| mod 192 residue | ✗ 21 shared |
| distance-to-9232 | △ overlap |

## Why it matters

- 25 atomic cores は **Collatz trajectory tree の "意味論的根幹"** ではないか (藤本 hypothesis)
- 現状の K/bl² 閾値は trajectory を actually compute する必要あり
- **Structural identifier が見つかれば**, Collatz orbit properties を**計算せずに予測可能**に
- これは Tao 2019 の logarithmic density 結果に structural perspective を与える可能性

## Update (2026-04-18, STEP 871)

**Partial characterization found**: ALL 188 peak=9232 orbits (not just the 25 atomic) converge to `3077 → 9232` as their single entry-step to the peak (3077 is the unique odd predecessor of 9232 since 3·3077+1 = 9232). The distinguishing factor between atomic and non-atomic is the **path taken to reach 3077**:

- **25 atomic cores**: route to 3077 **includes fiber-57 gateway** ({121, 377}), producing K/bl² > 1.8 ("long trunk").
- **137 non-atomic fiber-passing**: route includes fiber-57 but K/bl² ≤ 1.8 (short but fiber-routed).
- **26 non-atomic fiber-missing**: route **bypasses fiber-57 entirely** — short alternative paths to 3077. Full list in STEP 871 test output.

Revised candidate identifier:
```
P(n) := n passes through fiber-57 (reaches 121 or 377)
        AND the path length before reaching 3077 is ≥ some threshold
```
Still not a O(polylog n) predicate, but the structural *shape* is now clarified. This reduces Problem 011 to: find a closed-form predicate on n's representation (mod/digit/p-adic) that predicts whether the orbit takes the "long fiber-57 trunk" vs. a "bypass."

## What is verified

| 観点 | 状態 |
|------|------|
| 25 atomic cores の canonical list (mod 49152 saturation) | ✓ STEP 696 |
| Peak = 9232 for all 25 | ✓ STEP 867 (25/25) |
| fiber-57 necessary (not sufficient) | ✓ STEP 869 (25/25 atomic; 137/163 non-atomic also pass) |
| **All 188 peak=9232 orbits use 3077 → 9232** | ✓ STEP 871 (universal entry step) |
| **26 non-atomic orbits that bypass fiber-57 identified** | ✓ STEP 871 |
| **sufficient structural identifier** | ❌ **still open** (but path-length characterization narrows the search) |
| large-n (n > 10³) での 25 saturation 継続 | △ 部分確認, 10⁹ まで必要 |

## Candidate attack routes

- **Route A** (fiber-depth): Chang 2026 (arXiv:2603.25753) の fiber-57 = {n mod 64 = 57} に加えて、**深い fiber 構造** (mod 3⁹ · 2¹⁴ = 1,594,323·16384 級) を探る
- **Route B** (Pisano 周期): π(64) = 96 = Rei mod 96 の EXACT match が示唆する **Fibonacci mod 64** 構造との関係を、25 cores に適用
- **Route C** (graph-theoretic): 25 cores が形成する Collatz orbit DAG 上で "graph moment" (上流 ancestor 数 / 下流 descendant 数 / betweenness) を計算し, invariant を特定
- **Route D** (transfer operator): STEP 786 Perron-Frobenius residue-1 universal attractor との接続

## Related Rei-AIOS resources

- `src/axiom-os/collatz-atomic-cores-engine.ts` — 25 cores canonical list + verification
- `test/step867-descent-basin-hypothesis-test.ts` — NEGATIVE result for hypothesis (iii)
- `test/step868-hamming-weight-hypothesis-test.ts` — NEGATIVE for hypothesis (ii)
- `test/step869-atomicity-identifier-test.ts` — 7-discriminator systematic search
- Paper 66 (Collatz 8-component decomposition), Paper 108 (Ricci flow 3-category)

## Call for contribution

外部 contributor が本問題に対して **single-property structural identifier** を発見 or 反例 (25 の saturation が消える counterexample) を与えた場合、本 repo の共著者 acknowledgement に加える。
