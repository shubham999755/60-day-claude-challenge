# What I Learned — Prior Authorization Workflow Simulator

A reflection on building a gamified, drag-and-drop simulation of the US healthcare
Prior Authorization (PA) process as a single self-contained HTML file.

## Domain: How Prior Authorization Actually Works

- PA is a three-party workflow — **Patient**, **Provider**, and **Payer** — and
  friction shows up at every handoff between them, not just inside one team.
- A request isn't binary. Payers return one of several outcomes: **Approval**,
  **Pend** (more info needed), or **Denial** — and a denial isn't the end of the
  road; providers can pursue a **formal appeal** or a **peer-to-peer review**
  (a direct clinician-to-clinician conversation with the payer's medical director).
- **Medical necessity** criteria and required documentation differ by service
  type. An MRI, a specialty drug, an elective surgery, and an inpatient
  admission each have their own evidentiary bar (e.g. step therapy for
  specialty meds, conservative-treatment history for surgery, level-of-care
  criteria for inpatient stays).
- Document completeness is one of the biggest levers on outcome. Missing a
  single required item is a common, avoidable cause of pends and denials —
  which is exactly why the simulator scores outcomes off a "required docs
  collected" ratio rather than a flat coin flip.
- Every extra cycle (pend response, appeal, peer-to-peer) has a real time
  cost. Modeling "days elapsed" made the administrative burden of PA visible
  in a way a static description doesn't.

## Technical: Building It as a Single Dependency-Free HTML File

- **Pointer Events beat native HTML5 Drag-and-Drop for this use case.** Native
  `dragstart`/`dragover`/`drop` doesn't work reliably on touch devices. Using
  `pointerdown` / `pointermove` / `pointerup` with `setPointerCapture` gave one
  code path that works for mouse *and* touch, at the cost of manually managing
  element reparenting and drop-target detection via `elementFromPoint`.
- **Always ship a non-drag fallback.** A "tap to arm, tap the destination"
  click path makes the same interaction accessible to keyboard/switch users
  and anyone whose input device makes drag gestures awkward, without
  duplicating the underlying state-transition logic.
- **A small state machine keeps a "gamified" flow honest.** Modeling
  `currentStage` + `allowedTargets` explicitly (rather than letting the UI
  imply what's next) made it easy to guarantee the user can never skip a step
  or drop a case somewhere invalid — invalid drops just shake and bounce back.
- **Separate the outcome *engine* from the outcome *presentation*.** Keeping
  the weighted-random decision logic (`runInitialReview` / `runSecondReview`)
  in pure functions, independent of the stamp animation and modal wiring,
  made the risk math easy to reason about and tune independently of the UI.
- **Constraints can become the design language.** The brief called for "shades
  of blue with black text," which ruled out the usual red/green
  approve-deny convention. Leaning into that constraint — distinguishing
  outcomes by rubber-stamp icon and border style instead of hue — ended up
  feeling more true to an actual insurance form than a full-color UI would have.
- **In-memory state is a genuine constraint, not just a simplification.** With
  `localStorage` off the table, every reset had to be a real, explicit
  teardown (`fullReset()`) — clearing timers, DOM nodes, and disabled zones —
  rather than relying on a page reload to clean up.
- **Confetti and stamp animations are cheap in vanilla JS.** No canvas or
  animation library needed — a handful of absolutely-positioned divs with
  randomized CSS keyframe delays is enough for a convincing celebration effect.

## Product / Gamification

- Educational value and game feel don't have to compete: pairing every state
  transition with a one- or two-sentence "why this step exists" explanation
  turned the simulator into a teaching tool without slowing down the
  interaction loop.
- A visible **efficiency score** (penalized by extra days, pends, and denials)
  gives the player an incentive to actually gather complete documentation up
  front — reinforcing the real-world lesson, not just decorating the UI.
- Ending on a **summary screen with a full path log** matters as much as the
  celebration moment — it's what turns "I got an outcome" into "I understand
  why I got this outcome."

## Possible Next Steps

- Add a difficulty/payer-policy toggle (e.g. stricter vs. lenient payer) to
  vary risk profiles without touching the core engine.
- Track stats across multiple patients in a single session (still in memory)
  to compare efficiency scores scenario-to-scenario.
- Expand the appeal path with a mini interaction for building the appeal
  argument itself, mirroring the document-collection mechanic.
