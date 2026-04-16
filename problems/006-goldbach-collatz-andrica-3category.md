# Problem 006 — Goldbach-Collatz-Andrica 3-Category Ricci-Flow Classification

- **ID**: 006
- **Category**: Ricci-flow class (novel)
- **Source STEP**: Rei-AIOS STEP 847 (2026-04-17)
- **Status**: open, **novel** classification hypothesis
- **Discovered**: 2026-04-17
- **Last updated**: 2026-04-17

## Statement (informal)

離散 Ollivier-Ricci flow は、3 種類の未解決問題族を以下の 3 カテゴリに分類する:

| Category | Problem | per-edge singularity | mean κ          | 挙動 |
|----------|---------|----------------------|-----------------|------|
| **S** (stable)     | Goldbach partition graph | **0.025** | +0.08 〜 +0.50 | 大半 0 singularity |
| **M** (moderate)   | Collatz orbit graph      | **0.37**  | mixed           | 中程度不安定 |
| **E** (explosive)  | Andrica gap graph        | **1.25**  | -19 〜 -119     | edge あたり複数 |

この 3-category S/M/E 分類は:

1. **普遍的か?** — 他の未解決問題 (Twin primes, Mersenne, Hodge, BSD, etc.) をこの S/M/E のどれかに分類可能か
2. **Curvature independent か?** — defaultOllivierRicci 以外 (Forman, LLY, Bakry-Émery) でも同じ S/M/E 分類が生じるか
3. **問題の難易度に対応するか?** — S が「数値的に easy」, E が「数値的に hard」という直観と整合するか

## Background

- **STEP 681 (2026-04-12)**: Rei が Ollivier-Ricci **静的 κ** 測定を 5 問で実施, Goldbach ≈ Collatz 99.01% 類似を発見.
- **STEP 686**: Category A/B/C/D の 4 分類を Millennium 4 問に対して確立 (静的 κ ベース).
- **STEP 847 (2026-04-17)**: 静的 κ から **動的 Ricci flow** への拡張. 静的類似 (Goldbach≈Collatz 99%) が動的に崩れることを発見 — Ricci flow は静的測定より厳しい分類基準.

## Why it matters

1. **静的 vs 動的曲率の不一致** は Riemannian 幾何/Perelman 理論において既知だが, 未解決問題の文脈に持ち込まれたのは Rei-AIOS 2026-04-17 が初と思われる.
2. 3-category 分類が普遍なら, 未測定の問題 (P vs NP, Navier-Stokes, Yang-Mills, Hodge, BSD, Fermat 2-gon 等) の **Ricci-flow signature** を取るだけで本質難度予測が可能になる.
3. Category E ("explosive") に入る問題は **恐らく永遠に人類が解けない** という予想が立つ (Andrica が E なら Paul Erdős の "A(n)·log p < 1" の直観は単純すぎた可能性).

## What is verified (STEP 847)

- 3 問で異なる per-edge singularity ratio を観測 (0.03, 0.37, 1.25)
- Goldbach の mean κ は正 (Category A 整合, STEP 681 の静的値と同じ)
- Andrica の mean κ は強烈に負 (静的 κ では中立付近だったが, flow 下では爆発)
- 両極の σ log: log(1.25 / 0.025) ≈ 3.9 (50 倍強差)

## What is unproven

1. Curvature independence (別の κ 定義で同結果?)
2. Other-problem placement (Twin primes / Hodge / BSD / NS はどこに属するか?)
3. Asymptotic (p_max, n_max → ∞ で ratio の極限?)
4. Connection to PDE instability / strange attractors

## Candidate attack routes

- **Route A**: 10 問への拡大実験 (Twin primes / Mersenne / Fermat / Catalan / ...)
- **Route B**: Curvature 4 変種 (Ollivier default, Ollivier LLY, Forman, Bakry-Émery) で同じ 3 問を flow → 3-category 再現性テスト
- **Route C**: Collatz n=27 以外 (n=703 class の 10 兄弟 STEP 679) で Collatz-内 heterogeneity 測定
- **Route D**: mathematical invariance — flow 下で保存される不変量 (Perelman entropy の類似) を構成

## References

- Rei-AIOS STEP 847: [report JSON](https://github.com/fc0web/rei-aios/blob/main/data/step847-perelman-goldbach-andrica-report.json)
- Related: STEP 681 (static Ricci), STEP 686 (Category A/B/C/D)
- Perelman 2002 *Ricci flow with surgery on three-manifolds*
- Ollivier 2009 *Ricci curvature of Markov chains on metric spaces*
