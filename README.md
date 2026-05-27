# Rewrite Skills for Claude

Two skills that help you rewrite text so it sounds like a human wrote it — not an AI.

- **`rewrite`** — one rewrite in a tone you pick (or the original tone).
- **`rewrite-variants`** — 2 or 3 alternative rewrites so you can compare and pick.

Both work in English and German. Both auto-detect the input language. Both translate if you ask.

---

## Installation

1. You should have two folders next to this README:
   ```
   rewrite/
     SKILL.md
   rewrite-variants/
     SKILL.md
   ```
2. Drop both folders into your skills directory for this project.
3. New Claude conversations in this project will pick them up automatically. No restart needed.

If you ever update them, just replace the folder. The skill name in the YAML frontmatter (`name: rewrite`) is what Claude uses internally, so don't rename the folders.

---

## Using `rewrite`

### Quick reference

| Command | What you get |
|---|---|
| `/rewrite <text>` | Same tone as the input, just cleaner. |
| `/rewrite-professional <text>` | Polished, work-appropriate. |
| `/rewrite-friendly <text>` | Warm, conversational. |
| `/rewrite-motivating <text>` | Energetic, forward-looking, no hype. |
| `/rewrite-concise <text>` | Shorter, tighter. |
| `/rewrite-formal <text>` | More formal register. |
| `/rewrite-casual <text>` | More relaxed. |
| `/rewrite-en <text>` | Translate to English and rewrite. |
| `/rewrite-de <text>` | Translate to German and rewrite. |

Any `/rewrite-<suffix>` works if the suffix is interpretable (e.g. `/rewrite-knapp`, `/rewrite-direkt`, `/rewrite-empathisch`).

You can also skip the slash command. Phrasing like *"rewrite this to sound more professional"* or *"mach das mal freundlicher"* triggers the skill too.

### Examples

**Professional:**
> `/rewrite-professional hey just letting you know the report is gonna be late, sorry`
>
> → *The report will be slightly delayed. Apologies for the inconvenience.*

**Friendly:**
> `/rewrite-friendly Please find attached the requested documentation.`
>
> → *Documentation's attached — let me know if anything's unclear.*

**Match original (default):**
> `/rewrite i think we should maybe consider possibly moving the meeting`
>
> → *I think we should move the meeting.*
>
> (Voice stays informal and tentative; only the doubled hedging gets cut.)

**Translation:**
> `/rewrite-en Wir bedauern, Ihnen mitteilen zu müssen, dass Ihre Bewerbung diesmal nicht erfolgreich war.`
>
> → *We're sorry to let you know your application wasn't successful this time.*

---

## Using `rewrite-variants`

### Quick reference

| Command | What you get |
|---|---|
| `/rewrite-variants <text>` | 2 versions in contrasting tones. |
| `/rewrite-options <text>` | Same as above. |
| `/rewrite-3 <text>` | 3 versions. |
| *"give me a few versions of …"* | 2 versions (3 if you say "drei" / "three"). |

You can also request specific tones: *"give me one professional and one diplomatic version of: …"*

### Example

> `/rewrite-3 the new feature is ready for testing pls check by friday`

**Professional**
The new feature is ready for testing. Please review by Friday.

**Friendly**
New feature's ready for testing — could you take a look by Friday?

**Concise**
Feature ready. Test by Friday.

---

## What "sounds human" means here

Both skills filter out the patterns that give AI writing away:

- **No em-dash addiction.** One em-dash in a rewrite is the soft ceiling. No "It's not X — it's Y."
- **No triadic flourishes.** "Clear, concise, and compelling" is the AI fingerprint. One adjective beats three.
- **No filler transitions.** Cuts "Moreover," "Furthermore," "Darüber hinaus," "Folglich."
- **No corporate-AI vocabulary.** Avoids *delve, leverage, robust, seamlessly, comprehensive, nahtlos, umfassend, ganzheitlich.*
- **No wrap-up sentences.** Cuts "In conclusion," "Overall."
- **Varied sentence length.** Short sentences allowed. Fragments occasionally fine.

The skills also preserve all concrete facts — names, dates, numbers, URLs — across every rewrite.

---

## Tips

**Order matters.** The command goes first, the text after. `/rewrite-professional` then your text. Putting the command at the end won't work reliably.

**Already-good input.** The skill won't impose a rewrite for its own sake. If your text is already clean, expect small touches rather than a full makeover. That's intentional.

**Code blocks survive.** If your input mixes prose and a code snippet, only the prose gets rewritten. The code is left alone.

**Lists stay lists.** Bullet points and numbered lists keep their structure. Only the wording of each item changes.

**Tone tuning.** If "friendly" feels too casual or "professional" feels too stiff for your context, just say so: *"a bit warmer than that"*, *"slightly less formal"*. The skill picks it up.

---

## Troubleshooting

**The skill didn't trigger.**
Make sure the slash command is at the very start of your message. If you're using a natural-language phrasing, include the word "rewrite" (or *umschreiben / überarbeiten*).

**Output included a preamble like "Here's the rewrite:".**
That's a skill miss. Send it back with: *"just the rewrite, no preamble."* If it happens often, the description in SKILL.md may need tightening.

**Variants sound too similar.**
Ask for sharper contrast: *"make them more different"* or specify tones directly: *"one punchy, one detailed."*

**German rewrite sounds stiff.**
Tell the skill: *"natürlicher, weniger akademisch."* The most common slip is keeping `darüber hinaus` or `folglich` — both should disappear.

---

## Editing the skills

Open `SKILL.md` in either folder. The frontmatter (between the `---` markers) controls when the skill triggers. The body is the instructions Claude follows once triggered.

Useful places to edit:

- **Add a new tone suffix.** In `rewrite/SKILL.md`, add a row to the command table in *Step 1*.
- **Adjust the AI-tell list.** Both skills have an "Avoid" block. Add words or constructions you personally want filtered.
- **Change the default variant count.** In `rewrite-variants/SKILL.md`, adjust the count in *Step 1*.

After editing, replace the folder in your skills location. Changes take effect on the next conversation.