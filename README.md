# ClaudeFire 🔥

### An output style that makes Claude Code talk like a person again.

> _"There's a terrible seam in your spine which might be causing a footgun and
> have a significant blast radius."_
> — Claude, allegedly telling you your function is a bit long

If you've used Claude Code lately, you know the voice. Everything is a **seam**
or a **spine**. Every risky change has a **blast radius**. Your one-line fix is
a **footgun**, the util nobody imports is **load-bearing**, and the bug is not
"unexpected," it's **haunted**. It will not simply disagree with your design.
It will inform you, warmly, that there is a terrible seam in your spine.

ClaudeFire is a drop-in [Claude Code output
style](https://code.claude.com/docs/en/output-styles) that turns that off. It
bans the flavor vocabulary, keeps the real engineering concepts, cuts the
"You're absolutely right!" filler, and tells Claude to lead with the answer
instead of narrating a scenic tour of its own findings.

It does **not** touch how Claude writes code. Same rigor, same tools, same
reasoning. It only changes the prose it aims at you.

> [!TIP]
> **⭐ If this made you exhale, star the repo.** Not for internet points — the
> theory is that if enough of us star it, it eventually lands on a dashboard at
> **Anthropic** and Claude finally learns to stop telling us about the seams in
> our spines. Think of the star button as a petition. One click, no blast
> radius. 🔥

---

## The problem, as documented by r/ClaudeCode

This whole thing exists because of [one legendary
thread](https://www.reddit.com/r/ClaudeCode/comments/1uyxibb/theres_a_terrible_seam_in_your_spine_which_might/)
where a few hundred developers realized they were all being subtweeted by the
same robot. A tour:

- **papabear556 (top comment, 117 points):** _"Look as long as it's not load
  bearing you'll be fine."_
- **mxriverlynn:** _"you're right to call that out. but here's what everyone is
  quietly talking about: wear your belt and suspenders"_
- **Instance9279:** _"You hit the nail on the head!"_ →
  **Exotic-Anteater-4417:** _"'Nail' is doing a lot of work there"_
- **sweatyboasting197:** _"that 'significant blast radius' one always gets me,
  it's like they're describing a bomb disposal instead of a code refactor"_
- **wldsoda:** _"I was building supplementary marketing assets... and it started
  calling them 'spine assets' 🤮"_
- **A-Pseudo-Random-User**, doing the voice better than the voice: _"I'm going
  to give you an honest assessment. Your observation is load bearing. The thread
  of your thoughts is correct... Would you like me to explore that trend next?
  ... There could be a rich seam to explore there. Should I delve into that
  instead?"_

The best theory in the thread, from **AVAVT** (104 points):

> When AI used easy to understand terms, human could actually understand it and
> quickly pointed out the error. When it used obscure terms human quickly got
> tired of trying to comprehend, and just nodded. It recognized that as good
> feedback → behavior retained.

And the most honest diagnosis, from **Tall-Log-1955**:

> I think it's the result of being trained to not offend people or criticize
> them too much. "Your design is bad because it has flaws that are going to
> cause problems" is something that it has a hard time saying. So it says
> "There's a terrible seam in your spine which might be causing a footgun and
> have a significant blast radius."

The original poster, **brainhack3r**, ended with: _"I'm going to give it this
thread and see if it understands the problem and propose a solution :)"_

Reader, this repo is the solution.

---

## Why a `CLAUDE.md` line doesn't cut it

The thread already tried the obvious fix, and **kusa-jp** wrote the definitive
post-mortem:

> a `claude.md` line (or telling it to "remember" to talk normal) works for
> about 20 minutes, then the context fills up and it quietly drifts back to
> spine/seam/load-bearing. the instruction is still in there, it just gets
> outweighed. what actually stuck for me was moving it out of `claude.md` into
> an **output-style**, because that gets re-applied every turn instead of
> sitting at the top of a growing conversation. [...] so **ban the vocab, keep
> the concepts.**

That's the entire design of this repo, and it's why **Xaun** reported success
where `CLAUDE.md` failed:

> Look up Claude Code `/output-style` in settings. I made my own and it solved
> my frustrations. Rules and Claude.md was not effective for me.

An output style is a harness setting, re-applied **every turn**. A `CLAUDE.md`
note is one line at the top of a conversation that keeps growing until the note
is a rounding error. Same instruction, very different staying power.

---

## What `Readable` actually does

Three rules. That's it.

1. **Bans the flavor vocabulary, keeps the concepts.** Out go the decorative
   metaphors: _spine, seam, surface, substrate, spike, vein, wrinkle, sidecar,
   chamfer, footgun, blast radius, load-bearing, belt and suspenders, cargo
   cult, haunted, grooves, smoking gun._ In their place, the plain thing: "the
   boundary between X and Y," "other code depends on this," "an easy mistake
   that would break X." The real terms that carry meaning — _dependency,
   tradeoff, boundary, interface, contract, regression, race condition,
   idempotent, invariant_ — stay. So do legitimately-standard terms like _smoke
   test_ and _dogfood_ (yes, [the thread argued about those
   too](https://www.reddit.com/r/ClaudeCode/comments/1uyxibb/theres_a_terrible_seam_in_your_spine_which_might/)).
   The rule is: ban the costume, keep the idea.

2. **Cuts the filler and sycophancy.** No "You're absolutely right." No "Great
   question." No "Let me…." No em-dash pileups. No ending every turn with "Would
   you like me to explore that next?" As **Neurojazz** imagined Claude finally
   pushing back: _"Pros read prose. Get back to work monkey brain."_

3. **Leads with the answer.** Inverted pyramid: conclusion first, reasoning
   after. Short sentences, short paragraphs. The reader gets the outcome in
   sentence one and can stop reading if that's all they needed.

The full ruleset, including the instead-of/write table, lives in
[`.claude/output-styles/readable.md`](.claude/output-styles/readable.md).

---

## Install

1. Copy [`readable.md`](.claude/output-styles/readable.md) into your project (or
   your home directory) under `.claude/output-styles/`:

   ```
   your-project/
   └── .claude/
       └── output-styles/
           └── readable.md
   ```

2. Turn it on. In Claude Code, run `/output-style` and pick **Readable** — or
   set it in your settings:

   ```json
   { "outputStyle": "Readable" }
   ```

3. That's it. It re-applies every turn.

---

## The one honest caveat (sorry)

It's a strong bias, not a lint rule. If your own repo's docs are stuffed with
_seam_ and _blast radius_ — and a lot of them are, because that's been the
house style everywhere for a year — Claude reads that corpus every turn and some
of the vocabulary leaks back in. The style lowers the frequency; it can't drive
it to zero while the surrounding docs keep teaching the words. The real fix is
scrubbing the docs too. See
[`.claude/output-styles/README.md`](.claude/output-styles/README.md) for the
longer version.

Also worth stating plainly, because the thread worried about it and **kusa-jp**,
**actvt_io**, and others tested it: **banning flavor words costs you nothing on
quality.** It's pure style. The only time output got worse was when people got
greedy and also banned real concept words like _tradeoff_ or _dependency_. This
style deliberately doesn't do that.

---

## Credit

All of this is downstream of one Reddit thread and its very funny comment
section:

**["There's a terrible seam in your spine which might be causing a footgun and
have a significant blast
radius"](https://www.reddit.com/r/ClaudeCode/comments/1uyxibb/theres_a_terrible_seam_in_your_spine_which_might/)**
— r/ClaudeCode, by brainhack3r.

If your Claude ever tells you your PR has a significant blast radius, send it
here.

---

## One last thing: ⭐ star it

Seriously. If you want Claude Code to talk like a person, the fastest way to
make that happen is to make this loud enough that **Anthropic** notices. Stars
are the only vote GitHub gives you.

So: **[star the repo](https://github.com/VideoFireAI/ClaudeFire)**, send it to
the one coworker whose Claude keeps calling their config "load-bearing," and
let's get the seams out of everyone's spines.

_No blast radius. Promise._
