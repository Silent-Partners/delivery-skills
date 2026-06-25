# Silent Partners Skills

The shared library of Claude Code skills used across Silent Partners — delivery methodology, design-engineering craft, and supporting templates. This is the org's one home for skills we want teammates to install on their machines.

The library currently holds two families:

- **Delivery skills** — methodology assets for the AI Builder Transformation Program: rubrics and handoff guides Silent Partners SEs use to deliver workshop POCs and hand them off to customer Enterprise Architecture teams.
- **Design-engineering skills** — craft skills that raise the bar on UI, animation, and motion design. Useful on any frontend we build.

This repository is internal to Silent Partners. Do not share the delivery skill files directly with customers in raw form — they are designed to be installed on a workshop participant's machine by the supporting SE as part of the engagement, not self-serve. Some skills are vendored from third parties under their own licenses (see [Licensing & attribution](#licensing--attribution)).

---

## What's in here

```
delivery-skills/
├── README.md                              (this file — team reference)
├── CHANGELOG.md                           (version history)
├── INSTALL.md                             (install instructions for a single skill folder)
└── skills/
    ├── ea-handoff-llm-agent/              (LLM and Agent POCs)
    │   └── SKILL.md
    ├── ea-handoff-rpa/                    (RPA POCs)
    │   └── SKILL.md
    ├── ea-handoff-document-intelligence/  (Document Intelligence POCs)
    │   └── SKILL.md
    ├── ea-handoff-predictive-ml/          (Predictive ML POCs)
    │   └── SKILL.md
    └── motion-design/                     (design-engineering craft — vendored, MIT)
        ├── LICENSE.txt
        ├── emil-design-eng/               (UI polish, component & animation decisions)
        │   └── SKILL.md
        └── review-animations/             (strict animation/motion code review)
            ├── SKILL.md
            └── STANDARDS.md
```

Planned additions:

- `poc-handoff-readiness-rubric` — SE-facing gate to assess whether a POC is ready for handoff
- A Generative AI pattern skill is deliberately not planned separately — most generative AI POCs fall under `ea-handoff-llm-agent`. If a generative use case emerges that doesn't fit that skill, we'll add one then rather than building speculatively.

---

## When to use which skill

### Delivery skills

| Pattern | Skill to use |
|---|---|
| LLM-based assistants, chatbots, RAG, agents with tool use, single-turn LLM workflows, generative AI use cases | `ea-handoff-llm-agent` |
| RPA / process automation, browser automation, desktop automation | `ea-handoff-rpa` |
| Document Intelligence — OCR, extraction, classification, form parsing | `ea-handoff-document-intelligence` |
| Predictive ML — classification, forecasting, scoring, recommendation on tabular or structured data | `ea-handoff-predictive-ml` |

If a POC spans more than one pattern, pick the skill matching the dominant pattern and supplement with prose for the secondary capability. Do not try to merge handoff docs across patterns — EA reviewers benefit from consistent shape per pattern.

### Design-engineering skills

| Need | Skill to use |
|---|---|
| Building or polishing UI, choosing animation easing/timing, component design, the invisible details that make software feel great | `motion-design/emil-design-eng` |
| Reviewing animation or motion code against a strict craft bar before it ships | `motion-design/review-animations` |

`emil-design-eng` activates automatically when you're working on UI craft. `review-animations` is review-only (`disable-model-invocation: true`) — invoke it deliberately when you want an animation critique.

---

## Installing a skill on your machine

Each skill is a self-contained folder containing a `SKILL.md`. To install one, copy the **leaf** skill folder (the one that directly contains `SKILL.md`) into `~/.claude/skills/` — for example `skills/motion-design/emil-design-eng/` becomes `~/.claude/skills/emil-design-eng/`. Do not copy the `motion-design/` grouping folder itself; that's just organization inside this repo.

```bash
# Mac / Linux / WSL
mkdir -p ~/.claude/skills
cp -R skills/motion-design/emil-design-eng ~/.claude/skills/
cp -R skills/motion-design/review-animations ~/.claude/skills/
```

Confirm with `/skills` in a Claude Code session that the skill is listed. `INSTALL.md` has the full step-by-step for non-technical teammates.

## How to deliver a skill to a workshop participant

The workshop participant should never need to interact with this repo directly. The flow is:

1. Clone or pull this repo on your own machine (`git clone git@github.com:Silent-Partners/delivery-skills.git`).
2. Identify the right skill folder for the participant's POC pattern (see table above).
3. Send the participant two things:
   - The skill folder (e.g., `skills/ea-handoff-llm-agent/`) — zip it, attach in Slack, drop in their workshop Google Drive folder, whatever is convenient.
   - The `INSTALL.md` file from this repo — they read this once to get the skill installed on their machine.
4. Confirm with them they can see the skill in their Claude Code session (running `/skills` lists installed skills) before the handoff conversation begins.

The reason the participant doesn't clone the repo themselves is that we don't want to add them to a Silent Partners GitHub org for a 12-week engagement, and we don't want methodology updates flowing to them automatically — they should use the version that was current when their POC was built.

---

## How to update a skill

Methodology and craft both evolve. When you learn something that should change a skill:

1. Branch from `main` (`git checkout -b update-llm-agent-honesty-section`).
2. Edit the relevant `SKILL.md`.
3. Update `CHANGELOG.md` with a new entry describing what changed and why. Reference the engagement or pattern that prompted the change if possible.
4. Open a PR. At least one other teammate reviews before merging.

If you're updating in response to something a customer flagged, capture their feedback in the CHANGELOG entry. That's how the library improves over time rather than drifting on individual preferences.

For vendored third-party skills (like `motion-design/`), prefer pulling upstream updates over hand-editing, so we can track their changes — and keep the upstream `LICENSE` file in place.

---

## How to add a new skill

When adding a new pattern skill or supporting skill:

1. Create a new folder under `skills/` (or under a category folder like `motion-design/`) with a kebab-case name matching the skill's `name` frontmatter field.
2. The folder must contain a `SKILL.md` file with valid frontmatter (`name` and `description` fields required).
3. The `description` field is what triggers the skill in Claude Code, so write it to fire on phrases real users say, not just the technical name of the artifact.
4. Update the "What's in here" tree and the relevant "When to use which skill" table in this README.
5. Add a CHANGELOG entry under a new version.
6. If the skill is vendored from a third party, include its upstream `LICENSE` file and note the source and license in [Licensing & attribution](#licensing--attribution).
7. Open a PR. New skills get an extra review pass — the description field especially.

---

## Licensing & attribution

- **`motion-design/`** (`emil-design-eng`, `review-animations`) — vendored from [emilkowalski/skills](https://github.com/emilkowalski/skills), authored by Emil Kowalski. MIT licensed; the upstream license is kept at `skills/motion-design/LICENSE.txt`.

---

## Maintainers

- Richard — lead author, delivery methodology owner
- Ryan — co-maintainer, review on all PRs
