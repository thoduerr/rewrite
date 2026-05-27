---
name: rewrite
description: >
  Rewrites text so it sounds human — no AI tells. Single rewrite (default) or
  multiple stylistic variants. Triggers on /rewrite and any /rewrite-<suffix>
  (e.g. -professional, -friendly, -concise, -variants, -3, -en, -de), and on
  natural phrasing like "rewrite this", "polish this", "make this friendlier",
  "give me a few versions", or German equivalents ("umschreiben",
  "überarbeiten", "freundlicher", "mehrere Varianten"). Auto-detects language;
  translates on request.
---

# Rewrite

Take the user's text and rewrite it so it reads like a real person wrote it — not an AI. The skill has two modes routed by command: **single rewrite** (the default) and **variants** (two or three contrasting versions). The text after the command is the input. The rewrite(s) are the response.

## Workflow

### Step 1 — Identify mode and parse the command

Look at the first token of the message.

**Single rewrite mode:**

| Command | Tone |
|---|---|
| `/rewrite` (or just "rewrite this: …") | **Match original** — keep the writer's voice, fix only awkwardness, redundancy, or unclear phrasing. Don't smooth out personality. |
| `/rewrite-professional` | **Professional** — clear, polished, suitable for work emails, reports, customer-facing copy. |
| `/rewrite-friendly` | **Friendly** — warm, conversational, contractions fine. |
| `/rewrite-motivating` | **Motivating** — energetic, forward-looking, encouraging. No hype words. |
| `/rewrite-<other>` | Interpret the suffix sensibly: `-casual`, `-concise`, `-formal`, `-knapp`, `-direkt`, etc. |

**Variants mode:**

| Command / Phrase | Count |
|---|---|
| `/rewrite-variants`, `/rewrite-options`, `/rewrite-2` | 2 |
| `/rewrite-3`, "3 versions", "drei Varianten" | 3 |

If there's no slash command but the user clearly asked for a rewrite, default to single rewrite, match-original tone. If they asked for "a few versions" or similar, use variants mode with 2.

### Step 2 — Detect the language

Default: rewrite in the same language as the input. Switch only if the user explicitly asks:
- `/rewrite-en`, `/rewrite-de`
- "translate to English and polish", "in Englisch umschreiben", "auf Deutsch"

When translating, translate first, then apply the rules in the target language.

### Step 3 — For variants mode: pick contrasting tones

Pick contrasts that are meaningfully different *and* useful for this kind of text. Don't always pick the same three.

| Text type | Suggested contrast |
|---|---|
| Messages and emails | **Professional** vs **Friendly** |
| Short replies | **Concise** vs **Warm** |
| Difficult news or pushback | **Direct** vs **Diplomatic** |
| Marketing, announcements, social | **Punchy** vs **Detailed** |
| General writing, unclear context | **Professional** vs **Friendly** (+ **Concise** if 3 requested) |

If the user named specific tones ("one professional and one diplomatic"), use those exactly. For German input, apply the same contrast logic and use the German labels (see Output rules).

Write each variant independently — as if it were the only one. Variants should differ along at least one strong axis: sentence structure (long flowing vs short staccato), word choice (formal vs everyday), stance (warm and personal vs neutral and informational), or length (one variant noticeably shorter than another is fine and often good). It's normal for one variant to be 30 words and another to be 12.

### Step 4 — Generate and self-check

Apply the chosen tone. Scan the draft and fix any of these before output:

- [ ] **"Not just X — it's Y" construction** — the strongest em-dash tell. Also: more than one em-dash in a short rewrite.
- [ ] **Triadic adjective stacks** ("clear, concise, and compelling")
- [ ] **Filler transitions**: *Moreover, Furthermore, Additionally, Darüber hinaus, Folglich, Letztendlich*
- [ ] **AI vocabulary**: *delve, navigate, leverage, robust, comprehensive, crucial, vital, seamlessly, elevate, unlock, harness, foster, empower, streamline, resonate, embark, tapestry, testament, realm, multifaceted, intricate, paramount, nahtlos, umfassend, ganzheitlich, essentiell, facettenreich, tiefgreifend, beleuchten*
- [ ] **Wrap-up sentences** ("In conclusion", "Overall", "To sum up")
- [ ] **Hedging boilerplate** ("It's worth noting that", "Es ist wichtig zu erwähnen")
- [ ] **Decorative emoji** at end of sentence (✨, 🚀, 🎯) — strip unless the input already had them.
- [ ] **Semicolon overuse** — AI chains clauses with semicolons. Prefer periods or short sentences.
- [ ] **Every sentence the same shape** — vary length and structure
- [ ] **Made-up content** — did you add information the writer didn't include? Remove it.

**For variants mode, also check:**
- [ ] **Are the variants actually different?** Read them side by side. If two sound 80% the same, rewrite one with more contrast (sentence structure, word choice, stance, or length).
- [ ] **Facts preserved across all variants** — names, dates, numbers, URLs identical in every version.

## Sound human

The goal is not "polished writing." It's writing that doesn't feel like an AI wrote it. Step 4 has the operational checklist of what to strip out. This section captures the underlying intent and what to do *instead*.

### Do

- **Vary sentence length** — mix short and long, one short sentence can carry a lot
- **Use contractions** when natural ("don't", "we're", "it's")
- **Concrete over abstract** — "three customers complained" beats "stakeholder feedback indicated concerns"
- **Active voice** unless passive serves a purpose
- **Cut filler** — "in order to" → "to", "due to the fact that" → "because", "at this point in time" → "now"
- **One idea per sentence** — don't chain three clauses with semicolons
- **Keep the writer's quirks** when matching original tone — small imperfections sound human

### German specifically

Avoid *nahtlos*, *umfassend*, *ganzheitlich*, *facettenreich*, *tiefgreifend*, *im Rahmen von*. Use natural connectors (*aber*, *denn*, *also*) instead of stiff academic ones (*darüber hinaus*, *folglich*, *infolgedessen*).

## Output rules

**Single rewrite mode:**
- Output the rewritten text only. No "Here's a more professional version:", no preamble, no closing note.

**Variants mode:**
- For each variant: bold label on its own line, then the text. Blank line between variants. Nothing else — no commentary like "this version emphasizes…".
- For German output, label in German: **Professionell**, **Freundlich**, **Knapp**, **Motivierend**, **Direkt**, **Diplomatisch**.

```
**Professional**
<the rewritten text>

**Friendly**
<the rewritten text>
```

**Both modes:**
- Keep the paragraph structure of the input. Don't introduce bullet points or headers if the input was prose.
- Preserve concrete facts: names, dates, numbers, places, URLs.
- Don't add information the writer didn't include.
- If the input had code blocks, leave the code untouched and rewrite only the surrounding prose.

## Edge cases

**No text after the command** (just `/rewrite-professional` on its own)
→ Reply: "What should I rewrite?" Nothing else.

**Very short input** (under 10 words)
→ Still rewrite, but stay similarly short. Don't expand a one-liner into a paragraph. In variants mode, each variant should stay short too.

**Input that's already well-written**
→ Make minimal changes. Don't impose a rewrite for its own sake.

**Input with bullet points or lists**
→ Keep the list structure. Rewrite the items themselves.

**Input mixing prose and code**
→ Rewrite only the prose. Leave code blocks identical.

**Input embedded in chatter** (`"can you rewrite this for me: <text> thanks!"`)
→ Strip the surrounding chatter. Rewrite only the target text. Don't reply to the chatter.

**Long input** (more than ~50 words)
→ Track input length. Don't balloon a 50-word note into 90 unless the tone explicitly demands expansion (`-detailed`). For `-concise`, shorten; otherwise keep length roughly proportional.

**Obvious typos**
→ Preserve proper nouns as written (could be intentional spelling: a name, a product). Fix obvious typos in common words unless they read as stylistic ("gonna", "kinda" stay).

**Ambiguous suffix** (`/rewrite-x` where x is unclear)
→ Pick the most plausible interpretation and proceed. Don't ask.

**User asks for variants of something Claude just wrote**
→ Treat Claude's prior message as the input.

## Examples

### Single rewrite

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

**Input** (`/rewrite`, no suffix — match original):
> i think we should maybe consider possibly moving the meeting to thursday because monday is going to be really really busy for most of the team

**Output**:
> I think we should move the meeting to Thursday. Monday's going to be packed for most of the team.

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

### Variants

**Input** (`/rewrite-variants`, default 2):
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

**Input** (user-specified tones):
> "give me one direct and one diplomatic version of: I can't make Friday's meeting"

**Output**:

**Direct**
I can't make Friday's meeting.

**Diplomatic**
Friday's not going to work for me — could we find another time? Happy to suggest a few slots that work.