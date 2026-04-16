# Problem 005 — Andrica Ricci-Flow Explosion Hypothesis

- **ID**: 005
- **Category**: Ricci-flow class (novel)
- **Source STEP**: Rei-AIOS STEP 847 (2026-04-17)
- **Status**: open, **novel** (Rei-AIOS 2026-04-17 発見)
- **Discovered**: 2026-04-17
- **Last updated**: 2026-04-17

## Statement (informal)

連続素数 `p_i, p_{i+1}` 間のエッジに **Andrica 値** `w(e_i) = √p_{i+1} − √p_i` を重みとする graph 上で、**discrete Ollivier-Ricci flow**

```
w_{t+1}(e) = w_t(e) · exp(-2·κ(e)·Δt)
```

を走らせると、**edge あたり singularity 数が 1 を超える** (p≤100 で 1.67, p≤10000 でも 1.16). 一方で同じ flow が Collatz orbit graph (n=27) では 0.37, Goldbach partition graph では 0.03 しか singularity を生まない. この **3-category 分類** は何を意味するか?

## Statement (formal / empirical)

n=25 ≤ N_primes ≤ 1229 (p_max ∈ {100, 500, 1000, 5000, 10000}) の範囲で:

```
Andrica gap graph G_A:
  nodes = {p_1, p_2, ..., p_k}  (consecutive primes up to p_max)
  edges = (p_i, p_{i+1}) with weight √p_{i+1} − √p_i

Flow parameter: dt = 0.05, maxSteps = 20, epsilonFloor = 1e-6
Curvature:     defaultOllivierRicci (hash-based positional)

Empirical:
  per-edge singularity ratio (Andrica) = 1.25 ± 0.2
  mean κ initial                       = -20 〜 -120   (strongly negative!)
  per-edge singularity ratio (Collatz) = 0.37
  per-edge singularity ratio (Goldbach)= 0.03
```

## Background

- **STEP 681** で Rei は Ollivier-Ricci **静的測定** を実装. Collatz/Goldbach/Riemann/Twin/Mersenne 上で κ を測定し, **Goldbach ≈ Collatz 99.01%** 類似を発見 (Category A).
- **STEP 686** で Millennium 4 カテゴリ分類 (A=Collatz/Goldbach, B=Riemann, C=Hodge, D=BSD).
- **STEP 846** で Rei は静的測定を **動的 Ricci flow** に拡張.
- **STEP 847 (本 problem の source)** で Goldbach partition + Andrica gap に適用し, Andrica の異常な不安定性を発見.

## Why it matters

1. ★ **Andrica 予想は Ricci flow レベルで Collatz より本質的に難しい可能性** ★ を示唆
2. 既存の「Ricci 静的 κ ≈ 0.64」という類似性 (Category A) は動的には崩れる — **静的類似と動的類似は異なる**
3. Andrica の peer mathematicians による「Andrica は本質的に easy」との評価と矛盾する signal

## What is verified (empirical evidence, STEP 847)

| p_max  | primes | gaps | collapse | divergence | per-edge | mean κ |
|--------|--------|------|----------|------------|----------|--------|
| 100    | 25     | 24   | 0        | 40         | 1.667    | -119.5 |
| 500    | 95     | 94   | 0        | 101        | 1.075    | -46.6  |
| 1000   | 168    | 167  | 0        | 189        | 1.132    | -37.8  |
| 5000   | 669    | 668  | 0        | 818        | 1.225    | -24.2  |
| 10000  | 1229   | 1228 | 0        | 1420       | 1.156    | -19.2  |

**観察**: collapse = 0 (weight が 0 に落ちない) だが divergence が爆発的. mean κ は p_max 増加で単調に大きく (|κ| 減少), **large primes are gentler** パターン.

## What is unproven (the open gap)

1. **Curvature dependence**: defaultOllivierRicci (hash-based) に依存する結果. 別の curvature 関数 (LLY, Bakry-Émery, Forman) で同じ 3-category が再現するか未検証.
2. **Asymptotic**: p_max → ∞ で per-edge ratio は 1.0 に収束するか発散するか未知.
3. **Meaning**: Andrica 予想自体の証明不可能性に対応するのか, それとも単なる curvature artifact か.
4. **Interaction with Wieferich primes**: Wieferich 1093, 3511 を含む gap が特異的に singularity を生むか未検証.

## Candidate attack routes

- **Route A**: Curvature 関数を Forman-Ricci または LLY に置換し, Andrica explosion が persist するか確認.
- **Route B**: Asymptotic 解析 — 素数定理 p_n ≈ n log n を用いて κ(p_n, p_{n+1}) の n → ∞ 挙動を解析的に導出.
- **Route C**: Twin primes subgraph のみ抽出して flow → 特異性が twin 部位に集中するか検証.
- **Route D**: Wieferich 素数を含む local neighborhood の flow 挙動 (Paper 102 の延長).

## Reward / Incentive

None (open research). 反例または curvature-independence 証明を歓迎.

## References

- Rei-AIOS: [STEP 847 script](https://github.com/fc0web/rei-aios/blob/main/scripts/step847-perelman-goldbach-andrica.ts)
- Data: [report JSON](https://github.com/fc0web/rei-aios/blob/main/data/step847-perelman-goldbach-andrica-report.json)
- Engine: [perelman-flow-engine.ts](https://github.com/fc0web/rei-aios/blob/main/src/axiom-os/perelman-flow-engine.ts)
- Related: [Paper 74 Andrica bound](https://doi.org/10.5281/zenodo.19543XXX)
- Andrica (1986) original conjecture.
