---
name: rewrite-variants
description: >
  Produces 2 or 3 differently-styled rewrites of the user's text so they can pick or
  mix. Use whenever the user asks for multiple stylistic versions of a piece of text.
  Triggers on /rewrite-variants, /rewrite-options, /rewrite-2, /rewrite-3, or phrases
  like "give me a few versions", "show me different variants", "different tones",
  "a couple of options", "verschiedene Versionen", "mehrere Varianten", "in
  verschiedenen Stilen", "ein paar Varianten". Default to 2 variants; produce 3 only
  when explicitly asked ("3 versions", "drei Varianten", "/rewrite-3"). Auto-detects
  input language; translates if the user specifies a target language.
---

# Rewrite Variants

Produce 2 (default) or 3 (on request) genuinely different rewrites of the user's text. Each variant must read like a real person wrote it, and the variants must differ in tone — not just word swaps.

## Workflow

### Step 1 — Parse the command and count

| Command / Phrase | Count |
|---|---|
| `/rewrite-variants`, `/rewrite-options`, `/rewrite-2` | 2 |
| `/rewrite-3`, "3 versions", "drei Varianten" | 3 |

If the user named specific tones ("one professional and one friendly", "eine knappe und eine ausführliche"), use those. Otherwise pick contrasts in Step 2.

### Step 2 — Pick the tones

Pick contrasts that are meaningfully different *and* useful for this kind of text. Don't always pick the same three.

| Text type | Suggested contrast |
|---|---|
| Messages and emails | **Professional** vs **Friendly** |
| Short replies | **Concise** vs **Warm** |
| Difficult news or pushback | **Direct** vs **Diplomatic** |
| Marketing, announcements, social | **Punchy** vs **Detailed** |
| General writing, unclear context | **Professional** vs **Friendly** (+ **Concise** if 3 requested) |

### Step 3 — Detect language

Default: each variant in the input language. Translate only if the user specifies a target language.

### Step 4 — Generate each variant

Apply the chosen tone, following the "Sound human" rules below. Each variant is independent — write it as if it were the only one.

### Step 5 — Self-check before output

Before sending, verify:

- [ ] **Are the variants actually different?** Read both side by side. If they sound 80% the same, rewrite the second one with more contrast. Differ in sentence structure, word choice, stance, or length.
- [ ] **No AI tells in any variant.** Scan each for: em-dash addiction, "not just X — it's Y", triadic flourishes, *delve / leverage / robust / seamlessly / nahtlos / umfassend*, "Moreover / Furthermore / Darüber hinaus", wrap-ups, hedging boilerplate.
- [ ] **Facts preserved across all variants.** Names, dates, numbers, places, URLs identical in every version.
- [ ] **Output format clean.** Bold label, then text, then blank line. No commentary like "this version emphasizes…".

If anything fails, fix before sending.

## Output format

For each variant: bold label on its own line, then the text. Blank line between variants. Nothing else.

```
**Professional**
<the rewritten text>

**Friendly**
<the rewritten text>
```

For German output, label in German: **Professionell**, **Freundlich**, **Knapp**, **Motivierend**, **Direkt**, **Diplomatisch**.

## Sound human (applies to every variant)

### Avoid

- Em-dash addiction and the "not just X — it's Y" construction
- Triadic adjective stacks ("clear, concise, and compelling")
- Filler transitions: *Moreover, Furthermore, Additionally, Darüber hinaus, Folglich, Letztendlich*
- AI vocabulary: *delve, navigate, leverage, robust, comprehensive, crucial, vital, seamlessly, elevate, unlock, harness, foster, empower, streamline, nahtlos, umfassend, ganzheitlich, essentiell*
- Wrap-up sentences and hedging boilerplate
- Identical sentence shapes within a single variant

### Do

- Vary sentence length; use contractions where natural
- Concrete nouns and verbs, active voice
- Cut filler ("in order to" → "to")
- One idea per sentence

## Making variants actually different

Variants should differ along at least one strong axis:

- **Sentence structure** — long flowing vs short staccato
- **Word choice** — formal vocabulary vs everyday words
- **Stance** — warm and personal vs neutral and informational
- **Length** — one variant noticeably shorter than another is fine and often good

It's normal for one variant to be 30 words and another to be 12.

## Edge cases

**No text after the command**
→ Reply: "What should I rewrite?" Nothing else.

**Very short input** (under 10 words)
→ Still produce variants, but each should stay short. Don't pad them to differentiate.

**User specifies tones**
→ Use exactly those, even if unusual. Don't substitute.

**User asks for variants of something Claude just wrote**
→ Treat Claude's prior message as the input.

**Input with bullet points, lists, or code**
→ Keep that structure in every variant. Only the prose changes.

## Examples

**Input** (default — 2 variants):
> Unfortunately we have decided not to move forward with your application at this time. Thank you for your interest.

**Output**:

**Professional**
We've decided not to move forward with your application. Thank you for taking the time to apply.

**Warm**
After a lot of thought, we've decided not to move forward this time. We appreciated the chance to learn about your work and wish you the best with what comes next.

---

**Input** (`/rewrite-3`):
> the new feature is ready for testing pls check by friday

**Output**:

**Professional**
The new feature is ready for testing. Please review by Friday.

**Friendly**
New feature's ready for testing — could you take a look by Friday?

**Concise**
Feature ready. Test by Friday.

---

**Input** (German, 2 variants):
> Wir müssen leider den Liefertermin verschieben.

**Output**:

**Professionell**
Wir müssen den Liefertermin leider verschieben. Einen neuen Termin teilen wir Ihnen zeitnah mit.

**Direkt**
Der Liefertermin verschiebt sich. Neuen Termin schicke ich nach.

---

**Input** ("give me two versions, one direct and one diplomatic: I can't make Friday's meeting"):

**Output**:

**Direct**
I can't make Friday's meeting.

**Diplomatic**
Friday's not going to work for me — could we find another time? Happy to suggest a few slots that work.