---
name: side-biz-ideas
description: "[非推奨] side-biz-idea-genに移行済み。後方互換のためのラッパー。「副業アイデア」関連のリクエストはside-biz-idea-genが処理する。このスキルは2026-06-01に廃止予定。Use side-biz-idea-gen instead. This wrapper exists for backward compatibility only."
---

# side-biz-ideas（非推奨）

**このスキルは `side-biz-idea-gen` に置き換えられました。**

## 移行情報

- 後継スキル: `side-biz-idea-gen`
- 廃止予定日: 2026-06-01
- 変更理由: 4条件プレフィルタ、3つの生成メソッド、深掘りプロファイル対応を追加し、スキル名を役割に合わせてリネーム

## 動作

このスキルが呼ばれた場合、`/side-biz-idea-gen` に処理を委譲する。$ARGUMENTS はそのまま渡す。
