# Problem 015 — Why Do 26 of 163 Non-Atomic Peak-9232 Orbits Miss Fiber-57 Gateway?

- **ID**: 015
- **Category**: Collatz-equivalent / Structural
- **Source**: Rei-AIOS STEP 869 (2026-04-17) — Atomicity identifier search
- **Status**: open, structural
- **Discovered**: 2026-04-18
- **Last updated**: 2026-04-18

## Statement (informal)

STEP 869 で以下が判明:
- 25 atomic cores **全て**が fiber-57 gateway (= {121, 377} 通過) を使う
- 163 非-atomic peak-9232 orbits のうち **137 が gateway を通過** (84.0%)
- **残り 26 非-atomic peak-9232 orbits は fiber-57 を通らずに peak 9232 に到達**

問い: この **26 orbits は何か構造的 / algebraic に特徴付けられるか**? 短い orbit / 特定の odd start / 別 route で peak に到達 — どのような性質か?

## Statement (formal)

```
Let P9232 = {n ∈ [3, 1000) odd : peak(n) = 9232}.        |P9232| = 188
Let G = {n ∈ P9232 : ∃ k, Collatz^k(n) ∈ {121, 377}}.    |G| = 162
Let P9232 \ G = {n ∈ P9232 : Collatz orbit misses both 121 and 377}.
                                                         |P9232 \ G| = 26

Let A₂₅ = 25 atomic cores.   A₂₅ ⊂ G.

Question: Find a structural characterization C of P9232 \ G.
 I.e., C : ℕ → Bool with:
   C(n) = true ⟺ n ∈ P9232 \ G.
```

## Background

Chang (arXiv:2603.25753) が fiber-57 を Syracuse-confinement の中核として解析。Rei STEP 697 で Chang 補題を integrate し、25 atomic cores の atomicity と fiber-57 を結びつけた。しかし STEP 869 の 7-discriminator 系統検証で、fiber-57 gateway は:

- atomic cores に対して **universally present** (25/25)
- 非-atomic peak-9232 orbits に対して **大部分 present** (137/163)
- **26 non-atomic で absent** — この 26 が structural anomaly

仮説 (現状未検証):
- **仮説 α**: 26 orbits は短い (K(n) が小) でそもそも長い trajectory を走らない
- **仮説 β**: 26 orbits は別の fiber (e.g., mod 128 or mod 256 の別 residue class) を通る
- **仮説 γ**: 26 は特定の Collatz tree 構造上の "bypass route" を使う

## Why it matters

- fiber-57 を「peak 9232 の唯一の経路」と主張していた前提の反例発見
- Chang fiber-57 の universality 評価に結果を feedback
- 26 orbits の性質が分かれば、"peak = 9232 but non-atomic" の structural 説明に直結
- Problem 011 (sufficient structural identifier) の attack route の一つ

## What is verified

| 観点 | 状態 |
|------|------|
| |P9232 \ G| = 26 on [3, 1000) | ✓ STEP 869 |
| A₂₅ ⊂ G (全 atomic core が gateway 経由) | ✓ STEP 869 |
| **26 orbits の具体リスト** | ❌ 未抽出 (必要なら 1 秒で抽出可) |
| **K(n) distribution for 26** | ❌ 未測定 |
| **26 の mod residue pattern** | ❌ 未分析 |
| 仮説 α / β / γ | ❌ 全 open |

## Candidate attack routes

- **Route A** (brute force enumerate): 26 orbits を抽出 → K, bitLen, mod 96, mod 192, peak 到達 step を tabulate
- **Route B** (tree structure): Collatz tree で 26 が peak 9232 に到達するまでの common ancestor path を推定
- **Route C** (alternative fiber): Chang 式に倣い、mod 2^k で 26 の特徴 fiber を探す (k = 6, 7, 8, 9, 10 で test)
- **Route D** (large n): 同現象が [3, 10⁶] で継続するか。26 が 500 や 5000 に scale するか?

## Related Rei-AIOS resources

- `test/step869-atomicity-identifier-test.ts` — 7-discriminator 実装 + (B) fiber-57 結果
- `src/axiom-os/collatz-atomic-cores-engine.ts` — 25 atomic cores canonical
- Chang (arXiv:2603.25753) fiber-57 paper (Research Radar monitored)
- STEP 697 Chang-Mathlib interop

## Call for contribution

本問題は **computational** で解析が容易 (数分). 26 orbits のリスト化 + characterization だけで十分な貢献。structure が見つかれば Problem 011 の attack route に直結する可能性。
