# <Language name> (<code>) language pack

<!--
How to add a language:
1. Copy this file to langs/<code>.md, where <code> is the ISO 639-1 code (vi, ko, de, ...).
2. Fill in every section. You may write the pack in English or in the language itself.
3. Use it locally right away (the skill reads langs/<code>.md), or open a pull request.
Delete this comment when you are done.
-->

| Field | Value |
|---|---|
| code | `<code>` |
| status | `native-verified` / `ai-draft` / `community` |
| maintainers | <GitHub handles, or "none yet"> |
| default mode | <one of the modes below> |

Status meanings:

- `native-verified`: a native speaker checked every row
- `ai-draft`: written by an AI and not yet checked by a native speaker
- `community`: contributed by native speakers but not reviewed by the maintainer

## Input modes

List how people actually type this language. Keep the mode names short; users ask for them by name.

| Mode | What it means |
|---|---|
| `keyboard` | <e.g. PC keyboard layout and input method> |
| `mobile` | <e.g. phone keyboard, autocorrect, predictive text> |
| `voice` | <speech-to-text> |

## Density

Define the baseline density at intensity `1`. Count characters for languages written without spaces,
words otherwise. At intensity `N`, the skill multiplies the expected number of slips by
`1 + (N - 1) / 3`; intensity `10` is four times the baseline.

| Field | Value |
|---|---|
| Intensity 1 baseline | 1 per <N characters or words> |

## Error catalog

One row per mistake type. Every row needs a real example (intended → mistake) and the reason it
happens; the reason is what lets a reviewer who is not a native speaker judge the row.

| ID | Type | Mode | Example (intended → mistake) | Why it happens | Weight | Visibility | Notes |
|---|---|---|---|---|---|---|---|
| K1 | <adjacent key> | keyboard | <word → wrod> | <fingers land on the neighboring key> | high | noticeable | |
| M1 | <...> | mobile | | | medium | subtle | |
| V1 | <...> | voice | | | low | subtle | |

- Weight: `high` / `medium` / `low`, meaning how often the skill should pick this type
- Visibility: `subtle` if the writer would likely miss it on a quick reread, `noticeable` if it
  stands out. Slips that survive into sent text are mostly subtle, so the skill uses `noticeable`
  rows only in casual chat or at intensity `4`–`10`, unless the user explicitly selected that pattern
- The examples only illustrate the mechanism. The skill applies the mechanism to the words in the
  user's text, so describe the mechanism clearly in "Why it happens"
- Add `habit` in Notes for spelling beliefs (not typing slips) that a person repeats whenever the same
  word appears. The skill uses habits rarely

## Protect (language-specific)

Patterns in this language that must never be changed, in addition to the rules in `SKILL.md`
(for example, negation particles or honorific forms whose change reads as rude).

- <...>

## Don't

Mistakes that would look wrong, mocking, or offensive in this language.

- <...>
