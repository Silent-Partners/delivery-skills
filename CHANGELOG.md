# Changelog

Version history for the AI Builder Skills methodology. Each entry describes what changed in a skill and what prompted the change. When updating a skill, add an entry here so SEs delivering future engagements can see what's evolved.

Versions are dated rather than semver — methodology shifts don't map cleanly to major/minor/patch.

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
