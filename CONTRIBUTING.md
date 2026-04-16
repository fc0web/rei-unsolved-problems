# Contributing

外部からの指摘・反例・証明試行を歓迎します.

## 提出可能なもの

1. **反例 (counterexample)** — 具体的な n / graph / Lean 4 term で問題を破る入力
2. **部分証明 (partial proof)** — 条件付きで問題の一部を解く手筋
3. **完全証明 (complete proof)** — Lean 4 で zero-sorry な proof
4. **類似問題への接続 (cross-reference)** — 既知の予想との同値性/含意の発見
5. **形式化補助 (formalization)** — 既存 markdown 問題を Lean 4 化する PR

## 手順

```
1. Fork this repository
2. Create a new branch: fix/problem-XXX-YYY-summary
3. Add your contribution:
   - For counterexample: problems/XXX-*.md 内に "Counterexamples" section を追加
   - For Lean 4 proof: lean4/XXX_*.lean を新規作成
4. Submit PR — 対応する Rei-AIOS STEP への reference を含めると審査が早い
```

## 品質基準

- **Honest framing** — "証明した" claim は peer review 完了後のみ許可
- **Reproducible** — 計算は script or Lean term で示す
- **Citable** — 既存論文への参照を含める

## 連絡先

- GitHub Issues: 推奨 (discussion が permanent trail になる)
- Email: fc2webb@gmail.com (重要指摘のみ)

## 禁止事項

- 既知未解決問題 (Riemann, P vs NP 等) の「解決した」claim の投稿 — 該当 repo (arXiv 等) へ
- 本 repo の問題の「完全解」を主張する PR は Lean 4 検証付き必須
