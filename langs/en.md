# English (en) language pack

> **This pack is an AI draft and has not been checked by a native speaker.**
> If a row looks unnatural, please open an issue or a pull request.

| Field | Value |
|---|---|
| code | `en` |
| status | `ai-draft` |
| maintainers | none yet (native reviewers welcome) |
| default mode | `keyboard` |

## Input modes

| Mode | What it means |
|---|---|
| `keyboard` | PC keyboard, QWERTY layout |
| `mobile` | Phone touch keyboard with autocorrect and predictive text |
| `voice` | Speech-to-text dictation |

## Density

The intensity `1` baseline is one slip per 60 words. At intensity `N`, multiply the expected number of slips by `1 + (N - 1) / 3`. Intensity `10` matches the previous heavy density.

## Error catalog

- Weight: how often to pick the type (`high` / `medium` / `low`)
- Visibility: `subtle` if the writer would likely miss it on a quick reread, `noticeable` if it
  stands out. Use `noticeable` rows only in casual chat or at intensity `4`–`10`, unless the user
  explicitly selected that pattern
- The examples only illustrate the mechanism; apply it to the words in the user's text
- `habit` marks spelling beliefs, not typing slips. Keep them consistent within a text, but most
  texts should contain none

| ID | Type | Mode | Example (intended → mistake) | Why it happens | Weight | Visibility | Notes |
|---|---|---|---|---|---|---|---|
| K1 | Adjacent key | keyboard | the → thr, and → anf, just → jusr | A finger lands on the neighboring key | high | noticeable | Only real QWERTY neighbors |
| K2 | Transposed letters | keyboard | the → teh, because → becuase, with → wtih | Two fingers fire out of order | high | noticeable | |
| K3 | Dropped letter | keyboard | definitely → definitly, which → whic, really → realy | A key press is missed | high | subtle | Easiest to miss in long words |
| K4 | Doubled letter | keyboard | really → reallly, too → tooo | A key bounces or is held | medium | noticeable | |
| K5 | Missing space | keyboard | of the → ofthe, a lot → alot | The space bar is missed | medium | noticeable | |
| K6 | Capitalization slip | keyboard | I → i, The → THe, a lowercase sentence start | Shift pressed too late or released too late | medium | subtle | |
| K7 | Dropped apostrophe | keyboard | don't → dont, I'm → Im | The apostrophe key is skipped | medium | subtle | Never drop the n't itself |
| K8 | Common misspelling | keyboard | receive → recieve, separate → seperate, weird → wierd, until → untill | Spelling habit | medium | subtle | habit |
| K9 | Homophone slip | keyboard | their → there, your → you're, then → than | Typing fast by sound | medium | subtle | Never where it flips the meaning |
| M1 | Autocorrect real-word swap | mobile | we're → were, we'll → well, it's → its | Autocorrect picks a valid word | high | subtle | The intended word must stay obvious |
| M2 | Fat finger on a touch key | mobile | hi → ho, am → sm | Small keys, imprecise taps | high | noticeable | Only neighboring keys |
| M3 | Double-space period | mobile | Great  thanks → Great. thanks | The double-space shortcut inserts a period | low | noticeable | |
| V1 | Homophone | voice | write → right, whether → weather, here → hear, four → for | Speech-to-text picks the wrong spelling | high | subtle | |
| V2 | Word boundary | voice | apart → a part, anymore → any more, sometimes → some times | The recognizer splits or joins words | medium | subtle | |
| V3 | Missing punctuation | voice | Two sentences run together without a period or comma | Dictation does not add punctuation reliably | medium | subtle | Count one missing mark as one change |
| V4 | Dropped function word | voice | going to the store → going to store | Short unstressed words are not heard | medium | subtle | Never drop "not" or "no" |
| V5 | Filler | voice | So the plan is → So um the plan is | Fillers get transcribed | low | noticeable | At most once per text |

## Protect (language-specific)

- Negations: not, no, never, and every n't contraction keep their negative meaning
- Words whose misspelling reads as a different, meaningful claim (e.g. "now" → "not")

## Don't

- Don't produce autocorrect results that are vulgar or offensive
- Don't use misspellings that make the writer look uneducated unless the user asks for them
- Don't change regional spelling (color / colour); that is not a mistake
