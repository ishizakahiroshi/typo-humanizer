# typo-humanizer

English | [日本語](README.ja.md)

An agent skill that adds human-like slips to text: typos, IME conversion mistakes, and speech-to-text
errors. It is plain Markdown. There is no app to install; your AI agent reads `SKILL.md` and does the work.

## Why

AI-written text is often too clean. Text written by people carries small slips that come from the
way it was typed: a key next to the right one, a wrong conversion candidate, a word the voice
recognizer misheard. This skill adds those slips, and only those. It does not rewrite your wording.

Useful for:

- Casual posts and chat replies that shouldn't read as machine-polished
- Test data for typo tolerance: search, spell checkers, form validation, voice interfaces
- Dialogue and fiction where characters text or dictate
- Mockups and demos that need realistic user input

## What it is not

- **Not a rewriter.** For wording and tone, use a style humanizer such as
  [blader/humanizer](https://github.com/blader/humanizer), then add slips with this skill.
- **No promise about AI detectors.** Don't use it to present AI-generated work as your own where
  that is not allowed, such as coursework.

## Example

The same sentence with one slip forced in each input mode (synthetic example):

| Mode | Text |
|---|---|
| original | I'll send the slides tomorrow. Let me know if you have any questions. |
| keyboard | I'll send the slides tomorrow. Let me know if you have any quesitons. |
| mobile | Ill send the slides tomorrow. Let me know if you have any questions. |
| voice | I'll send the slides tomorrow let me know if you have any questions. |

Japanese:

| Mode | Text |
|---|---|
| original | ちょっと遅れますが、明日の資料は送っておきます。確認をお願いします。 |
| keyboard | ちょっと遅れますが、明日の資料は送っておきます。確認をおねがいしmす。 |
| flick | ちよっと遅れますが、明日の資料は送っておきます。確認をお願いします。 |
| voice | ちょっと遅れますが、明日の資料は送って置きます。確認をお願いします。 |

## Install

For Claude Code, clone the repository into your personal skills folder:

```sh
git clone https://github.com/ishizakahiroshi/typo-humanizer ~/.claude/skills/typo-humanizer
```

On Windows (PowerShell):

```powershell
git clone https://github.com/ishizakahiroshi/typo-humanizer "$HOME\.claude\skills\typo-humanizer"
```

For other agents, put the folder wherever the agent loads skills from, or tell the agent to read
`SKILL.md` before the task.

## Usage

Ask in plain language:

- "Add a few typos to this: ..."
- "Make this look like it was dictated on a phone. Medium, and show me the changes."
- "Use keyboard pattern K5 at intensity 3, every time, and show the changes: ..."
- 「この文章に誤字を混ぜて。スマホのフリック入力風、弱めで」

| Option | Values | Default |
|---|---|---|
| language | any language name or ISO 639-1 code | detected from the text |
| input mode | `keyboard` / `mobile` / `voice` (Japanese uses `flick` instead of `mobile`) | the pack's default |
| pattern(s) | one or more IDs or type names from the language pack | all eligible patterns |
| frequency | `N/10`: how many requests in 10 get any slip. "always" means `10/10` | `2/10` |
| intensity | `1`–`10`: how many slips to aim for when this text gets them. Specifying an intensity forces a slip unless you also specify frequency. `light` / `medium` / `heavy` remain aliases for `1` / `4` / `10` | `1` |
| exact count | a count such as "3 typos"; overrides frequency and intensity and forces that count | none |
| genre | `chat` / `email` / `document` | detected from the text |
| show changes | on / off | off |
| protect | words or spans to leave untouched | — |

**Most requests come back unchanged, on purpose.** People don't make a typo in every message, and
text that always has one reads as careless rather than human. At the default `2/10`, about 2 requests
in 10 get slips. Intensity is separate from frequency: `1` is the lightest amount and `10` targets
four times as many slips as `1`, spread evenly across ten levels. If the agent can run shell commands, it rolls the dice with a one-line command;
otherwise the model decides, and the rate may drift. Say "always" to force slips. Selecting a pattern
or specifying an intensity also forces a slip unless you specify a frequency; an exact count overrides both and forces that count.

When a text does get slips, the skill prefers ones a writer would miss on a quick reread, clusters
them the way real slips cluster (long sentences, the end of a long text), and uses fewer in formal
writing.

Never changed: numbers, dates, amounts, URLs, code, file paths, names, quotations, and negations.

## Language packs

Each language keeps its own list of realistic mistakes in `langs/<code>.md`.

| Code | Language | Status | Modes |
|---|---|---|---|
| `ja` | Japanese | AI draft, review by the author pending | keyboard (romaji IME), flick, voice |
| `en` | English | AI draft, native review welcome | keyboard, mobile, voice |

**Other languages still work.** Name the language and the skill infers typical mistakes from the
model's knowledge. The output then starts with a notice that no pack was used, because the result may
look unnatural to native speakers.

If the result looks off, write a pack:

1. Copy `langs/_template.md` to `langs/<code>.md`
2. Fill in the input modes, the density, and the mistake catalog, with an example and a reason for each row
3. Use it locally right away, or open a pull request

Native speakers reviewing the English pack, or adding a new one, are very welcome.

## License

MIT
