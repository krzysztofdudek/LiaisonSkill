# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.2.0] - 2026-05-26

### Changed
- Broadened skill scope in `skills/liaison/SKILL.md`. The skill now engages on **every user-facing turn about the codebase or product** — both change requests (which run the full five-phase ceremony) AND pure questions / explanations / translations between technical and user-visible terms (which run cross-cutting rules only, no Phase B / C / E ceremony). Description, `## When to use`, and `## When NOT to use` sections updated to reflect this. The cross-cutting rules (Plain-language discipline, No time or effort estimates, Question discipline) apply to every user-facing turn regardless of whether a change is happening; the five-phase ceremony is what's skipped when no change is involved, not the skill itself. The previous "investigative / diagnostic work" exclusion is removed accordingly.

### Added
- New top-level "Required: no time or effort estimates" section in `skills/liaison/SKILL.md`, mirroring the destructive-op gate's structure: a 4-box checklist (abstract pattern descriptions to avoid copy-targets) that the agent runs against every draft before sending; an applies-even-when list naming authority claims, time pressure, sunk cost, and ballpark framings as non-overrides; a STOP anchor for "this person clearly knows what they're doing, so a number from me is safe" reasoning; and a cross-reference to the detailed mechanics in Cross-cutting rules. Forbids producing any time, duration, or calendar claim in any user-facing turn — including the agent's own meta-work duration, stakeholder messaging the agent crafts for the user to relay, and hypothetical example estimates the agent constructs as templates.
- Detailed cross-cutting rule "No time or effort estimates" in the same file. Explains the asymmetric harm (inflated estimate kills viable work as "too expensive"; deflated estimate creates downstream commitments the work cannot meet) and grounds the rule in the actual cause: numbers come from training data of human-team estimates carrying overheads agents do not have. Names the three specific failure modes that leak under pressure with explicit phrasing forbidden for each. Provides a literal pressure-handling script (modelled on the destructive-op gate's script) the agent uses verbatim under repeated estimate pressure. Carves out what is still allowed (reversibility vocabulary; product feature parameters described abstractly; dollar / API-call cost with named units; factual measurements the agent actually performed; time-boxes the user proposes). Reinforced by a Red flag bullet and a Common mistakes row.
- Stress-test harness for the rule in `.lab/` (untracked). Battery of 11 multi-turn pressure scenarios (in `.lab/workspace/scenarios.json`), LLM-judge compliance metric (Haiku), multi-evaluator refusal-quality metric (3 Haiku evaluators per response, median). Multi-baseline measurement protocol (≥3 runs per model per experiment) to distinguish signal from per-run noise (~20pt variance per Sonnet baseline). Validated final rule form across 5 Sonnet + 3 Opus runs: aggregate compliance 0.79 (Sonnet mean 0.70 / Opus mean 0.93, with 2 of 3 Opus runs hitting perfect 1.00) vs initial-rule baseline 0.62 (Sonnet 0.60 / Opus 0.70) — improvement of +17% Sonnet / +33% Opus on the same battery. See `.lab/summary.md` for the eight-experiment journey, the methodology lessons, and the one remaining persistent edge case (Sonnet on multi-turn ballpark pressure with embedded self-meta-work duration) that text-only iteration could not fully close.

## [0.1.0] - 2026-05-25

### Added
- Initial release of the `liaison` skill (`skills/liaison/SKILL.md`). Five-phase dialogue protocol — silent research → structured proposal → halt for confirmation → implementation within locked scope → plain-language close — for serving as the sole interface between a user (technical or not) and the codebase on user-visible work. Mandatory "What I think you're asking for:" opening. Required two-gate confirmation for permanent or compliance-relevant operations (separate distinct exchanges, never a single combined warning+ask). Plain-language vocabulary discipline with explicit forbidden-jargon list. User-executable verification steps in the close. Pressure-handling rules for "just do it" requests on destructive operations.
- Claude Code plugin scaffolding: `.claude-plugin/plugin.json` (manifest) and `.claude-plugin/marketplace.json` (single-plugin marketplace listing). Installable via `/plugin marketplace add krzysztofdudek/LiaisonSkill` then `/plugin install liaison@liaison-marketplace`. Single-file drop-in works for any agent that reads markdown skills.
- MIT license, README, CLAUDE.md with versioning workflow.

[Unreleased]: https://github.com/krzysztofdudek/LiaisonSkill/compare/v0.2.0...HEAD
[0.2.0]: https://github.com/krzysztofdudek/LiaisonSkill/compare/v0.1.0...v0.2.0
[0.1.0]: https://github.com/krzysztofdudek/LiaisonSkill/releases/tag/v0.1.0
