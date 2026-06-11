# AI Builder Skills

Methodology assets for the AI Builder Transformation Program — Claude Code skills, rubrics, and supporting templates used by Silent Partners SEs to deliver workshop POCs and hand them off to customer Enterprise Architecture teams.

This repository is internal to Silent Partners. Do not share the skill files directly with customers in raw form. The skills are designed to be installed on a workshop participant's machine by the supporting SE as part of the engagement — they are not self-serve.

---

## What's in here

```
ai-builder-skills/
├── README.md                              (this file — SE reference)
├── CHANGELOG.md                           (methodology version history)
├── INSTALL.md                             (exec-facing install instructions)
├── .claude-plugin/
│   └── marketplace.json                   (plugin marketplace — lists studio-build)
├── plugins/
│   └── studio-build/                      (build phase — self-serve plugin)
│       ├── .claude-plugin/plugin.json
│       └── skills/studio-build/SKILL.md
└── skills/                                (handoff phase — SE-delivered folders)
    ├── ea-handoff-llm-agent/              (LLM and Agent POCs)
    │   └── SKILL.md
    ├── ea-handoff-rpa/                    (RPA POCs)
    │   └── SKILL.md
    ├── ea-handoff-document-intelligence/  (Document Intelligence POCs)
    │   └── SKILL.md
    └── ea-handoff-predictive-ml/          (Predictive ML POCs)
        └── SKILL.md
```

The skills split across two phases of the engagement, and the split drives how each is
delivered:

- **`studio-build` (build phase, self-serve plugin).** Governs how Claude builds the
  scoped slice with the participant in Claude Code. Distributed as a **plugin** via the
  marketplace in this repo — the participant installs it **once** and it persists across
  every build. Source of truth is `build-experience-v1.md` in the Studio app repo; the
  copy here is the synced runtime artifact.
- **`ea-handoff-*` (handoff phase, SE-delivered folders).** Run at the **end**, once a POC
  exists, to produce the Enterprise Architecture handoff document. Still delivered the old
  way — the SE sends the participant the relevant skill folder (see "How to deliver a
  skill" below).

They never overlap; a build is never a handoff, and `studio-build`'s description steers
Claude away from handoff-doc requests so the wrong skill never fires.

### Installing the studio-build plugin

The participant runs these two commands once in Claude Code (the repo is public, no auth):

```
/plugin marketplace add silent-partners/delivery-skills
/plugin install studio-build@silent-partners
```

After that, the skill is always present — the Studio's per-build starter prompt just
invokes it ("Use the studio-build skill…") and never re-installs.

Planned additions:

- `poc-handoff-readiness-rubric` — SE-facing gate to assess whether a POC is ready for handoff
- A Generative AI pattern skill is deliberately not planned separately — most generative AI POCs fall under `ea-handoff-llm-agent`. If a generative use case emerges that doesn't fit that skill, we'll add one then rather than building speculatively.

---

## When to use which skill

| Pattern | Skill to use |
|---|---|
| Building the scoped slice with the participant (start of the build, post-interview) | `studio-build` |
| LLM-based assistants, chatbots, RAG, agents with tool use, single-turn LLM workflows, generative AI use cases | `ea-handoff-llm-agent` |
| RPA / process automation, browser automation, desktop automation | `ea-handoff-rpa` |
| Document Intelligence — OCR, extraction, classification, form parsing | `ea-handoff-document-intelligence` |
| Predictive ML — classification, forecasting, scoring, recommendation on tabular or structured data | `ea-handoff-predictive-ml` |

If a POC spans more than one pattern, pick the skill matching the dominant pattern and supplement with prose for the secondary capability. Do not try to merge handoff docs across patterns — EA reviewers benefit from consistent shape per pattern.

---

## How to deliver a skill to a workshop participant

The workshop participant should never need to interact with this repo directly. The flow is:

1. Clone or pull this repo on your own machine (`git clone git@github.com:silentpartners/ai-builder-skills.git`).
2. Identify the right skill folder for the participant's POC pattern (see table above).
3. Send the participant two things:
   - The skill folder (e.g., `skills/ea-handoff-llm-agent/`) — zip it, attach in Slack, drop in their workshop Google Drive folder, whatever is convenient.
   - The `INSTALL.md` file from this repo — they read this once to get the skill installed on their machine.
4. Confirm with them they can see the skill in their Claude Code session (running `/skills` lists installed skills) before the handoff conversation begins.

The reason the participant doesn't clone the repo themselves is that we don't want to add them to a Silent Partners GitHub org for a 12-week engagement, and we don't want methodology updates flowing to them automatically — they should use the version that was current when their POC was built.

---

## How to update a skill

Methodology evolves. When you learn something from a real engagement that should change a skill:

1. Branch from `main` (`git checkout -b update-llm-agent-honesty-section`).
2. Edit the relevant `SKILL.md`.
3. Update `CHANGELOG.md` with a new entry describing what changed and why. Reference the engagement or pattern that prompted the change if possible.
4. Open a PR. At least one other SE reviews before merging.

If you're updating in response to something a customer flagged, capture their feedback in the CHANGELOG entry. That's how the methodology improves over time rather than drifting on individual preferences.

---

## How to add a new skill

When adding a new pattern skill or supporting skill:

1. Create a new folder under `skills/` with a kebab-case name matching the skill's `name` frontmatter field.
2. The folder must contain a `SKILL.md` file with valid frontmatter (`name` and `description` fields required).
3. The `description` field is what triggers the skill in Claude Code, so write it to fire on phrases real users say, not just the technical name of the artifact.
4. Update the "What's in here" tree and the "When to use which skill" table in this README.
5. Add a CHANGELOG entry under a new version.
6. Open a PR. New skills get an extra review pass — the description field especially.

---

## Maintainers

- Richard — lead author, methodology owner
- Ryan — co-maintainer, review on all PRs
