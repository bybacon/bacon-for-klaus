---
name: decision-making
description: An architecture decision record (ADR) is a document that captures an important architecture decision made along with its context and consequences. Use when architectural decisions are made.
---

# Decision making aka ADRs

ADRs live in the project's `docs/decisions/` folder, **tracked the way stories are**. The contract is `BT-ADR-0014` in bacon-tracker.

## How to act on decisions

Never allocate an id, move a record between status directories, or edit `proposed.md` by hand when the project has the bacon-tracker rake tasks:

```
rake decision:new['My decision']                 # proposed record from _template.md, id from the locked .next-id, line added to proposed.md
rake decision:accept[NS-ADR-0018]                # proposed → accepted, sets date, drops the proposed.md line
rake decision:reject[NS-ADR-0018]                # proposed → rejected
rake decision:deprecate[NS-ADR-0018]             # accepted → deprecated
rake decision:supersede[NS-ADR-0009,NS-ADR-0018] # old → superseded, sets superseded_by on the old and supersedes on the new
rake decision:lint                               # frontmatter, status vs directory, ids, supersession pairs, proposed.md
```

The tasks handle the file lock, the id floor, the status frontmatter and both sides of a supersession. You write the Context, Decision and Consequences. The tasks do the bookkeeping. Fall back to the manual steps below only when a project has no Rakefile wired to bacon-tracker, and run `rake decision:lint` afterwards whenever it is available.

- A decision should always be related to a chore.
- A decision should only be made if it is needed now.
- A decision has to be rational and specific.
- A decision is immutable and contains a timestamp.
- A decision has to be communicated to and accepted by all stakeholders of the system.
- A decision has to be reconsidered when updating a software system.
- A decision can recur across projects in one organization.
- A decision has to follow the contract below.

## Characteristics of a good ADR

- **Rational:** Explain the reasons for doing the particular AD. This can include the context (see below), pros and cons of various potential choices, feature comparisons, cost/benefit discussions, and more.
- **Specific:** Each ADR should be about one AD, not multiple ADs.
- **Timestamps:** Identify when each item in the ADR is written. This is especially important for aspects that may change over time, such as costs, schedules, scaling, and the like.
- **Immutable:** Don't alter existing information in an ADR. Instead, amend the ADR by adding new information, or supersede the ADR by creating a new ADR.

## Decision identification

- How urgent and how important is the AD?
- Does it have to be made now, or can it wait until more is known?
- Both personal and collective experience, as well as recognized design methods and practices, can assist with decision identification.
- Ideally maintain a decision todo list that complements the product todo list.

## The contract

**The directory is the status.** One subdirectory per status, and the directory a record sits in is authoritative:

```
docs/decisions/
  _template.md   .next-id   proposed.md
  proposed/  accepted/  rejected/  deprecated/  superseded/
```

Statuses are a closed, lowercase set:
- `proposed`
- `accepted`
- `rejected`
- `deprecated`
- `superseded`
Only `proposed` and `accepted` are non-terminal.

**The filename is the id: `<NS>-ADR-NNNN-slug.md`** - `BT-ADR-0014`, `LFD-ADR-0003`. `<NS>` is the project's tracker namespace, so a record id is unique across every project and is the *only* way to cite one. Never write a bare `ADR 0006`.

- `rake decision:new` allocates the number. Manually: take it from `docs/decisions/.next-id` and increment it. Four digits, zero-padded. Ids are never reused, gaps are fine.
- The `ADR` token is required, it separates decision ids from story ids in the same namespace.

**Frontmatter.** `status` and `date` are required. `status` must match the directory. Everything else is optional and omitted when empty:

```yaml
---
status: accepted
date: 2026-09-14
deciders: [alex]
supersedes: [BT-ADR-0009]
superseded_by: []
stories: [BT-130]
canonical: <NS>-ADR-NNNN # only when adopting another project's decision
tags: []
---
```

`date` is the day the decision took effect. While `proposed` it is the day the record was written. it moves once, on acceptance or rejection, and never again.

**A new record starts from `docs/decisions/_template.md`**, never from a blank file and never from a copy of the template pasted into this skill - one source, so the two cannot drift.

**`proposed.md`** is the ordered list of records in `proposed/`. Membership is derived from the directory. The order is the priority. `decision:new` adds the `- <NS>-ADR-NNNN - short title` line and `decision:accept` / `decision:reject` drop it. Reordering the lines is the one edit you make by hand.

## Changing a decision

An accepted record is immutable. Do not edit its Context, Decision or Consequences.

- **Correcting or extending it** - append `## Amendment - YYYY-MM-DD (STORY)` explaining what changed and why.
- **Replacing it** - write a new record with `decision:new`, accept it, then `rake decision:supersede[OLD,NEW]`. That moves the old file to `superseded/` and sets `superseded_by:` on the old and `supersedes:` on the new. Both sides, or the pair is broken.
- **Reversing it** - that is a new decision, not an edit. Nothing returns to `proposed/`.

A status change moves the file between status directories and updates `status:` to match. That is what `decision:accept`, `decision:reject`, `decision:deprecate` and `decision:supersede` do.

## Template (Nygard format)

Use the project's `docs/decisions/_template.md`. It carries the frontmatter above plus:

```
# Name the decision

## Context
What is the issue that we're seeing that is motivating this decision or change?

## Decision
What is the change that we're proposing and/or doing?

## Consequences
What becomes easier or more difficult to do because of this change?
```

Record the costs as plainly as the benefits - a Consequences section with no costs in it means the decision was not examined.
