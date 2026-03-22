# 副業アイデア スキル作成プラン

## Context

ユーザーが副業のアイデアをブレインストーミング・評価・比較できるスキルを作成する。ユーザーの状況（スキル・可処分時間・予算・リスク許容度）に基づきパーソナライズされた提案を行うフレームワークを提供する。

## スキル構成

```
.claude/skills/side-biz-ideas/
├── SKILL.md                      # メイン指示（必須）
└── references/
    └── idea-categories.md        # カテゴリ別の詳細情報
```

- **scripts/ は不要**: 計算処理やファイル変換がなく、すべてClaude の推論で対応可能
- **assets/ は不要**: テキストベースの出力のみ

## 実装手順

### Step 1: init_skill.py でスキルを初期化

```bash
python .claude/skills/skill-creator/scripts/init_skill.py side-biz-ideas \
  --path .claude/skills \
  --resources references
```

### Step 2: SKILL.md を作成

**Frontmatter:**
- `name`: `side-biz-ideas`
- `description`: 日本語トリガーワード（「副業」「サイドビジネス」「副収入」「稼ぎたい」等）＋英語トリガーを含む包括的な説明

**Body の構成:**

1. **ヒアリング** — 5項目（スキル・興味・可処分時間・初期予算・リスク許容度）を自然な会話で確認
2. **アイデア提案** — 3〜5個のアイデアに構造化分析を付与（概要、マッチ度★、市場ポテンシャル、初期投資、初収益までの期間、月収レンジ、スケーラビリティ、必要時間、リスク、最初の一歩）
3. **比較と絞り込み** — 比較マトリクス表 + 深掘り分析（ビジネスモデル、開始手順、ツール、差別化、失敗パターン）
4. **注意事項** — 法的助言の免責、断定的表現の回避、投資助言の制限、就業規則確認の促し

**カテゴリ（8種）:**
- デジタルプロダクト / フリーランス・受託 / コンテンツ制作 / EC・物販 / コンサルティング / SaaS・アプリ開発 / 教育・スキルシェア / 投資・資産運用

### Step 3: references/idea-categories.md を作成

各カテゴリについて以下をまとめる（200-300行程度）:
- ビジネスモデルの特徴・収益構造
- 日本での主要プラットフォーム（CrowdWorks、Lancers、ココナラ、メルカリ、BASE、note、Udemy Japan 等）
- 現実的な収益レンジ
- 必要なスキルレベル（初級/中級/上級）
- 始めるためのコスト
- 日本固有の考慮事項（確定申告20万円ルール、開業届、インボイス制度等）

### Step 4: バリデーション

```bash
python .claude/skills/skill-creator/scripts/quick_validate.py .claude/skills/side-biz-ideas
```

### Step 5: パッケージング

```bash
python .claude/skills/skill-creator/scripts/package_skill.py .claude/skills/side-biz-ideas
```

## 対象ファイル

| ファイル | 操作 |
|---------|------|
| `.claude/skills/side-biz-ideas/SKILL.md` | 新規作成 |
| `.claude/skills/side-biz-ideas/references/idea-categories.md` | 新規作成 |

## 検証方法

1. `quick_validate.py` でスキル構造を検証
2. `package_skill.py` でパッケージングが成功することを確認
3. 以下のテストクエリで動作確認:
   - 「プログラマーで週10時間使えます。予算5万円で副業始めたい」
   - 「デザインが得意でリスク低めの副業を探しています」
   - 「SaaSとフリーランス、どっちが良い？」
