# AI Writing Voice Skill

A portable [Agent Skill](https://www.cursor.com/docs/context/skills) that audits a piece of writing for AI-isms — sycophant openers, contrast clichés, em-dash storms, lexical tells like *delve* and *tapestry*, listicle parallelism, bookend summaries, smug transitions, hedge clusters — and scores it on a **1–10 AI-Voice scale**.

Works in **Cursor**, **Claude Code**, and any other agent that reads the open `SKILL.md` standard.

## What it does

Given any text (a paragraph, a chapter, a blog post, a README), the skill:

1. Reads the text in full.
2. **Annotates every AI-ism it finds** — quoted, tagged by category, no skipping.
3. Tallies counts per category.
4. Scores the text 1–10:

| Score | Band | Meaning |
|------:|:-----|---------|
| 1–2 | 🟢 GREEN | Ship it. Human voice intact. |
| 3–6 | 🟡 YELLOW | Edit pass needed. Human voice still drives but tells are visible. |
| 7–10 | 🔴 RED | Structural rewrite. Reads like a model with light editing or worse. |

5. Lists the highest-leverage fixes to drop the score by a band.

The score is computed via a reproducible additive heuristic, not a vibe.

## Installation

### Cursor (recommended — Remote Rule)

1. Open Cursor Settings (Cmd+Shift+J on Mac, Ctrl+Shift+J on Windows/Linux)
2. Navigate to **Rules**
3. In **Project Rules**, click **Add Rule** → **Remote Rule (Github)**
4. Paste this repo's URL: `https://github.com/SilasReinagel/AiWritingVoiceSkill`

### Cursor / Claude Code (manual)

Drop `SKILL.md` into one of these locations:

| Location | Scope |
|----------|-------|
| `~/.cursor/skills/ai-writing-voice/SKILL.md` | Personal, all projects (Cursor) |
| `.cursor/skills/ai-writing-voice/SKILL.md` | Project, shared via repo (Cursor) |
| `~/.claude/skills/ai-writing-voice/SKILL.md` | Personal (Claude Code) |
| `~/.agents/skills/ai-writing-voice/SKILL.md` | Open-standard fallback |

```bash
mkdir -p ~/.cursor/skills/ai-writing-voice
curl -L https://raw.githubusercontent.com/SilasReinagel/AiWritingVoiceSkill/main/SKILL.md \
  -o ~/.cursor/skills/ai-writing-voice/SKILL.md
```

## Usage

In your agent chat:

```
/ai-writing-voice  audit @path/to/draft.md
```

or just:

```
Use the ai-writing-voice skill on this paragraph: "..."
```

The skill is designed for **explicit invocation**. It will not auto-fire on every edit.

## Example output

````markdown
# AI-Voice Audit

**Target:** draft-intro.md, ~480 words
**Score:** 7/10  🔴 RED
**One-line verdict:** Sycophant opener, three contrast clichés, twelve em dashes, and four lexical tells — reads like a lightly edited model draft.

## Tally
- sycophant-opener: 1
- contrast-cliché: 3
- em-dash-overuse: count=12 (budget=2)
- lexical-tell: 4  (delve, tapestry, journey, robust)
- phrasal-tell: 2
- listicle-parallelism: yes
- tricolon-abuse: 2
- bookend-summary: yes
- smug-transition: 3
- hedge-cluster: 0
- generic-scene-set: 1
- symmetric-paragraphs: no

## Findings
[AI-ism: sycophant-opener] "What a great question to start with..."
[AI-ism: generic-scene-set] "In today's fast-paced world of work..."
[AI-ism: contrast-cliché] "It's not about the hours. It's about the output."
[AI-ism: contrast-cliché] "This isn't a process. It's a promise."
[AI-ism: contrast-cliché] "Don't think of it as overhead — think of it as investment."
[AI-ism: lexical-tell "delve"] "...we'll delve into the mechanics of..."
[AI-ism: lexical-tell "tapestry"] "...the tapestry of modern teamwork..."
[AI-ism: lexical-tell "journey"] "...your journey toward operational excellence..."
[AI-ism: lexical-tell "robust"] "...a robust framework for..."
[AI-ism: em-dash-overuse, count=12, budget=2]
[AI-ism: smug-transition "moreover"] "Moreover, the data suggests..."
[AI-ism: smug-transition "furthermore"] "Furthermore, we observe..."
[AI-ism: smug-transition "in conclusion"] "In conclusion, the path forward..."
[AI-ism: listicle-parallelism] All four bullets in §2 use bold-noun + colon + 2-clause description
[AI-ism: tricolon-abuse] "clear, concise, and compelling"
[AI-ism: tricolon-abuse] "people, process, and platform"
[AI-ism: bookend-summary] closing paragraph restates lines 2–4
[AI-ism: phrasal-tell] "It's important to note that..."
[AI-ism: phrasal-tell] "At the end of the day..."

## Highest-leverage fixes
1. Cut the opening paragraph entirely. Start at the second paragraph's first concrete claim.
2. Convert the contrast clichés into direct positive statements. Keep at most one.
3. Em-dash diet: replace 10 of the 12 with periods or commas.
````

## The AI-isms it catches

- **Sycophant openers:** "Great question.", "What a fascinating insight.", etc.
- **Contrast clichés:** "It's not X, it's Y" patterns.
- **Lexical tells:** *delve, navigate, unleash, tapestry, realm, journey, harness, foster, leverage, robust, comprehensive, holistic, seamless, transformative, nuanced…*
- **Phrasal tells:** "In today's fast-paced world…", "At its core…", "Let's dive in.", "Whether you're X or Y…", "Not only X, but also Y."
- **Structural tells:** em-dash overuse, tricolon abuse, listicle parallelism, bookend summaries, symmetric paragraphs.
- **Smug transitions:** *indeed, moreover, furthermore, additionally, thus, in conclusion.*
- **Hedge clusters:** "may potentially", "could possibly", "it's worth considering."

Full annotation tag list lives in [`SKILL.md`](./SKILL.md).

## Why use this

Because LLM-assisted writing leaks. Even careful editors miss the tells because the prose is *grammatically fine* — it's the cadence that gives it away. This skill makes the leak visible: every fingerprint quoted, tagged, and counted, with a single number you can ship against.

Targets a hard constraint: **no chapter ships above a 2/10**.

## Compatibility

- Format: open Agent Skills standard (`SKILL.md` with YAML frontmatter)
- Tested in: Cursor 2.4+, Claude Code
- License: MIT

## Contributing

Found a new AI-ism the skill misses? Open an issue or PR. The field guide is meant to evolve as the models do.

## License

[MIT](./LICENSE)
