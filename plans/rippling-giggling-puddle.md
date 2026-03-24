# 副業スキル群の改善・自動化プラン

## Context
6ラウンドの副業探索ワークフローを経て、既存スキル群の重大なギャップが判明。
実際に効果的だったワークフロー（深いヒアリング→4条件フィルタ→競合検証→7質問→反復ループ→XLSX出力）を
次回以降自動で再現できるようにスキル化する。

## 変更全体像

```
[side-biz-planner] (反復ループ対応の7ステップに改修)
  ├─1─ [side-biz-hearing]      ← 改修: 5項目→10項目+ユニーク特性分析
  ├─2─ [side-biz-idea-gen]     ← 改修: 3生成法+4条件プレフィルタ
  │      └─ references/generation-methods.md (新規)
  ├─3─ [side-biz-validator]    ← ★新規: 競合Web検索+ブルー/レッドオーシャン判定
  ├─4─ [side-biz-evaluator]    ← 改修: 既存10項目+7厳格質問+派生ビジネス
  │      └─ references/derived-business.md (新規)
  ├─5─ [side-biz-compare]      ← 改修: 4条件加重スコア追加
  ├─ (←反復ループ: NO GO→Step 2に戻る)
  └─6─ [side-biz-actionplan]   ← 改修: XLSX出力+派生ロードマップ
         └─ scripts/generate_xlsx.py (新規)
```

---

## 新規作成ファイル (4件)

### 1. `.claude/skills/side-biz-validator/SKILL.md` (★最重要)
- **役割**: アイデアごとにWeb検索で実在する競合を調査し、ブルー/レッドオーシャン判定
- **allowed-tools**: WebSearch, WebFetch
- **構成**:
  - 検索戦略: アイデアごとに最低3角度で検索（直接/競合名/英語圏/ニッチ交差点）
  - 競合分析テーブル: 名前+URL / 内容 / 形式 / 規模 / 重複度 / 差分
  - 4段階判定: Red Ocean / Orange Ocean / Yellow Ocean / Blue Ocean
  - 4条件クイックスコア: 希少性/ニーズ/潜在性/参入障壁 (◎/○/△/×)
  - GO / CONDITIONAL GO / NO GO 判定
  - **原則: 「ユーザーに優しい嘘をつくより、正直に伝える方がはるかに親切」**

### 2. `.claude/skills/side-biz-idea-gen/references/generation-methods.md`
- 3つの生成法の詳細例とワークシート
  - サービス支出分析法（有料→無料、無料→収益化）
  - ユニーク交差点法（属性の掛け算でニッチ発見）
  - カテゴリブレスト法（既存8カテゴリ、フォールバック）
- 4条件スコアリング基準の定義（◎/○/△/×の判定基準）

### 3. `.claude/skills/side-biz-evaluator/references/derived-business.md`
- Tier定義: Tier 1（6-12ヶ月、自然な延長）/ Tier 2（12-18ヶ月、スキル深化）/ Tier 3（18ヶ月+、プラットフォーム化）
- スキル獲得テーブルのテンプレート
- 派生パスの代表例（コンテンツ→講座→コミュニティ→SaaS等）

### 4. `.claude/skills/side-biz-actionplan/scripts/generate_xlsx.py`
- openpyxlでXLSX生成（5シート構成）
  - 行動計画（Phase色分け、完了チェック欄）
  - KPI・目標（Phase別、目標/実績/達成率）
  - ツール・費用（無料/有料分類）
  - 法務・税務チェックリスト
  - 週次振り返りシート
- stdinからJSON受取 or 引数でデータ渡し
- openpyxl未インストール時はmarkdownフォールバック

---

## 既存ファイル改修 (6件)

### 5. `side-biz-hearing/SKILL.md`
- Part 1 (既存5項目): スキル/興味/時間/予算/リスク
- **Part 2 (新規5項目)**:
  - 有料サービス一覧（サブスク棚卸し）
  - 無料サービス一覧（日常アプリ）
  - 不満・困りごと（自分＋周囲）
  - 周囲の人の愚痴（同僚/家族/友人の不満→普遍的ペインポイント）
  - 願望・最近の変化（夢、目標、引越し等）
- **ユニーク特性分析**: 全10項目から「この人だけの交差点」を2-3個抽出

### 6. `side-biz-idea-gen/SKILL.md`
- 3つの生成法を追加（references/generation-methods.md参照）
- 4条件プレフィルタ: 2/4以上○のアイデアのみ提示
- ユーザー制約条件の明示的チェック（予算/時間/リスク/趣味性）

### 7. `side-biz-evaluator/SKILL.md`
- Part 1 (既存): 10項目の構造分析
- **Part 2 (新規): 7つの厳格質問**
  1. やっている人は本当にいない？
  2. どうやってマネタイズする？
  3. 週10時間で可能か？
  4. 競合に勝てる要素があるのか？
  5. 誰もやりたくないサービスか？
  6. 他の人ができないサービスか？
  7. サブスクにできるか？
  - 各問にYES/NO/PARTIAL + 根拠1-2文 + NOなら改善案
- **Part 3 (新規): 派生ビジネス分析**（references/derived-business.md参照）

### 8. `side-biz-compare/SKILL.md`
- 比較マトリクスに「4条件スコア」行を追加
- ランキングに「4条件重視」次元を追加（希少性25%/ニーズ30%/潜在性25%/参入障壁20%）
- 推薦に「Blue Ocean Champion」枠を追加

### 9. `side-biz-actionplan/SKILL.md`
- XLSX出力セクション追加（scripts/generate_xlsx.py呼び出し）
- 派生ビジネスロードマップセクション追加（90日後の展望）

### 10. `side-biz-planner/SKILL.md`
- 5ステップ線形 → **7ステップ反復ループ**に改修
- allowed-tools に `Skill(side-biz-validator)` を追加
- 反復ループルール:
  - NO GO → Step 2に戻り、却下理由を引き継いで再生成
  - 最大6ラウンド。6ラウンド後は最善候補を正直に提示
  - ユーザーが評価に異議 → 新しい視点で再検証（Round 6パターン）
- ユーザー制御ポイント: Step 3後・Step 5後に確認

---

## 実装順序

### Phase 1: 基盤（並列実行可）
- [1] side-biz-validator/SKILL.md 新規作成
- [2] side-biz-hearing/SKILL.md 改修
- [3] references/generation-methods.md 新規作成
- [4] references/derived-business.md 新規作成
- [5] scripts/generate_xlsx.py 新規作成

### Phase 2: SKILL.md更新（Phase 1の参照ファイル完成後）
- [6] side-biz-idea-gen/SKILL.md 改修
- [7] side-biz-evaluator/SKILL.md 改修
- [8] side-biz-compare/SKILL.md 改修
- [9] side-biz-actionplan/SKILL.md 改修

### Phase 3: オーケストレーター
- [10] side-biz-planner/SKILL.md 改修

### Phase 4: パッケージング
- 全7スキルの .skill ファイルを再生成（package_skill.py使用）

## 検証方法
- `quick_validate.py` で全スキルのフォーマット検証
- `package_skill.py` で全7つの .skill ファイルを再生成
- テスト起動: `/side-biz-planner` で一気通貫フローが動作することを確認
