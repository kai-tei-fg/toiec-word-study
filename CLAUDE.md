# えいたんごクイズ プロジェクト

## プロジェクト概要
子供向け英単語クイズWebアプリケーション。
親が単語帳を管理し、子供がスマホでクイズに挑戦する運用を想定。

## 技術スタック
- HTML + CSS + JavaScript（単一ファイル構成、サーバー不要）
- Web Speech API（英語発音）、Web Share API（結果シェア）
- Google Fonts: M PLUS Rounded 1c（日本語）, Nunito（英語）

## ファイル構成
- `index.html` — 子供向けクイズアプリ本体
- `worklist-manager.html` — 親向け単語帳管理ツール
- `worklists/worklists.js` — 単語帳データ（管理ツールで生成、window.WORKLISTS形式）
- `SPEC.md` — **詳細仕様書（修正時は必ず参照のこと）**

## 開発ルール
- 各HTMLは単一ファイル完結（HTML/CSS/JSを1ファイルに含む）
- サーバー不要、file:// プロトコルでも動作すること
- モバイルファースト設計（子供はスマホ操作が主）
- 子供向け: 大きめフォント、ひらがな多用のUIテキスト
- 英語発音は en-US ネイティブ音声（日本語音声を除外するフィルタ必須）
- CSVエンコーディング: UTF-8（BOM付き可）/ Shift_JIS 自動判定対応
- UIカラーはCSS変数（:root）で管理、変更時は変数を修正
- worklists.js は `<script src="worklists/worklists.js">` で両HTMLから自動読み込み

## 修正時の注意事項
- 変更前に SPEC.md を読んで現在の仕様を正確に把握すること
- 機能追加・変更後は SPEC.md も更新すること
- テストは index.html をブラウザで開いて手動確認
