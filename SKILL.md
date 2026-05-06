---
name: ai-writing-voice
description: Detect AI-isms in written content and score it on a 1-10 AI-voice scale (1-2 green / human, 3-6 yellow / mixed, 7-10 red / machine-cadenced). Use when the user asks to check for AI voice, AI-isms, ChatGPT-isms, LLM tells, machine-cadenced prose, or wants a "humanness" / "AI-voice" rating on a piece of writing.
---

# AI Writing Voice Detector

Forensic audit of a piece of writing for AI-generated cadence and lexical fingerprints. Output a categorized list of every AI-ism found and a single AI-Voice score from 1 (unmistakably human) to 10 (raw LLM output).

This is not a style critique. It's a fingerprint audit. Show receipts.

## Inputs

The target is whatever text the user supplies — a paragraph, a chapter, a blog post, a tweet, a README. If the user points at a file, read it. If they paste text, work from the paste. If they don't specify, ask what to audit.

## Workflow

1. **Read the target in full.** Do not skim. Cadence tells require reading sequence.
2. **Tally each AI-ism category** below. Keep counts.
3. **Annotate every instance.** Quote the offending text with its category tag (see Annotation Format). Do not skip "minor" ones, do not summarize, do not collapse repeats — list each occurrence individually so the writer sees the full receipt.
4. **Compute the score** using the rubric below.
5. **Output the report** in the Output Format below.

Do not rewrite the prose unless the user asks. The job is detection and scoring, not editing.

## The AI-ism Field Guide

### Sycophant openers (kill on sight)
- "You're absolutely right."
- "Great question."
- "What a fascinating point."
- "Excellent observation."
- "That's an interesting perspective."

### The contrast cliché (strong-negation pattern)
- "It's not X, it's Y."
- "This isn't about X. It's about Y."
- "Don't think of it as X — think of it as Y."
- "X isn't the problem. Y is."

Allowed sparingly (max once per ~1000 words) when the contrast is genuinely the point.

### Lexical tells (specific words LLMs over-reach for)
*delve, navigate, unleash, unlock, embark, journey, harness, foster, leverage, streamline, empower, elevate, tapestry, landscape, realm, symphony, cornerstone, bedrock, robust, comprehensive, holistic, seamless, cutting-edge, game-changing, revolutionary, transformative, paradigm shift, nuanced, multifaceted, intricate*

### Phrasal tells
- "In today's fast-paced world..."
- "It's important to note that..."
- "It's worth mentioning..."
- "In the realm of..." / "In the world of..." / "In the landscape of..."
- "At its core..." / "In essence..." / "Fundamentally..."
- "At the end of the day..."
- "Whether you're X or Y..."
- "Picture this:" / "Imagine for a moment..."
- "Let's dive in." / "Let's explore."
- "Beyond just X..."
- "Not only X, but also Y."

### Structural tells
- **Em-dash overuse:** budget ~5 per ~1000 words. Beyond that, the dashes are doing the work the sentences should.
- **Tricolon abuse:** every list arrives in threes ("clear, concise, and compelling"). Vary list lengths or kill the third item.
- **Listicle parallelism:** bulleted blocks with identical bold lead-ins followed by identical-length descriptions.
- **Bookended summaries:** a closing paragraph that restates the opening with synonyms.
- **Symmetric paragraphs:** three paragraphs of nearly identical length and structure.

### Smug transitions
*indeed, moreover, furthermore, additionally, thus, henceforth, in conclusion, to sum up, in summary*

### Hedge clusters (compounded hedging)
- "may potentially"
- "could possibly"
- "it's worth considering"
- "one might argue that perhaps"

### Generic scene-setting
- "In today's fast-paced world..."
- "At its core..."
- "In essence..."
- "Fundamentally speaking..."

## The Tests

Apply each before scoring:

- **Read-Aloud Test.** Read a paragraph aloud. If the rhythm flattens and your jaw goes slack, it's machine-cadenced.
- **Friend Test.** Would a friend, in conversation, say this sentence? If no, it's been processed.
- **Tally Test.** Count em dashes, lexical tells, contrast clichés. Each has a budget.
- **Cover-the-Author Test.** Hide the byline. Could a top-three LLM have produced this from a one-line prompt? If yes, the voice has dissolved.

## Annotation Format

Quote and tag every instance:

```
[AI-ism: sycophant-opener] "What a fascinating insight..."
[AI-ism: contrast-cliché ×4] "It's not just code. It's craft."
[AI-ism: em-dash-overuse, count=23, budget=5]
[AI-ism: lexical-tell "delve"] → suggest "examine" or "study"
[AI-ism: listicle-parallelism] bullets all begin with bold noun + colon + 2-clause description
[AI-ism: bookend-summary] closing paragraph restates lines 3–7
```

Category tags: `sycophant-opener`, `contrast-cliché`, `em-dash-overuse`, `lexical-tell`, `phrasal-tell`, `listicle-parallelism`, `tricolon-abuse`, `bookend-summary`, `smug-transition`, `hedge-cluster`, `generic-scene-set`, `symmetric-paragraphs`.

## Scoring Rubric (AI-Voice, 1–10)

Higher = more AI. Lower = more human.

| Score | Band | Description |
|-------|------|-------------|
| **1** | GREEN | Unmistakably human. Zero fingerprints. A reader would never suspect machine assistance. |
| **2** | GREEN | Human voice intact. At most one stray tell (e.g. one extra em dash). Good enough. |
| **3** | YELLOW | Mostly human, but a handful of tells are visible. Worth a pass. |
| **4** | YELLOW | Mixed. Several patterns present but the human voice still drives. |
| **5** | YELLOW | Half and half. AI cadence noticeable in multiple paragraphs. |
| **6** | YELLOW | Tipping toward machine. The reader will start to suspect. |
| **7** | RED | Heavily AI-cadenced. Multiple tells per paragraph. Reads like a model with light editing. |
| **8** | RED | Dominantly machine. Sycophant openers, contrast clichés, em-dash storms, lexical tells throughout. |
| **9** | RED | Near-raw LLM output. Cover-the-author test fails everywhere. |
| **10** | RED | Indistinguishable from raw LLM output. Full rewrite required. |

**Band rule of thumb:**
- **GREEN (1–2):** ship it.
- **YELLOW (3–6):** edit pass needed.
- **RED (7–10):** structural rewrite, not just word-swaps.

### Scoring heuristics

Start at 1. Add to the score for each of the following present in the target:

- +1 for each sycophant opener (capped at +2)
- +1 if contrast clichés appear more than once per ~1000 words
- +1 if em dashes exceed budget (~5 per 1000 words)
- +1 for every 2 lexical tells from the list
- +1 if listicle parallelism is the dominant list shape
- +1 if a bookend summary is present
- +1 if smug transitions appear more than twice
- +1 for hedge clusters or generic scene-setting openers
- +1 if the Cover-the-Author Test fails

Cap the score at 10. Round to the nearest integer.

## Output Format

Return the report in this exact shape:

```markdown
# AI-Voice Audit

**Target:** [filename or "pasted text, ~N words"]
**Score:** X/10  [GREEN | YELLOW | RED]
**One-line verdict:** [single sentence — e.g. "Mostly human, but the em-dash count and two contrast clichés give it away."]

## Tally
- sycophant-opener: N
- contrast-cliché: N
- em-dash-overuse: count=N (budget=M)
- lexical-tell: N  (list the words found)
- phrasal-tell: N
- listicle-parallelism: yes/no
- tricolon-abuse: N
- bookend-summary: yes/no
- smug-transition: N
- hedge-cluster: N
- generic-scene-set: N
- symmetric-paragraphs: yes/no

## Findings
[List EVERY AI-ism instance using the Annotation Format above. One line per occurrence. Quote the offending text. Do not collapse repeats — five "delve"s means five lines. Group by category for readability, but do not omit any.]

## Highest-leverage fixes
1. [Most impactful single change to drop the score by 1+ band]
2. [Next most impactful]
3. [Next most impactful]
```

Keep the report tight everywhere except Findings. Findings is exhaustive — every receipt, every time. The other sections stay lean.
