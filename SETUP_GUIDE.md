# VSCode + Claude Code セットアップ＆開発ガイド

このドキュメントでは、えいたんごクイズアプリの開発を VSCode + Claude Code で継続するための手順を説明します。

---

## 1. 前提条件

| 必要なもの | 詳細 |
|-----------|------|
| **Anthropicアカウント** | Claude Pro / Max / Team / Enterprise のいずれか（無料プランは非対応） |
| **VSCode** | バージョン 1.98.0 以降推奨 |
| **Node.js** | 18.0 以降（ネイティブインストーラーなら不要） |
| **Git** | 2.23 以降 |

---

## 2. Claude Code のインストール

### 方法A: ネイティブインストーラー（推奨）

Node.js不要で最も簡単です。

**macOS / Linux:**

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**Windows（PowerShell）:**

```powershell
irm https://claude.ai/install.ps1 | iex
```

**macOS（Homebrewの場合）:**

```bash
brew install --cask claude-code
```

### 方法B: npm（Node.js環境がある場合）

```bash
npm install -g @anthropic-ai/claude-code
```

### インストール確認

```bash
claude --version
```

バージョン番号が表示されれば成功です。

---

## 3. 認証（初回のみ）

```bash
claude
```

初回起動時にブラウザが開き、Anthropicアカウントでのログインを求められます。認証完了後、ターミナルに戻って使用開始できます。

---

## 4. VSCode 拡張機能のインストール

1. VSCode を開く
2. `Ctrl+Shift+X`（Mac: `Cmd+Shift+X`）で拡張機能パネルを開く
3. **「Claude Code」** で検索
4. **Anthropic** 発行のものをインストール（他の非公式拡張に注意）
5. インストール後、左サイドバーに **✱（Spark）アイコン** が表示される

拡張機能が表示されない場合は、VSCode を再起動してください。

---

## 5. プロジェクトのセットアップ

### 5.1 フォルダ構成を準備

```
📁 eitan-quiz/
├── vocab-quiz.html
├── worklist-manager.html
├── sample_words.csv
├── SPEC.md                ← 仕様書（Claude Codeの参照用）
├── CLAUDE.md              ← これから作成するClaude Code用の指示ファイル
└── 📁 worklists/
    └── worklists.js
```

### 5.2 VSCodeでプロジェクトを開く

```bash
cd eitan-quiz
code .
```

### 5.3 CLAUDE.md を作成する

プロジェクトルートに `CLAUDE.md` を作成します。このファイルは Claude Code がプロジェクトの文脈を理解するための指示書です。

```markdown
# えいたんごクイズ プロジェクト

## プロジェクト概要
子供向け英単語クイズWebアプリ。親が単語帳を管理し、子供がスマホでクイズに挑戦する。

## 技術スタック
- HTML + CSS + JavaScript（単一ファイル構成、サーバー不要）
- Web Speech API（発音）、Web Share API（シェア）
- Google Fonts: M PLUS Rounded 1c, Nunito

## ファイル構成
- `vocab-quiz.html` — 子供向けクイズアプリ本体
- `worklist-manager.html` — 親向け単語帳管理ツール
- `worklists/worklists.js` — 単語帳データ（管理ツールで生成）
- `SPEC.md` — 詳細仕様書（必ず参照のこと）

## 開発ルール
- 各HTMLは単一ファイル（HTML/CSS/JSを1ファイルに含む）
- サーバー不要で file:// でも動作すること
- モバイルファーストのUI設計
- 子供向けに大きめの文字、ひらがな多用
- 発音はアメリカ英語（en-US）のネイティブ音声を使用
- CSVはUTF-8/Shift_JIS両対応

## 修正時の注意
- 変更前にSPEC.mdを参照して現在の仕様を確認すること
- UIの配色はCSS変数（:root）で定義済み
- worklists.jsのフォーマットは window.WORKLISTS = {...} 形式
```

---

## 6. Claude Code の使い方

### 6.1 起動方法

VSCodeで以下のいずれかの方法で起動できます。

- **Sparkアイコン** — 左サイドバーのアイコンをクリック
- **ステータスバー** — 右下の「✱ Claude Code」をクリック
- **エディタツールバー** — ファイルを開いた状態で右上のSparkアイコン
- **ショートカット** — `Cmd+Shift+P`（Mac）/ `Ctrl+Shift+P`（Win）→「Claude Code」と入力

### 6.2 基本的なプロンプトの書き方

Claude Code にプロジェクトの修正を依頼する際の例です。

**仕様を確認させてから修正依頼:**

```
SPEC.md を読んで、vocab-quiz.html の結果画面に
「もう一回おなじ単語でテスト」ボタンを追加してください。
同じ単語セットを再シャッフルして再テストできるようにしてください。
```

**ファイルを @メンションして指示:**

```
@vocab-quiz.html の選択式問題で、
選択肢を選んだ後に正解の英単語を自動で発音するようにしてください。
```

**バグ修正:**

```
@worklist-manager.html で削除ボタンを押しても
モーダルが表示されない不具合を修正してください。
```

### 6.3 便利なショートカット

| ショートカット | 動作 |
|---------------|------|
| `Option+K` (Mac) / `Alt+K` (Win) | 選択中のコードを @メンション付きでプロンプトに挿入 |
| `Cmd+N` (Mac) / `Ctrl+N` (Win) | 新しい会話を開始 |

### 6.4 差分の確認と適用

Claude Code がファイルを変更する際は、VSCodeのネイティブ diff ビューで変更内容が表示されます。

- **Accept** — 変更を適用
- **Reject** — 変更を却下

設定で Auto-Accept を有効にすると、変更が自動適用されます（信頼できるワークフロー向け）。

---

## 7. 実際の開発フロー例

### 例1: 新機能を追加する

```
1. VSCodeでプロジェクトを開く
2. Claude Code パネルを開く
3. 以下のようにプロンプトを入力:

   「SPEC.md を参照して、vocab-quiz.html に
   テスト中に「やめる」ボタンを追加してください。
   押すとHOME画面に戻り、途中経過は破棄されます。
   ボタンのスタイルは既存のbtn-outlineクラスを使ってください。」

4. Claude Code が提案する差分を確認
5. Accept/Reject で反映を制御
6. ブラウザでvocab-quiz.htmlを開いて動作確認
```

### 例2: バグを修正する

```
1. 問題のあるコードを選択
2. Option+K（Alt+K）でメンション挿入
3. 「この部分で○○の問題が起きています。修正してください」と入力
4. 差分を確認して適用
```

### 例3: SPEC.md を更新する

仕様変更後は SPEC.md も更新しておくと、次回以降の開発がスムーズです。

```
「vocab-quiz.html に「やめるボタン」を追加しました。
SPEC.md のQUIZ画面セクションにこの機能の仕様を追記してください。」
```

---

## 8. Tips

### 仕様書をうまく活用する

Claude Code は `CLAUDE.md` を自動で読み込みます。その中で「SPEC.md を参照」と書いておくことで、毎回仕様を伝える手間が省けます。

### 変更が大きい場合はPlan Modeを使う

Claude Code には Plan Mode があり、実行前に計画を確認・編集できます。大きな変更の場合は「まず計画を立てて見せてください」と伝えると安全です。

### Git で管理する

```bash
cd eitan-quiz
git init
git add .
git commit -m "初期バージョン"
```

Claude Code はGit履歴も参照できるため、バージョン管理しておくとより精度の高い提案が得られます。また、問題があった場合にすぐ戻せます。

### Checkpoints（チェックポイント）

Claude Code は変更前に自動でチェックポイントを保存します。`Esc` を2回押すか `/rewind` コマンドで以前の状態に巻き戻せます。

---

## 9. トラブルシューティング

| 問題 | 対処法 |
|------|--------|
| Claude Code が認識されない | ターミナルを再起動、`claude --version` で確認 |
| VSCode拡張が表示されない | VSCode再起動、または「Developer: Reload Window」実行 |
| Node.js バージョンエラー | `node --version` で18以上か確認。ネイティブインストーラーなら不要 |
| 認証エラー | `claude logout` してから `claude` で再認証 |
| 動作がおかしい | `claude doctor` で自動診断 |
