# Changelog

All notable changes to Remora are documented here.

## 0.1.3 — 2026-07-12

Correct the context policy introduced in 0.1.2. Stock Claude Code treats unknown custom model ids as 200K and caps a larger auto-compact setting, so Remora now defaults to a truthful 200K client window and leaves Claude's native output-reserve and precompute behavior untouched.

Add an explicit `calico` mode for users who install a verified Calico Claude binary containing the new `custom-context-window` adapter. In this mode Remora supplies an exact gateway-derived model/window map, reports 95% usable context to status-line consumers, and compacts at 90% of the raw provider window. Invalid maps and missing patches fail closed. Documentation also warns that `/resume` restores the old session's agent definitions after routing changes, and the release test no longer assumes Claude Code exists on the CI runner.

## 0.1.2 — 2026-07-12

Add gateway-aware context safety for long Claude Code sessions. Remora reads CLIProxyAPI's Codex-compatible model catalog and uses the smallest context window among every configured main and agent model. It mirrors Codex CLI 0.144.1's defaults: 95% is reported as effective context and auto-compaction starts at 90% of the raw provider window. For the current 372K GPT-5.6 catalog, that means 353.4K effective context and a 334.8K compact trigger.

Discovery failures and incomplete catalogs fall back conservatively without blocking startup. Explicit user environment overrides still win. `doctor --online` now reports the detected per-model ceilings, effective context, and compact trigger, while `dry-run` exposes the token-free child settings.

## 0.1.1 — 2026-07-12

Fix the offline bootstrap test on Linux runners by making the explicit checksum-only override take precedence when requested. Bootstrap remains attestation-first by default; only callers that set `REMORA_ALLOW_CHECKSUM_ONLY=1` opt into the documented trust downgrade. The bootstrap test now derives its artifact version from `VERSION` so future releases cannot silently test a stale package name.

## 0.1.0 — 2026-07-12

Initial release. It includes the isolated launcher, a six-role GPT-5.6 Sol/Luna map with Terra as the default Sonnet alias, TOML configuration, environment or credential-command authentication, offline/online doctor checks, and isolation-focused tests.

The public installation path is approval-gated and release-pinned. Release archives carry SHA-256 checksums and GitHub build-provenance attestations; the bootstrap requires both unless the user explicitly accepts checksum-only verification. The installer performs collision checks, preserves existing configuration, updates the payload atomically, and never writes native Claude state.

The gateway documentation now includes a minimal Docker Compose deployment, independent OpenSSL-generated keys, separate same-computer and home-lab paths, LAN management hardening, SSH-tunneled Codex OAuth callback, GUI enrollment handoff, Remora connectivity checks, and a Traditional Chinese quick-start. Its 429 analysis distinguishes OpenAI's active-turn continuation from CLIProxyAPI's request-level credential cooldown.
