---
name: autogrill
description: Drive a vague idea end to end — wayfinder map fully resolved, spec published, tracer-bullet tickets ready — with no human in the loop. Use when the user wants an idea planned to implementable tickets autonomously, or names autogrill.
disable-model-invocation: true
argument-hint: "A vague idea, plus any constraints"
---

A vague idea arrives. Autogrill runs **wayfinder → to-spec → to-tickets** to completion without a human at the wheel: it asks the grilling questions and answers them too, and resolves each wayfinder ticket in a fresh **subagent**, which is what dissolves wayfinder's one-ticket-per-session limit — the subagent *is* the session. The run ends with the map fully resolved, the spec published, and tracer-bullet tickets on the tracker that the user can implement as-is.

The human's only input is the **constraints** given at invocation (stack, deadline, budget, taste, forbidden moves). Record them verbatim in the map's Notes; every answer below must honor them.

Invoke the sibling skills — `wayfinder`, `to-spec`, `to-tickets`, `grilling`, `prototype`, `research`, `domain-modeling` — via the Skill tool; if one can't be invoked programmatically, read its SKILL.md and follow it. Sibling rules bind autogrill *except* where this file explicitly substitutes one thing for another: the human.

**No files on disk.** The entire run — the map, every decision record, the spec, and the tickets — lives in the issue tracker. Nothing is written to disk, no scratch branch, no checked-in artifact. The spec is itself a wayfinder ticket on the map, so the whole effort is readable in the tracker and a run leaves no files behind.

## Proxy answers

Everywhere the siblings demand a human — a HITL ticket, a grilling round, to-spec's seam check, to-tickets' breakdown quiz — answer as the user's **stand-in**: pick the answer the constraints point to, and record it as a **proxy answer**, reasoning and all, so the human can audit it later. A proxy answer is the agent's judgment on the record, never a fabricated human voice: write "proxy: X, because the constraint Y", never "the user wants X".

When a question genuinely cannot be answered without the human — credentials, irreversible external side effects, money, a taste call the constraints don't cover — **escalate** it: leave the ticket open, post the exact question with the options and your recommendation, and keep working every ticket not downstream of it. Escalation is a legitimate end state; a guessed answer to an unguessable question is not.

## The run

Four stages, in order. Do not enter the next until the current one's completion criterion holds.

### 1. Chart

Run wayfinder's *Chart the map* mode, with each grilling round replaced by a proxy round: ask the frontier, answer it yourself from the constraints and the codebase, dispatch research subagents for facts, and let the design tree expand. No fog found — the whole journey fits one session — means no map is needed: skip to stage 3 and spec the idea directly.

**Done when:** the map exists with Destination, Notes carrying the constraints, first tickets created and blocking edges wired.

### 2. Work the map to zero

Loop while open child tickets remain:

1. Take the **frontier** (open, unblocked, unclaimed). Dispatch **is** the claim: the subagent assigns the ticket to itself as its very first action before any work, so concurrent runs never double-up.
2. Dispatch one subagent per frontier ticket, in parallel. A subagent sees none of your context, so its prompt must be self-contained: the Destination, the Notes/constraints, the Decisions-so-far index, the full ticket body, the tracker name plus the ticket's project id and id/ref, and the proxy-answer rule above. The subagent **works the ticket end-to-end** per wayfinder's resolve step (grilling/prototype/research discipline, solo): claim it, resolve it, post its own resolution comment, close it. Ticket-level writes belong to the subagent. It returns the answer text plus confirmation the resolution comment landed.
3. The **map** is the one thing subagents never write — you are its only writer. Append the context pointer to Decisions-so-far yourself from each returned answer, and verify the ticket actually closed rather than trusting the claim.
4. Graduate fog into new tickets, wire their edges, update or delete anything the answers invalidated.

**Done when:** every child ticket is closed, or open only as an escalation with its question posted, and no fog remains that could graduate.

### 3. Spec

Run **to-spec** with the map and the full decision record as the conversation. The seam check is a proxy answer. Publish the spec **as a wayfinder ticket itself**: a child issue of the map carrying the spec, with a `wayfinder:` label — not a separate triage-labeled issue. It lives in the tracker beside the map; nothing is written to disk.

**Done when:** the spec exists as a wayfinder ticket child of the map, and every decision on the map appears in it or is named in its Out of Scope.

### 4. Tickets

Run **to-tickets** on the spec. The breakdown quiz is a proxy answer: hold the vertical-slice rules, and split until each slice fits one fresh context window. Publish in dependency order with real blocking edges.

**Done when:** every slice is published with acceptance criteria and blocked-by links, and the spec links the ticket set.

## Handoff

Report to the human, by name and link: the Destination; the map, spec, and tickets; the proxy answers that most deserve a skim (the ones a human might plausibly reverse, marked as such); and every escalation, as a question they can answer in one line. From there the human implements tickets, or re-answers a proxy and reruns from the affected ticket.
