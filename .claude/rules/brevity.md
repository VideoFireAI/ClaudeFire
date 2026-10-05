# Rule: brevity and DRY — one canonical home per fact, and cut what another system already holds

Applies whenever you write or edit a doc, skill, agent, rule, comment, or PR/issue body.

Two failures, one cause — writing down something you don't own.

- **Duplication** puts a fact in two files. The copy can't be updated by the change that invalidates it, so it rots
  silently and only an agent sweep finds it.
- **Verbosity** puts information in prose that some other system already holds — git, the issue tracker, the code
  itself. It costs context on every read and earns nothing.

## The ownership test

Before writing a sentence, ask: **if the thing this describes changed tomorrow, would this sentence become wrong — and
would the person making that change edit this file?**

- Yes wrong, yes they'd edit it → you own it. Write it.
- Yes wrong, no they wouldn't → **link instead.** This is the duplication that rots.
- Not wrong either way → it's background. Cut it.

## A document holds only what no source file can

The ownership test above applies per sentence. Apply it also at **document and section** granularity: a section can pass
sentence-by-sentence review while being a wholesale copy of a source file nobody asked.

Before writing or keeping a section, ask: **what artifact owns the fact this section states?** If a type, a schema, a
JSDoc block, a test, or a config file already owns it, the section is a copy — cut it and point at the owner. A
duplicate can be accurate and still be wrong: "is it wrong?" returns no; "does a source file already own this fact?"
returns yes. Those are different questions, and only the second one catches it.

**What a doc legitimately owns**, so this rule does not read as "delete all documentation":

- A shape that spans files — which of several modules owns which kind of thing, where no single file states it.
- A procedure spanning packages — a recipe whose steps live in four places.
- Rationale for a design choice that no file's code expresses.
- Anything whose reader will not open the source file to do the task in front of them.

**What a doc does not own:** a restatement of a type's fields, an enumeration of a union's members, a per-item catalog
whose items each carry their own doc comment, a list of which handler maps to which case.

**The reverse link is the cheaper half.** When a source file is the owner, add the pointer _there_ — at the definition.
An agent editing the union sees it at the moment it needs it, which no checklist item achieves.

## DRY: reference by contract, never by internals

**Treat every other skill, agent, doc, and module as a black box.** You may name it and state its contract — what you
pass in, what comes back, what it's for. You may not restate how it works inside.

- **A fact has exactly one home.** Everywhere else links to that home. When you catch yourself writing a fact the second
  time, delete it and link.
- **Restate the stable part, not the volatile part.** A name and the contract it publishes can't change under you
  silently. Its step count, internal file paths, node names, and mechanism can.
- **Never quote a count or an enumeration you don't own.** "all gates" survives a fifth gate; "the four gates" does not.
  Same for "each lens", "every check", "the whole chain".
- **Moving content leaves a pointer, not a copy.** The old location becomes one line naming the new home.
- **If you genuinely must restate an internal**, say so at the point of restatement and name the file it mirrors, so the
  next reader knows it's a copy that can drift.

### No exact numbers in documentation or code describing another system

**Do not hard-code counts that describe another system's implementation.** Phrases like "the 4 gates", "calls 3 agents",
or "runs 2 retries" are promises this file cannot keep — as soon as the count changes elsewhere, this text rots and
requires a documentation update you won't know to make. Use qualitative language: "all gates", "multiple agents", "a
configurable retry limit". The only exception is a count you own and enforce here, in this same file.

These are 'temporal comments' meaning they're VERY likely to become stale quickly. DO NOT use temporal or fragile
comments.

### DRY applies to code as much as to docs

**Duplicated logic creates multiple sources of truth, and both copies bitrot independently.** When you find yourself
copying a constant, a formula, or a block of logic from one place to another, define it once and reference it — in a
shared type, a utility function, or a named constant. A codebase where the same rule is encoded in two files will
disagree the moment one is updated and the other is not.

## Concision: cut what another system already holds

- **No history or evolution.** A file is the current specification, not a changelog. "Originally X, then we moved to Y,
  now Z" is three facts where one is true. Git holds the rest.
- **No issue or PR links as provenance.** "from #1234", "the research record from #567", "per the discussion in #89" —
  cut them. Link only when the reader must open that issue to do the task in front of them.
- **No rationale that only defends the decision.** Keep the "why" when it changes what the reader does at a decision
  point; cut it when it only explains why a past reviewer was satisfied.
- **No restatement.** No summary that repeats the body, no "as mentioned above", no heading that re-explains itself in
  its first line.
- **No throat-clearing or hedging.** Delete "it's worth noting that", "in general", "you may want to consider".
- **Parallel items go in a list or table.** Prose that enumerates is longer and harder to scan than the list it's
  imitating.

## Emphasis is a budget

Bold and warning callouts work by contrast. When every paragraph is bold, none of it reads as important. Bold the one
term that carries the rule, not the sentence around it.

## The pass

Before you finish an edit:

1. Every fact — do I own it? If not, is it a link?
2. Every cross-reference — is it a name and a contract, or did I leak internals?
3. Every count, list, and enumeration of another thing's parts — can it grow without me?
4. Every sentence — does deleting it lose something the reader needs to act?
5. Every section — what artifact owns this? If one does, is this section a copy?

## Direct Writing Style

When writing explanations, documentation, comments, plans, or other prose, be **upfront, literal, and direct**.

State the actual point first. Do not create suspense, mystery, or rhetorical buildup that forces the reader to infer the
conclusion.

### Avoid

- Rhetorical contrast such as: "X isn't Y — it's actually Z."
- Delayed reveals where the important information appears at the end of a sentence.
- Phrases like "the thing that...", "what looks like...", "what's really happening...", or "the surprising part is..."
- Rhetorical questions used to introduce or explain a point.
- Dramatic framing, clever phrasing, wordplay, or manufactured paradoxes.
- Marketing, clickbait, or "thought leadership" language.
- Writing that tries to sound profound, clever, or insightful instead of simply communicating the information.
- Unnecessary em dashes used to create dramatic pauses.
- Negating one idea merely to introduce the real idea.

### Prefer

Use simple declarative sentences. Put the subject, action, and conclusion directly in the sentence.

Bad:

> The banner isn't the gate — and the thing that is, is inverted.

Good:

> The banner is not the gate. The gate is inverted.

Bad:

> What looks like a deployment problem is actually a caching issue.

Good:

> The problem is caused by the cache, not the deployment.

Bad:

> There's a subtle issue here: the worker isn't actually doing what you think.

Good:

> The worker does not perform this operation.

Bad:

> The interesting part is that the request never reaches the server.

Good:

> The request never reaches the server.

### Core principle

**Do not make prose more interesting at the expense of making it more direct.**

Optimize for clarity, precision, and information density. The reader should understand the point on the first pass
without having to decode the author's rhetorical structure.

When there is a choice between a clever sentence and a straightforward sentence, always choose the straightforward
sentence.
