# Problem 012 — Why Does Peak 9232 Dominate 37.7% of Small Odd Collatz Orbits?

- **ID**: 012
- **Category**: Collatz-equivalent / Empirical structural
- **Source**: Rei-AIOS STEP 867 (2026-04-17) / STEP 696 peak-9232 discovery
- **Status**: open, empirical
- **Discovered**: 2026-04-18
- **Last updated**: 2026-04-18

## Statement (informal)

Collatz orbit の **global maximum** を `peak(n)` と呼ぶ。[3, 1000) の odd n のうち、**37.7% (188/499) が peak(n) = 9232 = 2⁴ · 577** を共有する。Peak 9232 が **このような高頻度で現れる構造的理由は何か?**

観測事実:
- 9232 = 16 × 577, 577 は素数
- 9232 は v₂ = 4 (4 連続 /2 で 577 に降りる)
- 9232 は popcount = 3 (疎な 2-adic 展開)
- 9232 は Collatz 下降経路 9232 → 4616 → 2308 → 1154 → 577 → 1732 → 866 → 433 → ...

問い: **なぜ 9232 か?** 他の n₀ = 2^a · p (p 素数) は同様の universal peak にならないのか?

## Statement (formal)

```
Define peak : ℕ → ℕ, peak(n) = max {T^k(n) : k ≥ 0, T^k(n) ≠ 1}
where T(n) = n/2 (n even) or 3n+1 (n odd) is the Collatz map.

Empirical: |{n ∈ [3, 1000) odd : peak(n) = 9232}| = 188.
Let U_N = |{n ∈ [3, N) odd : peak(n) = 9232}| / (N/2).

Conjecture (open): lim_{N→∞} U_N = u_∞ ∈ (0, 1)
  i.e., peak = 9232 は a positive fraction の odd n で common peak となる.

Stronger (also open): 全 a, b ≥ 1 に対し、定数 c(a, b) > 0 が存在し、
  peak = 2^a · p を持つ odd n の密度が c(a, b) に収束する.
```

## Background

STEP 696 で 25 atomic cores が発見された際、peak = 9232 は n = 27, 55, 91, 95, 143, 221 の 6 件のみで共有と記録されていた (**memory 訂正**: STEP 867 で実測した結果 25/25 全て + 合計 188/499 が共有)。

- 9232 は n=27 の classic Collatz trajectory high-water mark (Lagarias 2010 にも言及)
- Janik (arXiv:2603.11066v5) の syracuse-confinement とも関連
- Chang (arXiv:2603.25753) の fiber-57 構造で、peak 9232 が fiber を通る gateway として機能

## Why it matters

- **Collatz orbit tree** の構造に関する fundamental question
- 「なぜ 9232 か」への答えは **peak の universality class** を特定する
- Tao 2019 の logarithmic density 結果への補完: density bound は "almost every n" だが、**peak value 側の concentration** は別問題
- **Collatz orbit graph** が local/global 同じ peak に収束することを示すなら、それは trajectory tree の **strongly connected structural skeleton** の存在を意味する

## What is verified

| 観点 | 状態 |
|------|------|
| peak = 9232 for 188/499 odd n in [3, 1000) | ✓ STEP 867 |
| peak = 9232 for 25/25 atomic cores | ✓ STEP 867 |
| v₂(9232) = 4 | ✓ STEP 867 |
| popcount(9232) = 3 | ✓ STEP 868 |
| peak = 9232 density @ N = 10⁴ | ? 未測定 |
| peak = 9232 density @ N = 10⁶ | ? 未測定 |
| **limit density u_∞ existence** | ❌ open |
| **"なぜ 9232 か"** structural reason | ❌ open |

## Candidate attack routes

- **Route A** (empirical extrapolation): N = 10⁴, 10⁵, 10⁶, 10⁹ で density 測定 → 収束 / 発散 / 不確定 を判定
- **Route B** (orbit tree structure): 9232 への in-tree = Collatz trajectory で 9232 に到達する odd n の集合。この tree の graph-theoretic structure (branching factor, depth, ...) を解析
- **Route C** (Lagarias inverse orbit): 9232 の inverse Collatz orbit (逆方向). 9232 は **3n+1 = 9232 ↔ n = 3077** のみ不即 (T^(-1) は multi-valued). Inverse BFS で 9232 から上流展開
- **Route D** (577 prime 役割): 577 は 24² + 1 = 577 の特別な形. 他の n²+1 prime (2, 5, 17, 37, 101, 197, 257, 401, 577, ...) で同様の universal peak 現象が起きるか?

## Related Rei-AIOS resources

- `src/axiom-os/collatz-peak-9232-engine.ts` — peak-9232 detection + analysis
- `test/step867-descent-basin-hypothesis-test.ts` — Phase 4 が 188/499 を初測定
- STEP 696 memo (peak=9232 は 25 atomic cores の global high-water mark)
- Lagarias (2010) "The 3x+1 problem and its generalizations"

## Known related phenomena

- Roosendaal 10⁸ record: n = 63,728,127 で k_max = 11 (全 Collatz iteration max 深度)
- Rei mod 96 saturation (STEP 694) — peak 9232 とは別 modulus
- Paper 104 (near-WSS {7,11,13,19}) — prime family 関係?

## Call for contribution

本問題は **empirical** なので、大規模計算 (N = 10⁹ or 10¹²) の結果提供 contribution を歓迎。密度 u_∞ が 0 に収束 / 有限極限 / 発散 を実験的に絞り込むだけでも大きな貢献となる。
