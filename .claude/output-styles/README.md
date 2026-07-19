# `.claude/output-styles/`

Claude Code **output styles** for VideoFire agent runs. An output style changes
**how the agent writes its prose to a human** — not what tools it calls or how
it reasons about code. It's a harness setting, re-applied every turn, so it
holds for a whole session where a `CLAUDE.md` line would drift as context fills.

Today there's one style: [`readable.md`](readable.md), activated repo-wide by
`"outputStyle": "Readable"` in the reusable
[`.github/workflows/claude-run.yml`](../../.github/workflows/claude-run.yml). It
applies to **every** Claude model run (`/opus`, `/sonnet`, `/fable`) since they
all call that shared workflow. It does **not** reach the OpenAI Codex agents
(`/luna`, `/sol`), which run through a separate workflow and aren't Claude Code.

## What the `Readable` style does

It optimizes for the reader's time. Three rules, in short:

- **Bans the "Claude-speak" flavor vocabulary** — the physical/visual metaphors
  bolted onto ordinary software situations (_spine_, _seam_, _surface_,
  _footgun_, _blast radius_, _load-bearing_, _belt and suspenders_, _cargo
  cult_, _haunted_, …). It says the plain thing instead ("the boundary between X
  and Y", "other code depends on this", "an easy mistake that would break X").
  The nuance: it bans the **decorative metaphor**, not the idea — genuine
  technical terms (dependency, tradeoff, boundary, contract, regression, race
  condition) stay, as do literally-accurate industry terms (smoke test,
  dogfood).
- **Cuts filler and sycophancy** — no "You're absolutely right", "Great
  question", "Let me…"; sparing em-dashes; no reflexive "want me to do more?"
  sign-offs.
- **Leads with the answer** — inverted pyramid: the conclusion in the first
  sentence, the reasoning after; short sentences, short paragraphs, restrained
  emphasis.

It explicitly leaves coding behavior unchanged (`keep-coding-instructions: true`
in the front matter). Read [`readable.md`](readable.md) for the full rules and
the instead-of/write table.

## Why the banned words still leak — and the real fix

The style lowers the frequency of the banned words but **can't drive it to
zero**, and it's worth being clear why, because someone will notice the residue
("less often, but still there").

**The repo's own docs are saturated with these terms.** They appear **hundreds
of times across ~90 Markdown files** — in `.claude/rules/`, `AGENTS.md`, the
roadmap, and the design docs, where _seam_ / _spine_ / _blast radius_ /
_load-bearing_ are the established house style. That corpus is exactly the
material the agent reads to understand a task before writing a response, and a
standing "write prose that reads like the surrounding docs" instruction pulls
the vocabulary back in. Both the style file and the freshly-read context are
active every turn, so the words surface at reduced frequency. That's the
observed "less often but still" behavior, and it's a limitation of a
prompt-level style: it's a strong bias, not a lint rule.

**The higher-leverage fix is scrubbing the house-style docs**, not editing the
style file. Rewriting the metaphor uses across `.claude/rules/` and the
high-count roadmap/design docs to their plain equivalents stops the corpus from
teaching the vocabulary in the first place — which is what actually moves the
needle. Editing `readable.md` alone can't win against a corpus that keeps
modeling the banned words.

**One carve-out, so the ban isn't over-corrected.** `blast radius` is also a
literal proper noun here: there's a real skill named
[`report-blast-radius`](../skills/report-blast-radius/SKILL.md). Naming that
skill is correct usage, not flavor. The rule stays "ban the decorative
_metaphor_, keep the term when it's literally accurate" — a blanket "never say
blast radius" would break legitimate references.
