# Liaison

**A Claude Code skill that makes your agent talk to a non-technical user like a senior product engineer — capture intent, translate plainly, hold consent gates, deliver exactly what was confirmed.**

The default agent ships first and asks later. It assumes defaults, expands scope silently, calls things "done" without a way for the user to test. This skill changes the default: before any user-visible or destructive change, the agent reads back what it heard in plain language, waits for an explicit yes, delivers exactly the locked scope, and closes with user-executable verification steps.

The cost is a confirmation turn (sometimes two). The win is no silent shipping of the wrong thing, no jargon walls, no destructive operations without a conscious yes.

## Install

### Claude Code plugin (recommended)

Two slash commands inside Claude Code — first registers this repo as a marketplace, second installs the plugin from it:

```
/plugin marketplace add krzysztofdudek/LiaisonSkill
/plugin install liaison@liaison-marketplace
```

Restart Claude Code (or run `/plugin reload`). The skill activates automatically when the agent receives a request expressed in business or product terms (rather than as a complete technical specification), or when an operation could be destructive, irreversible, or affect users other than the requester.

To upgrade later: `/plugin marketplace update liaison-marketplace` then `/plugin install liaison@liaison-marketplace` again.

### Single-file drop-in (any agent)

The canonical skill body is `skills/liaison/SKILL.md` in this repo (one file, frontmatter-tagged). Copy it into your agent's skill directory:

- **Claude Code (user-level):** `~/.claude/skills/liaison/SKILL.md`
- **Claude Code (project-level):** `.claude/skills/liaison/SKILL.md` in your repo
- **Codex / Cursor / other agents:** wherever your tool reads skills or instructions from

## What it looks like running

> **User:** add "remember me" to the login page

> **Agent:**
>
> **What I think you're asking for:** A way for people to stay signed in between browser sessions so they don't have to enter their password every time.
>
> **What I would change:** Add a "Keep me signed in" checkbox under the login form. If checked, the sign-in lasts 30 days. If unchecked, the person is signed out when they close the browser.
> **What I would NOT change:** How passwords are stored, the "forgot password" flow, or two-factor sign-in.
> **Who's affected:** Anyone who signs in.
> **Reversibility:** Easy to undo.
> **Compliance / data notes:** Stores a longer-lived sign-in cookie on the person's device. People on shared computers should leave the box unchecked — I'd add a short hint near the checkbox to that effect.
> **How to check it afterward:** Sign in with the box checked, close the browser, reopen, visit the site — you should still be signed in. Repeat with the box unchecked — you should be signed out.
>
> **Before I do anything: confirm above?**

That's the kind of interruption you want. The user sees scope, blast radius, and an unsurfaced default (30 days, hint copy) BEFORE anything ships — instead of finding a 365-day cookie in the diff an hour later.

## When it fires

The skill activates whenever the request matches any of:

- Phrased in business / product terms without a precise technical specification ("add a button", "let users edit their profile", "we should email users who...")
- Touches a destructive or irreversible operation (delete, remove, purge, anonymize, hard-cancel)
- Touches compliance-relevant surfaces (personal data, payments, retention, audit, accessibility)
- Affects users other than the requester (everyone, a defined group, external recipients)
- Lacks an existing written spec the user wants followed verbatim

Full triggers, the five-phase protocol, the mandatory "What I think you're asking for:" opening, the two-gate checklist for destructive operations, plain-language vocabulary discipline, and the red flag table all live in [`skills/liaison/SKILL.md`](skills/liaison/SKILL.md).

## Why this exists

Agents default to autonomous completion. Given a fuzzy user request, they pick an interpretation, surface defaults silently, ship, and say "Done!". For purely technical tasks under a precise spec that's often fine. For user-visible work — and especially for destructive or compliance-relevant work — it's a recipe for shipping the wrong thing, eroding trust, and occasionally triggering irreversible damage.

This skill puts a senior product-engineer-shaped buffer between the user and the codebase. Capture intent. Read it back in the user's language. Wait for an explicit yes — twice for permanent operations. Build the locked scope, nothing else. Hand back a test recipe the user can run themselves.

The five phases (silent research → structured proposal → halt for confirmation → implementation within locked scope → plain-language close) and the discipline around each gate aren't ceremony. Each one prevents a specific failure mode observed when agents ship user-visible work without it.

## FAQ

**Won't this make every interaction long?**
Only when stakes warrant it. The skill includes a "collapsed form" of the proposal for low-stakes asks (easy to undo, requester only, no compliance, no billing) — one paragraph, same fields. Trivial requests get trivial treatment. The full template comes out for anything that's hard to undo, permanent, compliance-relevant, or affects users other than the requester.

**Does it work for technical users too?**
Yes. The plain-language discipline doesn't pretend the user is non-technical — it just keeps the proposal in product vocabulary so any user can veto without translating, and so the verification steps at the close are user-executable rather than buried in logs. Technical users skim and confirm. Non-technical users get an interface they can navigate.

**What about destructive operations? Is one "yes" enough?**
No. For Permanent or Compliance-relevant operations the skill mandates TWO distinct exchanges: confirmation of the proposal, then a separate turn restating the permanence in concrete terms (file counts, irrecoverable items, cost), then a second yes. A single combined "warning + ask" does NOT count. This is non-negotiable — the pattern "user said yes once, agent shipped" is exactly where irreversible mistakes happen.

**Does it conflict with be-precise, TDD, debugging skills?**
No. Be-precise governs implementation when the agent has a spec; liaison governs the interface BEFORE there's a spec, when the user request is the only input. TDD and debugging govern HOW you build or fix; liaison governs WHAT you build and whether you've confirmed it. They compose — liaison Phase D ("implementation within locked scope") delegates to TDD / debugging as normal.

**What if the user pressures me to skip the gate?**
The skill has explicit pressure-handling: hold the gate briefly and politely, one closed question, wait. If the user persists on a non-destructive operation, name the default, surface it, ship. If the operation is destructive or compliance-relevant, hold the gate, period. "The user clearly knows what they're asking for" is named in the skill as the exact failure-mode rationalization the gate exists to prevent.

## License

MIT

## See also

**[Be Precise](https://github.com/krzysztofdudek/BePreciseSkill)** — once the user's intent is locked, this keeps the agent honest during implementation: stops and asks when the spec is silent, contradicts itself, or tempts a workaround. Liaison handles the human ↔ intent gap; be-precise handles the intent ↔ code gap.

**[Researcher Skill](https://github.com/krzysztofdudek/ResearcherSkill)** — once a measurable goal is on the table, point the agent at it and let it iterate overnight: experiments, hypotheses, kept and discarded results.

**[Yggdrasil](https://github.com/krzysztofdudek/Yggdrasil)** — architecture rules in Markdown your agent can't ignore. A reviewer verifies every change against them and feeds violations back into the agent's loop before it moves on.

---

<div align="center">
  <img src="yggdrasil.svg" alt="Yggdrasil" width="150" />
</div>
