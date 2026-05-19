# Changelog

Version history for the AI Builder Skills methodology. Each entry describes what changed in a skill and what prompted the change. When updating a skill, add an entry here so SEs delivering future engagements can see what's evolved.

Versions are dated rather than semver — methodology shifts don't map cleanly to major/minor/patch.

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
