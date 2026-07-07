# What I Learned — Prior Authorization Story Simulator

A reflection on building a narrative, chat-style walkthrough of the Prior
Authorization (PA) process as a single self-contained HTML file.

## Domain: The Human Side of Prior Authorization

- Turning the same PA process into a **story with two characters** (patient
  and healthcare operations specialist) surfaced something a flowchart
  doesn't: PA isn't just steps, it's a conversation full of reasonable
  questions — "why does my insurance need to approve what my doctor already
  gave me?" is the whole friction point in one line.
- **Step therapy** is easy to dismiss as red tape, but framing it against an
  aggressive diagnosis (Rheumatoid Arthritis) made the real stakes clearer:
  for some conditions, the delay itself — not just the paperwork — can affect
  disease progression.
- A denial is a *data point*, not a dead end. Naming the specific missing
  piece (step-therapy documentation) turns "denied" from a scary word into an
  actionable next step — that reframing is probably the single most useful
  thing the story teaches.
- The **system side has its own cost**, separate from the patient's
  experience: an AMA-cited majority of PA requests see delays, and a denial
  can burn 2+ staff hours for a physician's office to resolve. Showing both
  the patient's emotional beat and the operations-side metric in the same
  scene made the two-sided cost of the process tangible.
- Ending on two separate takeaway columns (patient vs. system) rather than
  one merged list was worth the extra layout work — it kept "what Rahul
  learned" and "what a health system tracks" (denial rate, appeal rate,
  resolution time) from blurring into generic advice.

## Technical: A Story Engine Instead of a Workflow Engine

- **`createElement` + `appendChild` only, no `innerHTML`, is a real
  constraint worth respecting literally** — including on restart. It would
  have been easy to reset the chat with `chatFeed.innerHTML = ''`; instead,
  restart removes children one at a time (`while (firstChild) removeChild`),
  which keeps the "append-only, DOM-safe" guarantee true for the entire
  lifecycle of the page, not just the happy path.
- **Data-driven scenes beat hand-written DOM calls.** Modeling each scene as
  an array of typed `lines` (`narrator`, `doctor`, `rahul`, `priya`, `stat`)
  plus a `choices` array let one small `renderLine()` dispatcher handle all
  eight scenes, instead of eight bespoke blocks of near-duplicate code.
- **Mixed text + inline badges without `innerHTML` needs its own helper.**
  Labeling "StarCare Health" as an illustrative example everywhere it's
  mentioned meant building paragraphs out of alternating text nodes and
  small `<span>` badges (`appendRichText()`), rather than the far simpler
  (but disallowed) approach of building a string with HTML tags in it.
- **Branching dialogue doesn't require branching plot.** Both choices in a
  scene lead to the same next scene, but each appends its own short reaction
  lines first. That's enough to make a choice feel consequential (different
  dialogue, different character tone) without the combinatorial complexity
  of a true branching story tree — a good scope trade-off for an 8-scene
  educational piece.
- **`await` + a `wait(ms)` promise helper made staggered dialogue trivial.**
  Sequencing "narrator line → pause → doctor line → pause → choice buttons"
  as an `async` function reads almost like a script, which made it much
  easier to keep the pacing of an 8-scene story consistent than chaining
  nested `setTimeout` callbacks would have been.
- **Tailwind via CDN plus a small custom `<style>` block is a reasonable
  middle ground.** Tailwind's utility classes handled almost all layout and
  color, but `tailwind.config`'s `theme.extend.colors` was needed to keep the
  same blue palette as the companion workflow simulator, and a few
  hand-written keyframes (`stampIn`, `confettiFall`, `fadeUp`) covered the
  handful of things utility classes don't do well.

## Design System Consistency Across Two Apps

- Reusing the **"stamp" motif** (solid border = approved, dashed border =
  denied) from the earlier workflow simulator — instead of inventing new
  green/red states — kept both apps visually and conceptually part of the
  same series, and kept the palette honestly restricted to blue shades with
  black text.
- The **citation callout** (dashed border, monospace source line) borrowed
  the "form/data" visual language from the first app's case-card styling,
  which made the AMA statistic read as a sourced fact rather than a stray
  quote in a chat bubble.
- Consistent character-side conventions (patient always left, specialist
  always right, doctor/narrator always centered italic, never a bubble) are
  a small rule that did a lot of work — it meant a reader could tell who was
  "speaking" at a glance, scene after scene, without re-reading names.

## Possible Next Steps

- Let a choice actually change downstream content (e.g., picking "skip
  ahead" in Scene 2 slightly increases the chance of extra pend-style
  friction later), for lightweight replayability.
- Add a "glossary" side panel for terms like ICD-10, step therapy, and
  Letter of Medical Necessity, linked from their first mention.
- Track and display a running "questions asked" count as a light gamified
  layer, similar to the efficiency score in the companion workflow simulator.
