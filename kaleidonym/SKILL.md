---
name: kaleidonym
description: Use whenever the user wants a name, or a better name, for one specific thing — a project, product feature, epic, service, model, table, column, enum value, hook, component, function, variable, branch, PR title, or internal concept — including something new that has no name yet, and including reframing a specific thing so that a better name follows. Trigger on "what should I call", "rename", "name for", "the name bugs me", "better name", "reframe this", and on any request to pick a label, whether or not the user has complained about the current one.
---

# Kaleidonym

Applies Patrick Gunkel's *ideonomy* — treating things as bundles of properties along dimensions — to naming: generate name candidates for one thing, each traceable to a structural move rather than a thesaurus lookup. Everything it needs ships in this skill's directory: `bin/pick` and `methods/`.

## Core principle

A good rename isn't a synonym for the current name — it's a name for a *different framing* of the same thing. Vary the thing's properties along a dimension and each variant suggests its own name: a negated property suggests a name built on the opposite; a property re-instantiated in another domain suggests that domain's vocabulary. The operator does the conceptual work; the name is what you call the result.

## Step 0 — look at the thing before naming it

Naming from the label alone produces labels. Before drawing dimensions:

- **If it lives in code, read it — all of it.** Open the file (for a model, the table's columns in `db/schema.rb` too), and list the names of its siblings — other models in the same directory, other hooks in the same folder, other columns on the same table. Siblings fix the *register* a candidate has to fit: case, suffix (`Service`, `Policy`, `use…`), length. The register goes into the output as its own line (Step 4), because a reader judging the candidates needs it too; a candidate that breaks the register isn't disqualified, but its rationale says that it does.
- **Diagnose the current name.** One or two sentences: what does it claim that is false, what does it fail to say, what does it get confused with? Every rename is really a correction of this diagnosis, and it is the input negation needs (the false claim is a definitional property to negate). This diagnosis is the first thing the reader sees. Keep the evidence out of it: the counts and quotes that prove the diagnosis ("the comments already call it a membership 119 times", "the product doc calls these rows Contacts") go on the `Register:` line for a code target, or into the rationale of the candidate they support, so the diagnosis stays two sentences without losing them.
- **Check the premise separately.** Compare the user's description against what you read. If it is off (the column they mention doesn't exist, the thing does something else), say so on its own `Premise:` line before the diagnosis, because a name that fixes a misdescription is still wrong. Keep the correction to one sentence; the diagnosis stays one or two sentences of its own.
- **If the thing has no name at all**, there is nothing to diagnose. Replace the diagnosis with one or two sentences on what the name must say and what it must not be confused with — that is the definitional property the operators work on. A working name, a placeholder, or a name marketing dislikes is still a name: it has zero references in the repo and still gets the diagnosis, because the user is asking what it claims falsely.
- **If the thing isn't in code, read what describes it before naming gaps.** Product docs, locale copy, and nearby UI strings often answer who sees the name and what it sits next to. Product docs often sit beside the feature's code rather than at the root, so find them from the repo root and read the one nearest the feature; the repo's `AGENTS.md` or `CLAUDE.md` usually says what they're called (`fd` for that filename from the root). Not finding one in the directory you happen to be in is not evidence that none exists. What they don't answer goes on the `Gaps:` line as a question the reader can settle (who sees the name, what it sits beside, what it must not be confused with) rather than being filled in with a guess; a gap stated as an assumption ("HR buyers already use it") is a guess wearing a label. A product-facing name and an internal identifier are judged by different readers; establish which this is.

## Step 1 — draw dimension-prompts

```bash
bash "<this skill's base directory>/bin/pick" --names
```

`--names` draws dimension-prompts only, and prints each one's body — read them. Nothing else is drawn: the five naming operators all run every time (Step 2), since there are only five and skipping negation would mean skipping the diagnosis. Size the draw by how many facets the thing has: `--less` for a column, enum value, variable, branch, or PR title; `--more` for a model, service, product feature, or project; the default when you can't tell.

## Step 2 — work the dimensions and operators toward names

Apply each drawn **dimension-prompt** to the thing. Check the thing's concrete values before judging an axis: "polarity" looks dull for an access record until you notice the enum has a `no_access` value, and now the record encodes denials, not just grants. A *dud* is a dimension that, after checking, doesn't discriminate for this thing — say so in one line and move on; a forced candidate from a dud is weaker than an admitted miss. A *gap* is not having the detail to check at all — say what's missing instead of inventing a value.

Then run every **operator** as its naming move:

| Operator | Naming move |
|---|---|
| **negation** | Negate a definitional property (start with the diagnosed false claim). The opposite's vocabulary names the variant, and often names the original more honestly. |
| **substitution** | Swap one property's value and ask what *that* version is called. The contrast term frequently names the original by what it isn't. |
| **abstraction-lift** | Strip the domain words. Name the structural shape underneath — ledger, membership, lease, snapshot, verdict, grant. The shape's canonical name is a candidate. |
| **cross-domain re-instantiation** | Drop the lifted shape into another domain (law, finance, biology, theater, cartography…) and borrow the word that domain already uses for it. |
| **tree-finding** | Walk up, down, and across. Parent names generalize, child names specialize, sibling names show the register a candidate must fit. |

An operator can be a dud too: if cross-domain borrowing produces nothing that fits the register, say so and lean on the others rather than forcing a metaphor. Fuller procedures for each operator live in `methods/operators/` in this skill's directory if a move isn't landing.

For each variant the operators produce, ask: what would you call *this*? Let the variant supply the name from its own vocabulary. A candidate that is only a synonym of the current name — same framing, different word — isn't a result of any move and doesn't belong in the list.

## Step 3 — check the candidates

Scale the checks to where the name lives, because a name can only collide inside the namespace it occupies:

- **Function-local** (a local variable, parameter, or private helper): the register is the enclosing function and file, and that is the only scope a collision can happen in. Skip the per-candidate repo-wide sweep, the sibling-file Register, and the `Cost:` line, though a single check on whether a borrowed word ("palette", "entitlement") already means something specific in this codebase is still worth one search; read the function, draw with `--less`, and answer. If you notice something bigger while reading — the function is dead, the comment is wrong — say it in one line after the answer rather than expanding the naming work around it.
- **Branch names, PR titles, commit subjects**: these never share a namespace with code identifiers. Take the register from `git log --oneline`, `git branch -a`, or recent PR titles, not from sibling files; skip `rg` collision sweeps and the `Cost:` line; check only that a branch is a valid git ref (no spaces, `?`, `~`, `^`, `:`) and doesn't already exist. When asked for both a branch and a title, give each its own candidates and its own `**Pick:**` line.
- **Everything else in code** (models, columns, hooks, services, enum values): the full checks below apply.


Before presenting, for every candidate:

- **Convention.** Match the target's case and suffix pattern (snake_case column, PascalCase model, `useX` hook). A product-facing name is judged on how it sounds said aloud to a customer, not on grep-ability.
- **String collision.** For anything in a codebase, `rg` the candidate (and its snake/camel forms). Drop or flag candidates that already name something else, or that are substrings of common identifiers and would make searching painful.
- **Semantic collision.** A collision is an existing identifier, or a product term already in copy, that the candidate's words would be read as. Check whether the candidate's head word already means something different in this codebase or product — "effective" already naming a different calculation in a nearby helper, or "Suggested" already being a tab label. This catch is worth more than a string match and a string match won't find it. Identifiers are only half the namespace: for every word a candidate borrows, also search the copy customers read — locale files, pricing and plan data, product docs — because "Seats" on a pricing page or "Draft" as a document state is a collision no `rg` over class names will surface.
- **Length.** Identifiers: ideally one or two words. Product names: sayable in a sentence without explanation.
- **The diagnosis.** Does the candidate actually fix what the current name got wrong? A candidate that is merely different fails this test. The Pick must pass it: if your favourite doesn't, pick the one that does, or revise the diagnosis.
- **A collision-clean Pick.** A candidate with a collision you found can stay in the list with its `; collision:` fragment, but it can't be the Pick unless the `Cost:` line says why that collision is acceptable. Flagging a collision and picking the name anyway hands the reader the exact problem you were asked to solve.

For a rename in code, also `rg` the current name and count the references. That blast radius is what the reader weighs the Pick against, and it goes on the `Cost:` line, not in the Pick.

## Step 4 — present candidates, not process

Output this shape and nothing else. Five to eight live candidates; fewer if the thing genuinely has fewer facets, never padded to reach the count. A ruled-out name is in addition to the live ones, never in lieu of one; include at most one, last, with "ruled out:" starting its rationale, and only when the reader would otherwise ask why it's missing.

```
**Premise:** <only if the user's description was off; one sentence>
**Current name:** <what it claims falsely / fails to say, 1–2 sentences, no evidence>
    (nothing has been called anything yet, not even a working name: **Naming:** <what the name must say and must not be confused with, 1–2 sentences>)
**Register:** <code targets only: the sibling pattern candidates must fit, plus any count or quote that backs the diagnosis; one sentence>

- **<candidate>** — <the property or source domain that produced it, one sentence>; collision: <optional fragment>
- **<candidate>** — <...>
- **<name>** — ruled out: <optional, at most one, last>

**Pick:** <one candidate> — <why it best fixes the diagnosis, one sentence, hard stop>
**Cost:** <optional, one sentence: reference count of the current name, what else would rename, what the pick keeps greppable, how the new record relates to existing ones>
**Gaps:** <non-code targets only: what the brief doesn't say that the name depends on, one sentence>
**Not covered:** <a property, domain, or branch the dimensions touched but didn't turn into a name, said in plain words>
```

A "line" in this shape means one sentence; a second sentence is a second line even when it sits on the same row. The rationale is one sentence that traces the candidate to what produced it: either the property of the thing, restated in plain words, or the domain the word was borrowed from. The operator and dimension names are for you; the reader gets the reason.

- **Good:** `**Seat** — billing's word for one licensed member` (source domain). `**Proposed Updates** — nothing goes live until the customer approves, so what the AI makes is a proposal` (property, plain words).
- **Bad:** `**AccessEntry** — accurate and bland, reads like a log line` (describes how the name feels; traces nothing). `**Seat** — substitution on the scope dimension` (operator label; the reader can't use it). `**customer_visible** — flips the boolean so it reads positively` (describes the move you made on the name, not what about the thing it names).

Restating a property in plain language is the intended form and is not process narration; only the labels (negation, polarity, tree, sibling) are. Collision notes and convention breaks go in a trailing `; collision: …` fragment so they don't swell the sentence; every collision you found belongs there, because a hit you dropped silently is a hit the reader will rediscover. Migration notes and follow-ups go on the `Cost:` line or nowhere. The **Pick** line is not optional — the reader asked for a name, not a kaleidoscope; if two are genuinely tied, say which trade-off decides it. The Pick's one sentence says why the name fixes the diagnosis and stops there. The clause that wants to follow it — "and keeps `FeatureAccess` so call sites stay greppable", "and should `belongs_to` the snapshot", "OrganizationMembership is the fallback" — is about cost, schema, or a runner-up, and every one of those belongs on the `Cost:` line. If you find yourself writing "and" or "so" after the reason, cut there. The **Not covered** line tells the reader what's still available to name later, as the property itself ("whether a decision ever expires"), never as the prompt that raised it ("lifecycle").

Before presenting, check your draft against the names of the dimension-prompts you drew and the five operators. Many of them are ordinary words — lifecycle, scope, source, visibility — so they slip in as labels without looking like jargon. Wherever one is standing in for an explanation, replace it with the property it pointed at.

Candidates are a flat bullet list. Lay them out as a 2×2 markdown pipe table only when two drawn dimensions both discriminated and most of the four cells fill with a name; an empty cell is a name nobody needs, and a table with two empty cells is a list wearing a costume.

Plain text only: no Unicode box-drawing, no ANSI color. The output should paste cleanly into Slack, a PR description, or a commit message.

### Worked example

Thing: a Rails model `AccessGrant`, one row per user per resource, with an `access_level` enum whose values include `no_access`. Siblings in `app/models/`: `Permission`, `RoleAssignment`.

```
**Current name:** Claims every row grants access. The enum includes `no_access`, so a row can be a denial — the name is wrong for a third of the data.
**Register:** PascalCase nouns naming the record, not the act — Permission, RoleAssignment.

- **AccessDecision** — grant and deny are both decisions; names the row by what it always is
- **AccessRule** — underneath, a row is a predicate over (user, resource), and a predicate is a rule
- **AccessVerdict** — law's word for a decision that can go either way; sounds adjudicated, which rows aren't
- **AccessEntry** — the word that stays neutral about grant versus deny; honest but reads like a log line
- **Permission** — ruled out: the parent category; collision: already a class in app/models

**Pick:** AccessDecision — a row that can grant or deny is always a decision, which fixes the false claim directly.
**Cost:** 212 references in 48 files, with AccessGrantPolicy and the access_grants table following, though keeping "Access" leaves existing call sites greppable.
**Not covered:** whether a decision ever expires was never named; a `revoked_at` or TTL would need its own vocabulary.
```

## Why randomized selection

Left alone, naming defaults to synonym-swapping or bolting an adjective onto the existing name. The dimension draw forces you to inspect properties you wouldn't have reached for — polarity, lifecycle, authority — and each operator then turns an inspected property into a reframing rather than a thesaurus pass.

## Attribution

The operators, organons, and dimension-prompts come from Grace Kind's essays on Ideonomy at [gracekind.net](https://gracekind.net/writing/ideonomy/intro), which in turn synthesize the unpublished work of Patrick Gunkel (1937–2017), MIT futurist and originator of ideonomy. Grace's writing is the proximate source; Gunkel's monographs are the originals.
