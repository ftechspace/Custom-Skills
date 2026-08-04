
---
name: ticket-breakdown
description: Break a feature, spec, PRD, bug report, or vague piece of product work into a set of well-formed engineering tickets with clear scope, acceptance criteria, and dependencies. Use this skill whenever the user wants to split, slice, scope, or decompose work; whenever they mention tickets, issues, stories, epics, backlog grooming, sprint planning, or "what needs to get built"; and whenever they hand over a spec, PRD, feature description, or feature request and ask what to do with it. Also use it when the user has a large or fuzzy chunk of work and asks how to sequence or estimate it, even if they never say the word "ticket."
---

# Ticket Breakdown

Turn a chunk of product work into tickets an engineer can pick up cold and finish
without a follow-up conversation.

The failure mode this skill exists to prevent: tickets that are really just
section headings from the spec, restated. Those look like a breakdown but push
all the actual thinking onto whoever picks them up.

## Before writing any tickets

Read the source material and answer these for yourself. If a question can't be
answered from what you were given, that gap becomes an explicit open question in
the output — don't paper over it with a plausible guess.

1. **What's the user-visible change?** If nothing changes for a user or an
   operator, say so plainly; it's infrastructure work and gets scoped differently.
2. **What's the smallest version that could ship on its own?** This becomes the
   spine of the breakdown.
3. **What's explicitly out of scope?** Name it. Unstated non-goals are where
   scope creep enters.
4. **What's genuinely unknown?** Unknowns become spikes, not estimates.

If the source material is thin enough that most of these come back empty, stop
and ask the user rather than generating a confident-looking breakdown built on
invented requirements.

## Slice vertically, not horizontally

The most common bad breakdown splits work by technical layer: one ticket for the
schema, one for the API, one for the UI. Nothing is demoable until all three land,
every ticket blocks the others, and a slip anywhere slips everything.

Slice by user-visible capability instead. Each ticket should move something from
not-working to working, even if the working version is narrow.

**Example:** "Add saved searches"

Horizontal (avoid):
- Create `saved_searches` table
- Build saved-search API endpoints
- Build saved-search UI

Vertical (prefer):
- Save a search from the results page and see it in a list (one search, no naming, no editing)
- Rename and delete a saved search
- Run a saved search from the sidebar
- Share a saved search with a teammate

The first vertical ticket is bigger than the first horizontal one — it carries
schema, endpoint, and UI. That's the point. It also ships.

Horizontal splits are legitimate in two cases: a migration or platform change with
no user-facing surface, and a foundation genuinely shared by several slices where
building it inside the first slice would distort it. When you use one, say why.

## What each ticket needs

- **Title** — verb-first, describes the outcome. "Save a search from the results
  page," not "Saved search work" or "Frontend: search."
- **Context** — two or three sentences on why this exists and who it's for. The
  reader may never have seen the spec.
- **Scope** — what this ticket covers, and a short "not in this ticket" list
  pointing at the sibling tickets that handle the rest.
- **Acceptance criteria** — observable, checkable statements. Behavior, not
  implementation. If two engineers could disagree about whether it's met, rewrite it.
- **Dependencies** — what must land first, and what this unblocks. Say "none" when
  there are none; silence reads as an oversight.
- **Open questions** — anything the assignee would have to ask about. Tag who
  should answer.
- **Size** — S / M / L, with a one-line reason. Anything L is a candidate for
  further splitting; flag it rather than silently accepting it.

Implementation notes are optional and clearly marked as suggestions. The ticket
specifies the outcome; the engineer picks the approach.

See `references/templates.md` for the full ticket template, acceptance-criteria
patterns, and worked examples of good and bad tickets. Read it when you're about
to write the actual tickets, or when the user asks for a specific format.

## Writing acceptance criteria

Good criteria describe what an observer could verify:

- Weak: "Saving works correctly"
- Better: "After clicking Save on a results page, the search appears in the saved list within the same session"

Cover the unhappy paths too — empty states, permission failures, duplicate
submissions, offline or slow network, and the largest realistic input. A
breakdown that only describes the happy path is roughly half a breakdown.

Don't smuggle new requirements in through criteria. If a criterion isn't traceable
to the source material, mark it as a proposal rather than presenting it as settled.

## Sequencing

After the tickets exist, order them. Prefer an order that:

- Puts something demoable first, even if narrow
- Front-loads the tickets whose unknowns would most change the rest of the plan
- Keeps parallel tracks parallel — call out which tickets can run concurrently

Present the order as a short numbered list with a one-line rationale per position,
not a Gantt chart in prose.

## Spikes

When an unknown blocks estimation, write a spike ticket instead of guessing. A
spike is time-boxed, and its acceptance criterion is a decision or a document, not
working code. "Determine whether the existing search index can support saved-query
replay; output a recommendation and a rough cost, time-box 2 days."

Never fold a spike's uncertainty into a normal ticket's size. That's how L becomes XL.

## Output

Default to a numbered list of tickets in the conversation, each following the
template, followed by the sequencing list and a short "open questions for the team"
section collecting everything unresolved.

Switch formats when the user asks — a table for backlog review, a markdown file
for import, one ticket at a time for interactive refinement.

Aim for the smallest number of tickets that keeps each one independently
completable. Six well-scoped tickets beat twenty fragments. If a breakdown is
producing more than roughly a dozen tickets, the work probably wants an intermediate
grouping — say so and propose the groups.

## Calibration

Match the depth to the work. A one-line bug fix does not need four tickets and a
sequencing plan; say it's a single ticket and write the one ticket. Reserve the
full treatment for work that actually spans multiple people or multiple weeks.
