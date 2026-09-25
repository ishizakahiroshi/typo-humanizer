# typo-humanizer

[English](README.md) | 日本語

文章に、人が打ったような揺らぎを足す AI エージェント用の skill です。誤字、かな漢字の変換ミス、
音声入力の聞き間違いを混ぜます。中身は Markdown だけで、インストールするアプリはありません。
AI エージェントが `SKILL.md` を読んで作業します。

## なぜ

AI が書いた文章は、整いすぎていることがあります。人が書いた文章には、打ち方から生まれる小さな
ミスが残ります。隣のキーを押した、変換候補を 1 つずれて選んだ、音声入力が別の語に聞き取った、
といったものです。この skill はそうしたミスだけを足し、言い回しは書き換えません。

使いどころ:

- 機械的に整いすぎて見えると困る、くだけた投稿やチャットの返信
- 誤字への強さを試すテストデータ（検索、スペルチェッカー、入力フォームの検証、音声 UI）
- 登場人物がスマホで打ったり音声入力したりする場面の台詞や小説
- 実際の入力に近いサンプル文が要るモックアップやデモ

## これは何ではないか

- **文章の書き換えツールではありません。** 言い回しや口調を変えたいときは
  [humanizer-ja](https://github.com/gonta223/humanizer-ja) などの humanizer で整えてから、この skill で揺らぎを足してください。
- **AI 判定ツールの結果は約束しません。** レポートや課題など、AI の文章を自分の文章として出してはいけない場面では使わないでください。

## 例

同じ文に、入力方式ごとに 1 か所ずつ必ず入れるよう指定した例です（合成した例文）。

| mode | 文 |
|---|---|
| 元の文 | ちょっと遅れますが、明日の資料は送っておきます。確認をお願いします。 |
| keyboard（PC のローマ字入力） | ちょっと遅れますが、明日の資料は送っておきます。確認をおねがいしmす。 |
| flick（スマホのフリック入力） | ちよっと遅れますが、明日の資料は送っておきます。確認をお願いします。 |
| voice（音声入力） | ちょっと遅れますが、明日の資料は送って置きます。確認をお願いします。 |

## 導入

エージェントごとの個人用 skills フォルダへ clone します。保存先によって対応するプロジェクトが
異なります。最新の読み込み仕様は、公式の [Codex skills ガイド](https://developers.openai.com/codex/skills) と
[Claude Code skills ガイド](https://code.claude.com/docs/en/skills) を参照してください。

```sh
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/ishizakahiroshi/typo-humanizer "$HOME/.agents/skills/typo-humanizer"
```

Windows（PowerShell）:

```powershell
New-Item -ItemType Directory -Force "$HOME\.agents\skills" | Out-Null
git clone https://github.com/ishizakahiroshi/typo-humanizer "$HOME\.agents\skills\typo-humanizer"
```

Codex の更新:

```sh
git -C "$HOME/.agents/skills/typo-humanizer" pull --ff-only
```

### Claude Code

macOS / Linux:

```sh
mkdir -p "$HOME/.claude/skills"
git clone https://github.com/ishizakahiroshi/typo-humanizer "$HOME/.claude/skills/typo-humanizer"
```

Windows（PowerShell）:

```powershell
New-Item -ItemType Directory -Force "$HOME\.claude\skills" | Out-Null
git clone https://github.com/ishizakahiroshi/typo-humanizer "$HOME\.claude\skills\typo-humanizer"
```

Claude Code の更新:

```sh
git -C "$HOME/.claude/skills/typo-humanizer" pull --ff-only
```

ほかのエージェントでは、公式の個人用 skills フォルダに置くか、作業前に `SKILL.md` を読むよう
指示してください。

## 使い方

普通の言葉で頼みます。

- 「この文章に誤字を混ぜて。スマホのフリック入力風、弱めで」
- 「この文章に誤字を混ぜて。フリックの F2 を強度 3 で必ず使って」
- 「音声入力で書いたっぽくして。中くらいで、どこを変えたかも見せて」
- 「この文に誤字を混ぜて。K5 は除外して、目立たない誤字だけにして」
- "Add a few typos to this: ..."

| 指定 | 値 | 既定 |
|---|---|---|
| 言語 | 言語名か ISO 639-1 のコード | 文章から判定 |
| 入力方式 | `keyboard` / `flick` / `voice`（英語などは `flick` の代わりに `mobile`） | 言語パックの既定 |
| 誤字パターン | パックにある型の ID または名前を 1 つ以上 | 条件に合う全型 |
| 除外パターン | 使わない型の ID または名前を 1 つ以上 | なし |
| 目立ち度の上限 | `automatic` / `subtle-only`（目立つ型を使わない） | パックの通常動作 |
| ゆらぎ度 | `N/10`。10 回頼んだら何回誤字が入るか。「必ず」は `10/10` | `2/10` |
| 強度 | 誤字が入る回に何か所入れるか。`1`〜`10`（10 は 1 の約 4 倍）。`light` / `medium` / `heavy` はそれぞれ `1` / `4` / `10` と同じ | `1` |
| 個数 | 「3 か所」のように誤字の数を指定。ゆらぎ度と強度を上書きし、その数を必ず入れる | 指定なし |
| 文章の種類 | `chat` / `email` / `document` | 文章から判定 |
| 変更点の表示 | あり / なし | なし |
| 保護 | 触ってほしくない語や範囲 | — |
| 利用ログ | `on` / `off` / `summary` | off |

**ほとんどの回は、文章がそのまま返ってきます。わざとです。** 人は毎回誤字をするわけではなく、
毎回どこかが間違っている文章は、人らしいというより雑に見えます。既定のゆらぎ度 `2/10` では、
10 回頼んで 2 回くらい誤字が入ります。強度は発生確率と別の指定で、誤字が入る回の個数を調整します。
強度は従来の light から heavy までの範囲を 10 段階に等分します。1 が light、4 が medium、10 が heavy 相当で、
10 の密度は 1 の約 4 倍です。
エージェントがコマンドを実行できるときは 1 行のコマンドで
サイコロを振ります。実行できないときは AI が判断するので、頻度がずれることがあります。
確実に入れたいときは「必ず」、パターン、または強度を指定してください。個数を指定すると、その数を入れます。
パターンまたは強度を指定し、ゆらぎ度を指定しない場合は 1 件以上入れます。ゆらぎ度も同時に指定した場合は、
個数の指定がない限り、そのゆらぎ度に従います。

除外した型は選択対象から外します。指定した型を同時に除外した場合や、`subtle-only` と
「目立つ型を使う」指定が衝突した場合は、理由を説明して元の文を返します。別の型へ勝手に
置き換えません。

誤字が入る回は、書いた本人が読み返しても見落としそうな誤字を選びます。置き方も均等にせず、
長い文や長い文章の後半に寄せます。丁寧な文章では数を減らします。

数字・日付・金額・URL・コード・ファイルパス・名前・引用・否定の語には、どの設定でも手を付けません。

### ローカル利用ログ

利用ログは初期状態ではオフです。「利用ログをオン」で記録を開始し、「利用ログをオフ」で以後の
記録を止めます。「利用ログを集計」で、蓄積済みの回数や誤字数を集計できます。設定と CSV は
ユーザーのホームフォルダに置きます。場所は `~/.typo-humanizer/settings.json` と
`~/.typo-humanizer/usage.csv` です（Windows では通常 `%USERPROFILE%` の下です）。

CSV に記録するのは日時、言語、入力方式、文章の種類、ゆらぎ度、強度、追加した誤字の数、型 ID と
その個数です。入力文・出力文・指示文・名前・元ファイルのパスは記録しません。skill 自体はログを
アップロード・同期しませんが、PC 側のバックアップや同期ソフトがホームフォルダを複製する場合は
その対象になりえます。エージェントがファイルにアクセスできない場合は、文章の処理を続け、記録
できなかったことを伝えます。集計時も個々の CSV 行は表示しません。

```csv
timestamp,language,mode,genre,frequency,intensity,inserted_count,patterns
```

## 言語パック

言語ごとに、実際に起きるミスの一覧を `langs/<code>.md` に持っています。

| code | 言語 | 状態 | 入力方式 |
|---|---|---|---|
| `ja` | 日本語 | AI 下書き・作者の確認待ち | keyboard（ローマ字入力）、flick、voice |
| `en` | 英語 | AI 下書き・ネイティブの確認を募集中 | keyboard、mobile、voice |

**パックの無い言語でも動きます。** 言語を指定すると、AI が自分の知識からその言語で起きやすい
ミスを推定して入れます。その場合は、ネイティブから見て不自然になることがあるため、出力の冒頭に
「専用パックを使っていない」旨の注意が付きます。

不自然だと感じたら、パックを書いてください。

1. `langs/_template.md` を `langs/<code>.md` としてコピーする
2. 入力方式、密度、ミスの一覧を埋める。各行に例と「なぜそのミスが起きるか」を書く
3. 手元でそのまま使うか、pull request を送る

英語パックの確認や、新しい言語のパックの追加を歓迎します。

## ライセンス

MIT。詳細は [LICENSE](LICENSE) を参照してください。
