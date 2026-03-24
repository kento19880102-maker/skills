# SKILL.md リファクタリングガイド

SKILL.md が肥大化したとき、詳細仕様を references/ に分離してスリム化する手法。

## いつリファクタリングするか

以下のいずれかに該当する場合にリファクタリングを検討する:

- SKILL.md が **80行以上** で、詳細テーブルやテンプレートを含む
- セクション全体を **「1文の要約 + references へのリンク」で置換可能** な箇所がある
- JSON schema、output 形式定義、scoring 基準が直接埋め込まれている
- 同じスキルの references/ に既にファイルがある場合、SKILL.md 内にまだ残っている詳細があれば整合を取る

## 抽出すべきコンテンツの種類

以下の7カテゴリに該当するコンテンツは references/ への抽出候補:

| # | コンテンツ種別 | 具体例 | ファイル名例 |
|---|---------------|--------|-------------|
| 1 | 評価フレームワーク | N項目の構造分析テンプレート | `eval-framework.md` |
| 2 | 質問リスト＋回答テンプレート | YES/NO/PARTIAL の判定表 | `strict-questions.md` |
| 3 | 出力テンプレート | プロファイルテーブル、レポート形式 | `output-template.md` |
| 4 | スコアリング基準 | 加重スコア計算式、◎/○/△/× 定義 | `scoring-guide.md` |
| 5 | 判定基準テーブル | 4段階判定、GO/NO GO 基準 | `validation-criteria.md` |
| 6 | JSON スキーマ | スクリプトの入力仕様 | `xlsx-schema.md` |
| 7 | 分類・段階定義 | Tier 定義、カテゴリ一覧 | `derived-business.md` |

**判定の目安**: そのセクションが「ワークフローの手順」ではなく「手順の中で参照する詳細データ」であれば抽出候補。

## 抽出してはいけないもの

以下は SKILL.md 本体に残す:

- ワークフローのステップ順序と概要（1-2文の説明）
- 入力・出力仕様のサマリ
- 会話ルール・行動ガイドライン
- `$ARGUMENTS` 処理の指示
- スキルの目的と全体像

**原則**: SKILL.md だけ読んでもワークフロー全体が理解でき、references/ を読むと詳細が分かる状態にする。

## リファクタリング手順（5ステップ）

### Step 1: 現状把握

SKILL.md 全体を読み、セクションごとの行数を数える。80行未満なら通常リファクタリング不要。

### Step 2: 抽出候補の特定

各セクションを7カテゴリ判定表と照合し、抽出候補をリストアップする。

### Step 3: reference ファイルを作成

抽出候補ごとに references/ にファイルを作成する。

**ファイル構造**:
```markdown
# タイトル（内容を端的に表す名前）

1行の説明（このファイルが親スキルのどこで使われるか）。

## セクション1
（テーブル、テンプレート、基準定義等）

## セクション2
...
```

### Step 4: SKILL.md を書き換え

詳細セクションを参照リンクで置換する。

**Before**:
```markdown
## Part 1: 構造分析（10項目）

### 1. 具体的なビジネスモデル
- 収益構造（フロー型 / ストック型 / ハイブリッド）
- 価格設定の考え方と根拠
- ターゲット顧客の具体像

### 2. 市場分析
...（50行以上の詳細が続く）
```

**After**:
```markdown
## Part 1: 構造分析（10項目）

[eval-framework.md](references/eval-framework.md) の10項目テンプレートに沿って分析する。
```

### Step 5: 「詳細仕様ドキュメント」テーブルを追加

SKILL.md の末尾に、全 reference ファイルの一覧テーブルを追加する:

```markdown
## 詳細仕様ドキュメント

| 仕様 | ファイル |
|------|---------|
| 構造分析10項目 | [references/eval-framework.md](references/eval-framework.md) |
| 7つの厳格質問 | [references/strict-questions.md](references/strict-questions.md) |
| 派生ビジネス分析 | [references/derived-business.md](references/derived-business.md) |
```

「仕様」列には内容の機能名（ファイル名ではない）を書く。

## reference ファイルの命名規則

- **小文字英字 + ハイフン区切り** (`scoring-guide.md`, NOT `ScoringGuide.md`)
- **内容の目的で命名** する（SKILL.md のセクション名ではない）
- **汎用的な名前は避ける** (`details.md`, `appendix.md` は不可)
- 100行以上になる場合は **冒頭に目次** を付ける

## チェックリスト（リファクタリング完了後）

- [ ] SKILL.md だけ読んでワークフロー全体が理解できる
- [ ] 「詳細仕様ドキュメント」テーブルに全 reference ファイルが列挙されている
- [ ] 全リンクが相対パス (`references/filename.md`) で正しい
- [ ] SKILL.md と reference ファイルで情報が重複していない
- [ ] reference ファイルは1階層のみ（`references/sub/file.md` は不可）
- [ ] reference ファイル同士の相互参照は可（ただし SKILL.md への逆参照は不可）
- [ ] SKILL.md が 500行以下である
