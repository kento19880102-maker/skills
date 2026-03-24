---
name: side-biz-validator
description: "副業アイデア競合検証スキル。アイデアごとにWeb検索で実在する競合を調査し、ブルーオーシャン/レッドオーシャン判定と4条件スコアを付与する。「競合はいる？」「本当にやっている人はいない？」「レッドオーシャン？」「ブルーオーシャン？」「競合調査して」「実際に調べて」「バリデーション」「検証して」といったキーワードで起動する。Use when the user wants to validate a side business idea against real-world competitors via web search."
---

# 副業アイデア競合検証

アイデアごとにWeb検索で実在する競合を調査し、ブルー/レッドオーシャン判定を行う。

**大原則: ユーザーに優しい嘘をつくより、今ここで正直に伝える方がはるかに親切。**

## 入力

検証対象のアイデア（1つ以上）。$ARGUMENTS で親スキルから渡された場合はそれを使用する。ユーザーのプロファイル（ユニーク特性）があれば差別化の評価に活用する。

## 検証プロセス（アイデアごとに実行）

### Step 1: 多角的Web検索

[validation-criteria.md](references/validation-criteria.md) の「検索角度」セクションを参照し、最低3角度でWebSearchを実行する。

**競合が見つからなかった場合**: 「見つからなかった」と明記する。ただし「見つからなかった＝存在しない」ではない点も注記する。

### Step 2: 競合分析テーブル

発見した競合ごとに [validation-criteria.md](references/validation-criteria.md) の「競合分析テーブルテンプレート」に沿って記録する。

### Step 3: オーシャン判定

[validation-criteria.md](references/validation-criteria.md) の「オーシャン判定（4段階）」基準に従い判定する。

### Step 4: 4条件クイックスコア

[validation-criteria.md](references/validation-criteria.md) の「4条件クイックスコア」表に沿って ◎/○/△/× を付与する。

### Step 5: GO / NO GO 判定

[validation-criteria.md](references/validation-criteria.md) の「GO / NO GO 判定基準」に従い判定する。NO GOの場合、ピボットの方向性を1-2行で提案する。

## 出力フォーマット（アイデアごとに）

```
### [アイデア名]

**競合調査結果**:
（競合分析テーブル）

**オーシャン判定**: [Red/Orange/Yellow/Blue] Ocean
**理由**: （1-2文）

**4条件スコア**:
| 希少性 | ニーズ | 潜在性 | 参入障壁 |
|--------|--------|--------|----------|
| ○     | ◎     | ○     | △       |

**判定**: [GO / CONDITIONAL GO / NO GO]
**補足**: （1-2文）
```

## 会話ルール

- 具体的な競合名・URLを必ず示す。「競合がいるかもしれない」のような曖昧表現は禁止
- 検索で見つからなかった場合は「見つからなかった」と正直に報告する
- ユーザーが判定に異議を唱えた場合、ユーザーの新しい視点で追加検索を行い、再判定する
- 1ラウンドで全アイデアがNO GOだった場合、「全滅」と正直に伝え、方向転換を提案する

## 詳細仕様ドキュメント

| 仕様 | ファイル |
|------|---------|
| オーシャン判定・4条件スコア・GO/NO GO基準 | [references/validation-criteria.md](references/validation-criteria.md) |
