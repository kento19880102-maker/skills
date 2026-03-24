---
name: side-biz-evaluator
description: "副業アイデア評価スキル。副業アイデアを10項目の構造分析＋7つの厳格質問＋派生ビジネス分析で深掘り評価する。「このアイデアを評価して」「詳しく分析して」「実現可能性は？」「深掘りして」「もっと詳しく」といったキーワードで起動する。Use when the user wants a detailed evaluation of a specific side business idea."
---

# 副業アイデア評価

特定の副業アイデアについて、3パートの構造化フレームワークで深掘り分析を行う。

## 入力

評価対象のアイデア（ユーザーの指定または $ARGUMENTS で受け取る）。

## Part 1: 構造分析（10項目）

[eval-framework.md](references/eval-framework.md) の10項目テンプレートに沿って、各アイデアを分析する。

## Part 2: 7つの厳格質問

[strict-questions.md](references/strict-questions.md) の質問リストに従い、容赦なく現実チェックを行う。各問に YES/NO/PARTIAL で直接回答し、根拠を1-2文で添える。NOの場合は改善案も示す。

## Part 3: 派生ビジネス分析

このアイデアを実行することで獲得するスキルと、そこから派生する新しいビジネスを分析する。詳細フレームワークは [derived-business.md](references/derived-business.md) を参照。

### スキル獲得テーブル

| # | スキル | 主にどの活動から | 習得期間 | Tier 1派生 | Tier 2派生 |
|---|--------|------------------|----------|------------|------------|
| 1 | （分析結果） | | | | |

### 派生ビジネスロードマップ

- **Tier 1（6-12ヶ月後）**: 自然な延長線上のビジネス
- **Tier 2（12-18ヶ月後）**: スキル深化後に展開可能なビジネス
- **Tier 3（18ヶ月以降）**: プラットフォーム化・大規模展開

## 出力形式

各アイデアについてPart 1-3を見出し付きで構造的に記述する。スコアリング（★1-5）も付与する。

## 詳細仕様ドキュメント

| 仕様 | ファイル |
|------|---------|
| 構造分析10項目 | [references/eval-framework.md](references/eval-framework.md) |
| 7つの厳格質問 | [references/strict-questions.md](references/strict-questions.md) |
| 派生ビジネス分析 | [references/derived-business.md](references/derived-business.md) |
