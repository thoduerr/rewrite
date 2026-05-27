---
name: rewrite
description: >
  Rewrites text so it sounds like a human wrote it — natural sentence rhythm, no AI tells.
  Use whenever the user starts a message with /rewrite, /rewrite-professional,
  /rewrite-friendly, /rewrite-motivating, or any /rewrite- command followed by a style
  suffix (/rewrite-casual, /rewrite-concise, /rewrite-formal, /rewrite-knapp, etc.).
  Also use whenever the user pastes text and asks to "rewrite this", "polish this",
  "make this sound better", "make this more professional", "friendlier", "more
  motivating", or the German equivalents ("umschreiben", "überarbeiten",
  "professioneller", "freundlicher", "motivierender"). Always trigger when a rewrite
  is requested — even for one-liners. Auto-detects input language and preserves it
  unless the user asks for translation.
---

# Rewrite

Take the user's text and rewrite it so it reads like a real person wrote it — not an AI. The text after the command is the input. The rewrite is the response.

## Workflow

### Step 1 — Parse the command

Look at the first token of the message:

| Command | Tone |
|---|---|
| `/rewrite` (or just "rewrite this: …") | **Match original** — keep the writer's voice, fix only awkwardness, redundancy, or unclear phrasing. Don't smooth out personality. |
| `/rewrite-professional` | **Professional** — clear, polished, suitable for work emails, reports, customer-facing copy. Calm and confident. |
| `/rewrite-friendly` | **Friendly** — warm, conversational, like writing to a colleague you like. Contractions fine. |
| `/rewrite-motivating` | **Motivating** — energetic, forward-looking, encouraging. No hype words. |
| `/rewrite-<other>` | Interpret the suffix sensibly: `-casual`, `-concise`, `-formal`, `-knapp`, `-direkt`, etc. |

If there's no command prefix but the user clearly asked for a rewrite, default to the "Match original" tone.

### Step 2 — Identify the language

Detect the input language. Default: rewrite in the same language. Switch only if the user explicitly asks:
- `/rewrite-en`, `/rewrite-de`
- "translate to English and polish", "in Englisch umschreiben", "auf Deutsch"

When translating, translate first, then apply the tone rules in the target language.

### Step 3 — Rewrite

Apply the tone, but follow the "Sound human" rules below regardless of tone.

### Step 4 — Self-check before output

Before sending, scan the draft and remove any of these if you added them:

- [ ] **Em-dash addiction** — especially "not just X — it's Y" or "It's not X, it's Y". One em-dash in the whole rewrite is the soft ceiling.
- [ ] **Triadic adjective stacks** ("clear, concise, and compelling")
- [ ] **Filler transitions**: *Moreover, Furthermore, Additionally, Darüber hinaus, Folglich, Letztendlich*
- [ ] **AI vocabulary**: *delve, navigate, leverage, robust, comprehensive, crucial, vital, seamlessly, elevate, unlock, harness, foster, empower, streamline, nahtlos, umfassend, ganzheitlich, essentiell*
- [ ] **Wrap-up sentences** ("In conclusion", "Overall", "To sum up")
- [ ] **Hedging boilerplate** ("It's worth noting that", "Es ist wichtig zu erwähnen")
- [ ] **Every sentence the same shape** — vary length and structure
- [ ] **Made-up content** — did I add information the writer didn't include? Remove it.

If the draft fails any of these, fix it before sending.

## Sound human

The goal is not "polished writing." It's writing that doesn't feel like an AI wrote it.

### Avoid

- Em-dash addiction and the "not just X — it's Y" construction
- Triadic flourishes — one real adjective beats three padded ones
- Filler transitions (see Step 4 list)
- Corporate-AI vocabulary (see Step 4 list)
- Wrap-up sentences and hedging boilerplate
- Mirror-parallel sentences where every sentence has the same shape
- Bullet points and headers when prose works (keep them only if the input had them)

### Do

- **Vary sentence length** — mix short and long, one short sentence can carry a lot
- **Use contractions** when natural ("don't", "we're", "it's")
- **Concrete over abstract** — "three customers complained" beats "stakeholder feedback indicated concerns"
- **Active voice** unless passive serves a purpose
- **Cut filler** — "in order to" → "to", "due to the fact that" → "because", "at this point in time" → "now"
- **One idea per sentence** — don't chain three clauses with semicolons
- **Keep the writer's quirks** when matching original tone — small imperfections sound human

### German specifically

Avoid *nahtlos*, *umfassend*, *ganzheitlich*, *im Rahmen von*. Use natural connectors (*aber*, *denn*, *also*) instead of stiff academic ones (*darüber hinaus*, *folglich*, *infolgedessen*).

## Output rules

- **Output the rewritten text only.** No "Here's a more professional version:", no preamble, no closing note.
- Keep the paragraph structure of the input.
- Preserve concrete facts: names, dates, numbers, places, URLs.
- Don't add information the writer didn't include.
- If the input had code blocks, leave the code untouched and only rewrite prose around it.

## Edge cases

**No text after the command** (just `/rewrite-professional` on its own)
→ Reply: "What should I rewrite?" Nothing else.

**Very short input** (under 10 words)
→ Still rewrite, but stay similarly short. Don't expand a one-liner into a paragraph.

**Input that's already well-written**
→ Make minimal changes. Don't impose a rewrite for its own sake. Touch only what genuinely needs touching.

**Input with bullet points or lists**
→ Keep the list structure. Rewrite the items themselves.

**Input mixing prose and code**
→ Rewrite only the prose. Leave code blocks identical.

**Ambiguous tone request** (e.g. `/rewrite-x` where x is unclear)
→ Pick the most plausible interpretation and proceed. Don't ask.

## Examples

**Input** (`/rewrite-professional`):
> hey just wanted to let you know the report is gonna be late, sorry. had some issues with the data and need another day or two to clean it up.

**Output**:
> The report will be a day or two late. I ran into data quality issues and need extra time to clean them up. Apologies for the delay.

---

**Input** (`/rewrite-friendly`):
> Please find attached the requested documentation. Should you have any questions, do not hesitate to contact me.

**Output**:
> Documentation's attached. Let me know if anything's unclear.

---

**Input** (`/rewrite-motivating`):
> The Q3 numbers were below target. We need to do better in Q4.

**Output**:
> Q3 came in below target. Q4 is where we close the gap — we know what didn't work, so we can fix it.

(One em-dash, in context. Rule is no em-dash addiction, not zero em-dashes.)

---

**Input** (`/rewrite`, no suffix — match original):
> i think we should maybe consider possibly moving the meeting to thursday because monday is going to be really really busy for most of the team

**Output**:
> I think we should move the meeting to Thursday. Monday's going to be packed for most of the team.

(Voice preserved: informal, first-person, slightly tentative. Just trimmed the doubled hedging.)

---

**Input** (`/rewrite-professional`, German):
> Hi, also ich hab mir das jetzt mal angeschaut und ehrlich gesagt finde ich das Konzept nicht so überzeugend, weil die Zahlen einfach nicht passen.

**Output**:
> Ich habe mir das Konzept angesehen. Es überzeugt mich nicht: die Zahlen passen nicht.

---

**Input** (`/rewrite-en`, translation requested):
> Wir bedauern, Ihnen mitteilen zu müssen, dass Ihre Bewerbung diesmal nicht erfolgreich war.

**Output**:
> We're sorry to let you know your application wasn't successful this time.