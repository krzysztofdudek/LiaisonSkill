# Liaison

**A Claude Code skill for people who don't write code but use AI agents anyway.**

You can prompt Claude without knowing what a function is. You ask for "a button on the login page so people stay signed in", Claude does something. You have no way to read the diff. If it deletes the wrong table, you can't undo it. If it shipped close-but-not-quite what you wanted, you'll find out when a user complains.

This skill puts a translator in between. Before any user-visible or destructive change, the agent stops and reads back what it heard in your own words. It waits for an explicit yes. Then it builds exactly what was agreed. Then it tells you how to test it yourself, in the actual product, not in logs.

The cost is one extra round, sometimes two if the change is permanent. The win is you stop finding out in production what you actually shipped.

## Install

### Claude Code plugin (recommended)

Two slash commands inside Claude Code. First registers this repo as a marketplace, second installs the plugin from it.

```
/plugin marketplace add krzysztofdudek/LiaisonSkill
/plugin install liaison@liaison-marketplace
```

Restart Claude Code (or run `/plugin reload`). The skill activates when the agent receives a request phrased in business terms instead of a precise spec, or when an operation could be destructive or affect users other than you.

To upgrade later: `/plugin marketplace update liaison-marketplace` then `/plugin install liaison@liaison-marketplace` again.

### Single-file drop-in (any agent)

The canonical skill body is `skills/liaison/SKILL.md`. One file, frontmatter-tagged. Copy it into your agent's skill directory.

- Claude Code, user-level: `~/.claude/skills/liaison/SKILL.md`
- Claude Code, project-level: `.claude/skills/liaison/SKILL.md` in your repo
- Codex, Cursor, other agents: wherever your tool reads skills from

## What it looks like

You: "add 'remember me' to the login page"

Agent:

> **What I think you're asking for:** A way for people to stay signed in between browser sessions so they don't have to enter their password every time.
>
> **What I would change:** Add a "Keep me signed in" checkbox under the login form. If checked, the sign-in lasts 30 days. If unchecked, the person is signed out when they close the browser.
>
> **What I would NOT change:** How passwords are stored, the "forgot password" flow, or two-factor sign-in.
>
> **Who's affected:** Anyone who signs in.
>
> **Reversibility:** Easy to undo.
>
> **Compliance / data notes:** Stores a longer-lived sign-in cookie on the device. People on shared computers should leave the box unchecked. I'd add a short hint near the checkbox.
>
> **How to check afterward:** Sign in with the box checked, close the browser, reopen, visit the site, you should still be signed in. Repeat with the box unchecked, you should be signed out.
>
> **Before I do anything: confirm above?**

You see the scope, the blast radius, and the 30-day default before anything ships. Veto, adjust, or say go.

## When it fires

When the request comes in product terms instead of a spec. When the operation deletes or removes something. When it touches personal data, payments, or audit. When it affects users other than you. When there's no written spec to follow verbatim.

If you're not sure, the skill fires anyway. The cost of asking once too often is a turn. The cost of asking too few times is what got shipped.

Full details (triggers, phases, gates, vocabulary discipline, red flags) live in [`skills/liaison/SKILL.md`](skills/liaison/SKILL.md).

## Why this exists

I built Yggdrasil and BePrecise for people who can read code. They both assume someone who understands what the agent is doing. This one is for the other side.

Non-technical people can prompt Claude now. They also need someone in the middle who asks before acting and proves afterward that what was promised is what shipped. Without that mediator the agent makes its own call on what you meant and reports done in words you can't verify.

Each of the five phases (silent research, structured proposal, halt for confirmation, locked-scope implementation, plain-language close) prevents a specific way agents quietly ship the wrong thing to people who can't read the diff.

## FAQ

**Won't every interaction be long?**
Only when stakes warrant it. There's a collapsed one-paragraph form for low-stakes asks (easy to undo, only affects you, no compliance, no money, no hidden branches). The full template comes out for anything harder. Trivial requests get trivial treatment.

**Does it work for technical users too?**
Yes. The plain-language discipline doesn't pretend the user is non-technical. It keeps the proposal in product vocabulary so anyone can veto without translating, and the verification at the end is something you run in the actual product, not in logs.

**Is one "yes" enough for destructive operations?**
No. Permanent or compliance-relevant operations require two separate exchanges. Confirm the proposal. Then in a new turn, the agent restates the permanence in concrete terms (file counts, irrecoverable items, cost). Then you say yes again. A combined "warning + ask" in one turn does not count.

**Does it conflict with be-precise, TDD, debugging?**
No. Be-precise handles spec-to-code. Liaison handles user-to-spec. TDD and debugging are about how you build or fix. Liaison is about what you build and whether you confirmed it. They compose.

**What if the user pressures the agent to skip the gate?**
Hold it once politely. If they push on a reversible op, name the default and ship. If they push on something permanent or compliance-relevant, hold the gate. "User clearly knows what they want" is the exact rationalization the gate exists to prevent.

## License

MIT

## See also

**[Be Precise](https://github.com/krzysztofdudek/BePreciseSkill).** Once intent is locked, this stops the agent from silently filling spec gaps. Liaison covers the user-to-intent gap. Be-precise covers the intent-to-code gap.

**[Researcher Skill](https://github.com/krzysztofdudek/ResearcherSkill).** Once you have a measurable goal, point the agent at it and let it iterate. Experiments, hypotheses, kept and discarded.

**[Yggdrasil](https://github.com/krzysztofdudek/Yggdrasil).** Architecture rules and AST checks your agent can't ignore. A reviewer verifies every change before the agent moves on.

---

<div align="center">
  <img src="yggdrasil.svg" alt="Yggdrasil" width="150" />
</div>
