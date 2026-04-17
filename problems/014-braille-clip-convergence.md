# Problem 014 — Does Rule-Based Braille-D-FUMT₈ Converge to CLIP Performance as Rule Set Grows?

- **ID**: 014
- **Category**: Empirical / ML benchmarking
- **Source**: Rei-AIOS Paper 110 §7 M1 (2026-04-17) / STEP 845 FIDT
- **Status**: open, empirical
- **Discovered**: 2026-04-18
- **Last updated**: 2026-04-18

## Statement (informal)

Paper 110 M1 skeleton で、Rule-based Braille-D-FUMT₈ classifier は 15-excerpt philosophy dataset で **80% exact match / 100% within Hamming-1** を達成した (KEYWORD_RULES: 約 25 entries)。

問い: rule set を **N entries → ∞** に拡張した場合、classifier の top-1 accuracy は **CLIP-NN baseline に漸近収束**するか? それとも構造的 ceiling があるか?

## Statement (formal)

```
Let D be a labeled dataset of (text, DFumt₈ subset) pairs.
Let R_N be a rule set of N (keyword, DFumt₈ values) entries.
Let f_R(text) be the rule-based classifier: OR over matching rules.
Let f_CLIP(text) be the CLIP-NN baseline (k=1 on D).

Accuracy(f, D) = |{(x, y) ∈ D : f(x) = y}| / |D|

Conjecture (A): lim_{N→∞} Accuracy(f_{R_N}, D) = Accuracy(f_CLIP, D).
Conjecture (B): ∃ D such that sup_N Accuracy(f_{R_N}, D) < Accuracy(f_CLIP, D).

It is open which conjecture holds, and under what conditions on D.
```

## Background

Paper 110 では **相補性** の立場を取り、Braille は "定義的 / 決定的 / 解釈可能" axis で勝ち、CLIP は "情報密度 / empirical" axis で勝つとした。しかし 14-excerpt skeleton では Braille が 80% に達しており、**rule を増やすだけでさらに上がる可能性**がある。

この問いは ML vs 記号処理の **古典的議論** (GOFAI vs deep learning) の縮小版。本 dataset (philosophy tagging) は:
- 語彙が有限 (哲学概念は有限集合)
- D-FUMT₈ label は 256 値の bitmask (有限)
- 明示的 keyword mapping で大部分 covered

→ **rule-based は十分に large な N で optimal** の hypothesis は妥当。

## Why it matters

- 結果 (A) true の場合: philosophy domain 特有の semantic closure を示す (小さな語彙 + 明示的論理構造)
- 結果 (B) true の場合: **ambiguity / novelty / context dependence** が rule-based の構造的上限を作る
- 実用的: Rei-AIOS の SEED_KERNEL 理論 tagging / 索引生成への影響

## What is verified

| 観点 | 状態 |
|------|------|
| 15-excerpt dataset で 80% exact match @ N=25 rules | ✓ Paper 110 M1 skeleton |
| 100% within Hamming-1 @ N=25 rules | ✓ Paper 110 M1 skeleton |
| **N → 50 / 100 での accuracy scaling** | ❌ 未実施 |
| **CLIP ViT-B/32 baseline 測定** | ❌ 未実施 (model download 必要) |
| **大規模 dataset (e.g., 200 excerpts)** | ❌ 未実施 |

## Candidate attack routes

- **Route A** (empirical scaling): dataset を 100 以上 excerpt に拡張、rule を 100 entries 程度まで増加させて accuracy(N) を plot. 典型的な ML learning curve と比較.
- **Route B** (CLIP actual run): `pip install open_clip_torch` + ViT-B/32 (500MB). Japanese + English philosophy text embedding を取得、k-NN baseline 測定.
- **Route C** (theoretical bound): fixed domain (finite vocabulary, finite label) での rule-based limit を combinatorial に特定.
- **Route D** (hybrid): Braille mask ⊕ CLIP 512-d concat vector で Weighted-KNN がどちらよりも improve するか検証 (Paper 110 §7 M2).

## Related Rei-AIOS resources

- `test/paper110-m1-braille-vs-clip-skeleton.ts` — 現状 skeleton (15 excerpts, 25 rules)
- `papers/paper-110-braille-dfumt8-vs-embeddings-rigorous-comparison.md` — 5-axis 比較
- `docs/infinite-dot-theory-positioning-2026-04-17.md` — positioning

## Known references

- Mikolov et al. (2013) — word2vec scaling properties
- Radford et al. (2021) — CLIP
- Mac Lane (1945) — categorical foundations of semantics
- Paper 33 (Fujimoto 2026) — Braille-D-FUMT₈ base

## Call for contribution

CLIP ViT-B/32 を実 run して Braille-rule baseline との accuracy 差を 200+ excerpt scale で測定する contribution を歓迎。Paper 110 §7 M1 benchmark completion に直結。
