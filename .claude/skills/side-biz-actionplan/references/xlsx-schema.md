# XLSX入力JSONスキーマ

`scripts/generate_xlsx.py` に渡す入力JSONの仕様。

## トップレベル構造

```json
{
  "title": "90日間アクションプラン（副業名）",
  "subtitle": "〇〇で月収10万円を目指す90日間",
  "weeks": 12,
  "review_columns": ["note PV", "Xフォロワー", "DB登録数", "収益(円)"],
  "phases": [...],
  "tool_groups": [...],
  "legal_items": [...]
}
```

| フィールド | 型 | 必須 | 説明 |
|-----------|-----|------|------|
| title | string | ✓ | スプレッドシートのタイトル |
| subtitle | string | | サブタイトル（概要・目標） |
| weeks | integer | | 週次振り返りの週数（デフォルト: 12） |
| review_columns | string[] | | 週次KPI列名（未指定時は汎用デフォルト） |
| phases | Phase[] | ✓ | Phase 1〜4の配列 |
| tool_groups | ToolGroup[] | | ツール・費用シートのグループ |
| legal_items | LegalItem[] | | 法務・税務チェックリスト |

## Phase オブジェクト

```json
{
  "name": "Phase 1",
  "label": "準備期間（Day 1-14）",
  "tasks": [
    {
      "day": 1,
      "week": "Week 1",
      "task": "競合サービスを10個リストアップ",
      "category": "リサーチ",
      "hours": 2,
      "output": "競合リストスプレッドシート"
    }
  ],
  "kpis": [
    {
      "item": "競合調査完了",
      "target": "10社以上"
    }
  ]
}
```

## ToolGroup オブジェクト

```json
{
  "label": "無料で使えるツール",
  "tools": [
    {
      "name": "Notion",
      "purpose": "コンテンツ管理・DB",
      "cost": "0円",
      "priority": "必須",
      "note": "無料プランで十分"
    }
  ]
}
```

## LegalItem オブジェクト

```json
{
  "item": "就業規則の副業規定確認",
  "detail": "会社の就業規則で副業が許可されているか確認する",
  "timing": "開始前"
}
```

## 実行コマンド

```bash
# JSON文字列から直接生成
echo '{"title":"...","phases":[...]}' | python3 scripts/generate_xlsx.py output.xlsx

# JSONファイルから生成
python3 scripts/generate_xlsx.py output.xlsx input.json

# テンプレートのみ生成（JSONなし）
python3 scripts/generate_xlsx.py template.xlsx
```
