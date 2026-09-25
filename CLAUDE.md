<!-- このファイルはプロジェクト固有ルールのみを書く。個人/グローバル AI ルール
（言語・確認スタイル・出力フォーマット等）は各 AI ツールのグローバル設定へ。
fresh public clone でも有効な内容に保つこと。 -->

# typo-humanizer 開発ガイド

> **このファイルは索引であって本文ではない。** 全 AI セッションで全文がロードされるので、
> ルールの本文はここへ書かず、破る人が必ず開く場所（コード・検査スクリプト・skill・guide・台帳）へ置き、
> ここには索引の 1 行だけを残す。新しいルールを足す前に既存の CLAUDE.md・skill・guide・台帳を検索し、
> 正本が既にあれば参照だけにする。詳細は下記「設計原則の索引」。

## プロジェクト概要

文章に、人が打ったような揺らぎ（誤字、かな漢字の変換ミス、音声入力の聞き間違い）を足す AI エージェント用の skill。
アプリではなく Markdown だけで構成し、実行は `SKILL.md` を読んだ AI エージェントが行う。
ミスの種類は言語ごとの言語パック（`langs/<code>.md`）に分け、パックの無い言語は AI の推定で動かす。

## やらないこと（スコープ外）

- アプリ・CLI・ライブラリ・Web サービス化（Markdown の skill だけで完結させる）
- 文章の言い回しや口調の書き換え（別の humanizer の仕事）
- AI 判定ツールをすり抜けられると約束する記述
- 実在の人物の文体や癖をまねる機能

## 技術スタック

| 層 | 内容 |
|---|---|
| 本体 | Markdown（`SKILL.md` と `langs/*.md`） |
| 実行 | SKILL.md を読める AI エージェント（Claude Code 等） |
| 検査 | Node.js スクリプト（`scripts/secrets-scan.mjs` / `scripts/check-claude-md.mjs`） |

## ディレクトリ構成

- `SKILL.md`: skill の本体（入力・手順・保護範囲・出力）
- `langs/`: 言語パック。`_template.md` が書式の正本、`ja.md` / `en.md` が同梱のパック
- `README.md` / `README.ja.md`: 利用者向けの説明
- `scripts/`: 公開前の検査スクリプト
- `.githooks/` / `.github/workflows/`: 検査の自動実行

## 主要コマンド

- 秘密の走査（staged）: `node scripts/secrets-scan.mjs --staged --block`
- CLAUDE.md の構造検査: `node scripts/check-claude-md.mjs`

## 設計原則の索引（本文は正本にある）

事故から生まれた設計ルールを追記する表。**本文はここに書かず、破る人が必ず開く場所に置く。**
機械検査があるものは、それが最終的な歯止め（`scripts/check-claude-md.mjs` があれば CI/hook で検査）。

| ルール | 正本（本文はここ） | 機械検査 |
|---|---|---|
| 誤字の入れ方・保護範囲・出力形式 | `SKILL.md` | なし |
| 言語パックの書式と status の意味 | `langs/_template.md` | なし |

**新しいルールを足す前に、まずこの表に 1 行足せる形にできないかを考える。** できないもの
（機械検査も、決まったファイルも無いもの）だけが本文を持ってよい。

## AI 作業共通ルール

ビルド・コミット禁止、secrets-scan 責務、plan/bugfix/pending md の作成ルール等の AI 作業共通ルールは、各利用者のグローバル AI 設定に従う（作者環境の例: `~/.claude/CLAUDE.md` および `~/.claude/guides/`）。

- README とパックの例文は合成データで書く（実在の人名・社名・実際のメッセージを使わない）
- 言語パックの status を `native-verified` に変えるのは、その言語のネイティブが全行を確認したときだけ

## Obsidian artifacts

If `docs/obsidian/README.md` exists, use it as an index for related knowledge artifacts.
Use the repository-relative `docs/obsidian` entry. Do not write to a central absolute
path and do not silently fall back to `docs/local` when the entry is missing.

## secrets-scan（このリポジトリの配線）

書く瞬間の責務（固有名詞の一般化・fixture は合成データ等）は上記「AI 作業共通ルール」の参照先に従う。このリポジトリ固有の配線は以下:

- scanner: `scripts/secrets-scan.mjs`（手動実行: `node scripts/secrets-scan.mjs --staged --block`）
- layer 2: pre-commit hook（`.githooks/`・有効化は `scripts/install-hooks.sh` か `scripts/install-hooks.ps1`）/ layer 3: `.github/workflows/secrets-scan.yml` / layer 4: release ゲート
- env (full coverage に必要・未設定なら構造 regex のみで継続): `KB_ROOT` / `FAMILY_ROOT`。設定詳細は `scripts/secrets-scan.mjs` の冒頭コメント
- 参照実装・設計詳細: `worklog-bridge` リポの `docs/local/secrets-scan-design/`（gitignored・公開しない）

## 関連ドキュメント

| 項目 | パス |
|---|---|
| ユーザー向け README | `README.md` / `README.ja.md` |
| Codex/他 AI 用入口 | `AGENTS.md` |
| ローカル作業ノート（非公開） | `docs/local/`（存在する場合） |
| Obsidian knowledge artifacts | `docs/obsidian/`（存在する場合。作業キューではない） |
