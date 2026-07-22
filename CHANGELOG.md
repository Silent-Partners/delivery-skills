# Changelog

Version history for the AI Builder Skills methodology. Each entry describes what changed in a skill and what prompted the change. When updating a skill, add an entry here so SEs delivering future engagements can see what's evolved.

Versions are dated rather than semver — methodology shifts don't map cleanly to major/minor/patch.

---

## 2026-07-22

**`studio-build` companion genericized + made tool-agnostic; SKILL.md now generated**

The build companion is no longer Claude-Code-specific. Its behavioral body was genericized
("your AI build tool" / "the tool" instead of "Claude Code") so the same companion runs
across agentic engines (Claude Code, Gemini CLI, Copilot agent, Cursor) and instructed-chat
tools. Prompted by the tool-agnostic handoff work in the Studio (customers standardize on
different vendors; requiring Claude opened a procurement fight).

The single source of truth is now `src/content/build-companion-body.md` in the Studio repo.
This plugin's `SKILL.md` is **generated** from it via `npm run gen:companions` (the generator
preserves this file's hand-tuned frontmatter and swaps only the body). Non-Claude tools get
the same body as a download from the Studio handoff screen. Do not hand-edit the body here —
edit it in the Studio and regenerate.

---

## 2026-06-11 (later same day)

**Packaged `studio-build` as a self-serve plugin**

Moved `skills/studio-build/` to `plugins/studio-build/` and added a plugin marketplace
(`.claude-plugin/marketplace.json` + `plugins/studio-build/.claude-plugin/plugin.json`).
The repo is now public, so participants install the build skill themselves with two
commands — `/plugin marketplace add silent-partners/delivery-skills` then `/plugin install
studio-build@silent-partners` — once, persisting across every build.

This decouples install from the per-build starter prompt: the Studio no longer emits an
"install the skill" line each time. Install is a one-time onboarding step (shown on the
handoff page); the per-build prompt just invokes the already-present skill.

Deferred for now (revisit with real users): bundling safe permission defaults. A plugin
can't set `defaultMode` or `permissions.deny` (only `agent`/`subagentStatusLine` are
honored from plugin settings), so safety config would need a one-time write to the user's
settings or a cross-platform PreToolUse hook. Out of scope for v1 — these are low-risk
internal builds.

The `ea-handoff-*` skills are unchanged and still SE-delivered as folders.

---

## 2026-06-11

**Added: `studio-build`**

First build-phase skill, distinct from the `ea-handoff-*` handoff-phase family. Governs
how Claude behaves while building the scoped slice with a non-technical participant in
Claude Code: move fast down the happy path, hold the interview-agreed slice, stop only on
expensive-to-reverse forks, surface fragility honestly, and provoke testing against the
participant's real input. Ends by routing the participant back to the Studio to log the
build.

Prompted by a deployment gap: the Studio's self-service interview emits a starter prompt
ending with "install the studio-build skill from this repo," but only the handoff skills
were deployed here — so participants' Claude Code had no build-phase skill to resolve to
and would fall through to a handoff skill (wrong tool for the phase). The methodology had
been designed (`build-experience-v1.md` in the Studio app repo) but never deployed as a
runnable skill.

Source of truth is `build-experience-v1.md` in the Studio app repo; the `SKILL.md` here is
the synced runtime copy plus frontmatter. The `description` is written to fire on build
intent ("start the build with me," arriving from a Studio interview with a scoped slice)
and to explicitly steer away from handoff-doc requests, so Claude Code never confuses the
build phase with the handoff phase.

---

## 2026-05-19 (later same day)

**Added: `ea-handoff-rpa`, `ea-handoff-document-intelligence`, `ea-handoff-predictive-ml`**

Drafted the other three pattern handoff skills, completing first-pass coverage of the AI tool taxonomy (Generative AI is intentionally folded into `ea-handoff-llm-agent` rather than getting its own skill).

All three skills share the same eight-section structure as `ea-handoff-llm-agent`, the same three decision ownership tags, and the same self-check approach. Pattern-specific divergence is in:

- The "specific failure modes" section near the top of each SKILL.md, which encodes how each pattern tends to over-promise in workshop POCs
- The capability requirements list in Section 4
- The decisions list in Section 6
- The self-check items that catch pattern-specific blind spots (attended-vs-unattended for RPA, evaluation methodology for Document Intelligence, feature availability and business integration for Predictive ML)

**Caveat on these three:** unlike `ea-handoff-llm-agent`, which was grounded in the Pacific Hospitality Group engagement, these three were drafted from general practice rather than a specific recent engagement. They should be pressure-tested against a real workshop POC of each pattern before being considered solid. The failure modes encoded in each are the canonical ones, but customer-specific patterns will likely emerge that should be added.

---

## 2026-05-19

**Added: `ea-handoff-llm-agent`**

Initial release of the EA handoff document skill for LLM and Agent-pattern POCs. Eight-section structure: Executive Summary, Business Case, What Was Learned, Capability Requirements, Where POC Technology Is Non-Negotiable, What EA Needs to Decide, Known Open Problems, POC Artifacts.

Key methodology principles encoded in this version:

- Vendor-neutral capability framing — the handoff doc describes what the system needs to do, not which products do it, except where a specific product is genuinely the only option in its category
- Three-bucket honesty section — assumptions that held, assumptions that broke, things deliberately not tested — with the skill flagging empty "broke" or "not tested" sections as almost certainly the builder being too optimistic
- Three-tag decision ownership — EA decides / Product/Business decides / Joint decision — to prevent EA from feeling like they're being told what to do or quietly swapping out load-bearing POC choices
- Post-draft self-check — the skill validates its own output against vendor neutrality, invented facts, populated honesty sections, decision ownership tags, and capability/POC alignment before presenting the draft

Origin: built off learnings from the Pacific Hospitality Group engagement (five POCs, formal defense documentation, Azure AD as a shared blocker) and the AI Builder Transformation Program workshop format.
