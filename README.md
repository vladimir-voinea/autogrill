# autogrill

An agent skill that drives a vague idea end to end — research, grilling, spec, and tracer-bullet tickets — with **no human in the loop**.

Autogrill runs `wayfinder → to-spec → to-tickets` to completion: it asks the grilling questions and answers them itself (as recorded *proxy answers* you can audit later), resolves each wayfinder ticket in a fresh subagent, and escalates only what genuinely needs a human (credentials, money, irreversible side effects, uncovered taste calls).

The run ends with:

- a fully resolved **wayfinder map**
- a published **spec** (itself a ticket on the map)
- **tracer-bullet tickets** on the tracker, with acceptance criteria and blocking edges, ready to implement as-is
- for an **iOS app**, a final **deployment ticket** that ships it: App Store Connect listing complete, build uploaded to TestFlight, the user invited to install it, and the version one click from Submit for Review

Everything lives in the issue tracker — a run leaves no files on disk.

## Install

```bash
npx skills add vladimir-voinea/autogrill
```

Then invoke it in your agent:

```
autogrill A vague idea, plus any constraints (stack, deadline, budget, taste, forbidden moves)
```

The human's only input is the **constraints** given at invocation. They're recorded verbatim in the map's Notes, and every proxy answer must honor them.

## What it depends on

Autogrill orchestrates sibling skills — `wayfinder`, `to-spec`, `to-tickets`, `grilling`, `prototype`, `research`, `domain-modeling` — and an issue tracker. If a sibling can't be invoked programmatically, it reads its `SKILL.md` and follows it.

An iOS idea additionally depends on `appstore-publish`: the final deployment ticket names it, and whoever works the ticket loads and follows it.

## The iOS ending

An iOS run ends with a **deployment ticket** blocked by every other ticket in the set. Its acceptance criteria are `appstore-publish`'s ready-to-submit checklist: the app registered on App Store Connect, the listing complete, the build in TestFlight, the user invited and installed, and the version one click from Submit for Review. The human's steps — ASC credentials, the Paid Apps agreement, the app record Apple only creates in a browser, the final Submit click — are recorded in the ticket rather than guessed.

## Proxy answers & escalation

Wherever the underlying skills demand a human decision, autogrill answers as the user's stand-in and writes the reasoning on the record: *"proxy: X, because the constraint Y"* — never a fabricated human voice. Questions that can't be guessed (credentials, payments, irreversible effects) are escalated: the ticket stays open with the exact question, options, and a recommendation, while everything not downstream keeps moving.

## License

MIT
