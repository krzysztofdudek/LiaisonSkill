# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.0] - 2026-05-25

### Added
- Initial release of the `liaison` skill (`skills/liaison/SKILL.md`). Five-phase dialogue protocol — silent research → structured proposal → halt for confirmation → implementation within locked scope → plain-language close — for serving as the sole interface between a user (technical or not) and the codebase on user-visible work. Mandatory "What I think you're asking for:" opening. Required two-gate confirmation for permanent or compliance-relevant operations (separate distinct exchanges, never a single combined warning+ask). Plain-language vocabulary discipline with explicit forbidden-jargon list. User-executable verification steps in the close. Pressure-handling rules for "just do it" requests on destructive operations.
- Claude Code plugin scaffolding: `.claude-plugin/plugin.json` (manifest) and `.claude-plugin/marketplace.json` (single-plugin marketplace listing). Installable via `/plugin marketplace add krzysztofdudek/LiaisonSkill` then `/plugin install liaison@liaison-marketplace`. Single-file drop-in works for any agent that reads markdown skills.
- MIT license, README, CLAUDE.md with versioning workflow.

[Unreleased]: https://github.com/krzysztofdudek/LiaisonSkill/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/krzysztofdudek/LiaisonSkill/releases/tag/v0.1.0
