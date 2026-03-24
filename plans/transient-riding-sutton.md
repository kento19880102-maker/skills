# 副業アイデア 親子スキル アーキテクチャ

## Context

既存の単一スキル `side-biz-ideas` を、5つの子スキル＋1つの親スキルに分割する。各子スキルは独立して使用可能であり、親スキルが一気通貫で全ステップを順番に実行する。これにより、各ステップの品質向上と再利用性を実現する。

## アーキテクチャ

```
User → [side-biz-planner] (親スキル)
           │
           ├─1→ [side-biz-hearing]     → ユーザープロファイル
           │
           ├─2→ [side-biz-idea-gen]    → 3-5個のアイデア（構造化分析付き）
           │        reads: references/idea-categories.md
           │
           ├─3→ [side-biz-evaluator]   → 10項目の深掘り評価
           │
           ├─4→ [side-biz-compare]     → 比較マトリクス＋推薦
           │        ↓ ユーザーにアイデア選択を確認
           └─5→ [side-biz-actionplan]  → 90日間行動計画
```

## ディレクトリ構成

```
.claude/skills/
├── side-biz-planner/           # 親スキル（オーケストレーター）
│   └── SKILL.md
├── side-biz-hearing/           # 子1: ヒアリング
│   └── SKILL.md
├── side-biz-idea-gen/          # 子2: アイデア生成
│   ├── SKILL.md
│   └── references/
│       └── idea-categories.md  # 既存ファイルを移動
├── side-biz-evaluator/         # 子3: アイデア評価
│   └── SKILL.md
├── side-biz-compare/           # 子4: 比較分析
│   └── SKILL.md
└── side-biz-actionplan/        # 子5: アクションプラン
    └── SKILL.md
```

## バリデーター制約

`quick_validate.py` が許可するフロントマター: `name`, `description`, `license`, `allowed-tools`, `metadata` のみ。
`disable-model-invocation`, `user-invocable`, `context` は使用不可。
→ 子スキルのdescriptionを狭いキーワードにし、親スキルに広いキーワードを持たせて棲み分ける。

## 実装手順

### Step 1: 既存スキルの削除

`side-biz-ideas/` を削除（`idea-categories.md` は `side-biz-idea-gen` に移動するため内容を保持）。
`side-biz-ideas.skill` パッケージファイルも削除。

### Step 2: 6つのスキルを init_skill.py で初期化

```bash
for skill in side-biz-hearing side-biz-idea-gen side-biz-evaluator side-biz-compare side-biz-actionplan side-biz-planner; do
  python3 .claude/skills/skill-creator/scripts/init_skill.py $skill --path .claude/skills
done
# side-biz-idea-gen のみ references ディレクトリ追加
mkdir -p .claude/skills/side-biz-idea-gen/references
```

### Step 3: 各 SKILL.md を作成

#### 子1: side-biz-hearing
- ヒアリング5項目（スキル・興味・時間・予算・リスク）
- 出力フォーマット: プロファイル表
- $ARGUMENTS 対応（親から情報が渡された場合は重複質問を避ける）
- トリガー: 「副業ヒアリング」「状況を整理したい」等の狭いキーワード

#### 子2: side-biz-idea-gen
- 8カテゴリから横断的に3〜5個のアイデア提案
- 各アイデアに10項目の分析フレームワーク
- `references/idea-categories.md` 参照（既存内容を移動）
- トリガー: 「アイデアを出して」「何を始めればいい？」等

#### 子3: side-biz-evaluator
- 10項目の深掘り分析（ビジネスモデル、市場分析、収益シミュレーション、リスク分析等）
- トリガー: 「評価して」「深掘り」「実現可能性」等

#### 子4: side-biz-compare
- 比較マトリクス表の作成
- 優先事項に基づくランキング（即金性/高収入/低リスク/スケーラビリティ/最小時間）
- 組み合わせ戦略の提案
- トリガー: 「比較して」「どっちがいい？」「ランキング」等

#### 子5: side-biz-actionplan
- 90日間ロードマップ（4フェーズ: 準備→MVP→初収益→拡大）
- 必要ツール・プラットフォーム一覧
- KPI設定、法務・税務チェックリスト
- トリガー: 「アクションプラン」「行動計画」「始め方」等

#### 親: side-biz-planner
- `allowed-tools: Skill(side-biz-hearing), Skill(side-biz-idea-gen), Skill(side-biz-evaluator), Skill(side-biz-compare), Skill(side-biz-actionplan)`
- 5ステップの順次実行ワークフロー
- 各ステップ間でユーザー確認
- Step 4後にアイデア選択を促し、Step 2への戻りも可能
- トリガー: 「副業」「サイドビジネス」「副収入」「稼ぎたい」等の広いキーワード

### Step 4: references/idea-categories.md を配置

既存の `side-biz-ideas/references/idea-categories.md` の内容を `side-biz-idea-gen/references/idea-categories.md` にコピー。

### Step 5: 全スキルのバリデーション

```bash
for skill in side-biz-hearing side-biz-idea-gen side-biz-evaluator side-biz-compare side-biz-actionplan side-biz-planner; do
  python3 .claude/skills/skill-creator/scripts/quick_validate.py .claude/skills/$skill
done
```

### Step 6: パッケージング

```bash
for skill in side-biz-hearing side-biz-idea-gen side-biz-evaluator side-biz-compare side-biz-actionplan side-biz-planner; do
  python3 .claude/skills/skill-creator/scripts/package_skill.py .claude/skills/$skill
done
```

## 対象ファイル

| ファイル | 操作 |
|---------|------|
| `.claude/skills/side-biz-ideas/` | 削除 |
| `side-biz-ideas.skill` | 削除 |
| `.claude/skills/side-biz-planner/SKILL.md` | 新規作成 |
| `.claude/skills/side-biz-hearing/SKILL.md` | 新規作成 |
| `.claude/skills/side-biz-idea-gen/SKILL.md` | 新規作成 |
| `.claude/skills/side-biz-idea-gen/references/idea-categories.md` | 移動（既存内容） |
| `.claude/skills/side-biz-evaluator/SKILL.md` | 新規作成 |
| `.claude/skills/side-biz-compare/SKILL.md` | 新規作成 |
| `.claude/skills/side-biz-actionplan/SKILL.md` | 新規作成 |

## 検証方法

1. 全6スキルが `quick_validate.py` を通過すること
2. 全6スキルが `package_skill.py` でパッケージングできること
3. テストクエリ:
   - 親スキル: 「副業を始めたい」→ side-biz-planner が起動し、5ステップを順次実行
   - 子スキル単体: 「このアイデアを評価して」→ side-biz-evaluator が単体で起動
   - 子スキル単体: 「フリーランスとSaaS、比較して」→ side-biz-compare が単体で起動
