---
name: ea-handoff-rpa
description: Produces an Enterprise Architecture handoff document for an RPA (Robotic Process Automation) POC built in an AI Builder workshop. Use this skill whenever the user asks to write a handoff doc, prepare a POC for enterprise architecture review, hand off a prototype to EA or IT, create a production-ready version of a POC, or document an automation for the architecture team — when the POC is an automation bot, a Power Automate Desktop flow, a UiPath/Automation Anywhere/Blue Prism flow, a browser-automation script, or any other process automation that drives existing applications without API integration. Also use when the user mentions taking their bot or automation to the enterprise architect, moving it from desktop to server, or making it run unattended. Do not use for LLM/Agent POCs, Document Intelligence POCs, or Predictive ML POCs — those have their own handoff templates.
---

# EA Handoff Document — RPA POCs

## What this skill is for

You are helping a workshop participant — usually an executive or business builder, occasionally the supporting Solution Engineer — produce a handoff document that gets their RPA POC into the hands of their Enterprise Architect. The EA was not in the workshop. They need enough context to evaluate the POC, decide what to build for production, and understand what they're committing to — without being told which specific RPA platform to use.

## Audience and tone

**Audience for the finished document:** the customer's Enterprise Architect or equivalent (sometimes titled "Solution Architect," "Platform Lead," "Director of Engineering," or in RPA-heavy organizations, "Automation CoE Lead"). Technically sophisticated. Skeptical of marketing language. Trusts honesty about limitations more than polish about capabilities.

**Tone:** plain, specific, businesslike. No hedging ("we think," "it might"). No marketing words ("revolutionary," "game-changing," "leveraging"). Numbers where the project supports them. Where it doesn't, name the gap rather than papering over it.

## Core principle: capabilities, not vendors

The POC was likely built on Power Automate Desktop, UiPath Community Edition, or a similar tool that was convenient in the workshop. The EA may build the production version on a different RPA platform, or may use the customer's existing enterprise RPA platform if one is already in place.

**The handoff document describes what the automation needs to do, not which products do it.** Every section except Section 5 ("Where POC Technology Is Non-Negotiable") and Section 8 ("POC Artifacts") should be written so that a reader could not tell which specific RPA platform was used in the POC.

If you find yourself writing the name of a specific product (Power Automate Desktop, UiPath, Automation Anywhere, Blue Prism, etc.) anywhere outside Sections 5 and 8, stop and rewrite the sentence as a capability description.

## Specific failure modes for RPA POCs

RPA POCs systematically over-promise in three ways. Probe for each when reading the project context:

1. **The POC ran on the builder's machine in attended mode.** It worked because the builder was logged in, the apps were already open, network access was fine, and credentials were stored in the OS keychain. Production almost always requires unattended execution on a server, which surfaces entirely new problems — credential management, app launching, session handling, network access through corporate firewalls.

2. **The POC ran on happy-path inputs only.** What happens when the source data has a missing field, an unexpected value, or a different format than the builder tested? What happens when the target application is slow, throws a popup, or is in maintenance? POCs rarely test these.

3. **The POC has no orchestration story.** What triggers the bot? When? Who notices when it fails? Where do failed transactions go? These are usually unanswered.

You should surface all three when filling out the document, especially Section 3 (What Was Learned) and Section 7 (Known Open Problems).

## Workflow

1. **Read the project context first.** Look for the bot definition file, screenshots of the flow, any sample input data, the conversation history with the builder, and workshop notes. The builder has typically been working on this POC for several days inside this project.

2. **Identify gaps.** Anywhere the project does not give you a clear answer to a required section, list the question for the builder rather than guessing. Group questions so the builder can answer many at once.

3. **Draft the document section by section** using the structure below.

4. **Run the self-check** against your own draft before presenting it to the builder.

5. **Present the draft to the builder** with any remaining `[NEEDS INPUT FROM BUILDER]` items called out clearly at the top.

6. **When the builder is satisfied**, offer to export the final document as a .docx file for sharing with EA.

## Never invent facts

When you do not know something, write `[NEEDS INPUT FROM BUILDER: <specific question>]` inline. Do not estimate, infer, or use plausible-sounding placeholder numbers. One invented number poisons the whole document.

---

## Document structure

Use this exact eight-section structure.

### Section 1: Executive Summary

One paragraph, no more than 120 words. It states:

- The business process the POC automates
- What the POC demonstrated (volume processed, time saved, error reduction — whatever the POC measured)
- The order-of-magnitude business value if built for production
- The headline ask of EA (typically: partnership to define the production automation architecture)

No vendor names. No hedging language.

### Section 2: Business Case

**2.1 The process being automated.** Who runs this process today, how often, how long does it take per execution, what does it cost in time or errors. Be concrete about the process — name the steps, the source systems, the target systems, and the people involved.

**2.2 The value if automated.** Capacity unlock is the most common value driver for RPA, though cost reduction, error reduction, and cycle-time compression also apply. Quantify if the project supports it. The math is usually "X executions per month × Y minutes saved per execution × Z fully-loaded labor cost" — be explicit about the inputs to the calculation.

**2.3 How the POC demonstrated value.** What ran? On what data? How many transactions? Was it timed? Was there a comparison to manual baseline? If the POC was demoed end-to-end, capture the result.

### Section 3: What Was Learned

Three subsections, each a short list of 3–6 items.

**3.1 Assumptions that held.** Things the builder expected to work, that worked. Examples: "The target application's UI is stable enough for selectors to remain valid across sessions." "The source data export format is consistent enough to parse reliably."

**3.2 Assumptions that broke.** Things that did not work or required workarounds. RPA-specific examples to probe for: brittle selectors that broke between runs, popups that interrupted the flow, application timeouts that required wait logic, credential prompts that couldn't be handled headlessly, character encoding issues in data, fields that the builder thought were optional but weren't.

**3.3 Things deliberately not tested.** RPA-specific examples to surface: unattended execution on a server, scaled volume, concurrent runs, exception handling, target system under load, the bot running while the builder is logged out, credential rotation, network access through corporate firewalls and proxies.

If 3.2 or 3.3 is empty, ask the builder before letting it ship empty. RPA POCs almost never have nothing in either bucket.

### Section 4: Capability Requirements

Use this format for each capability:

> **Capability:** [one-sentence description of what this component does]
> **Constraints learned in POC:** [throughput, latency, accuracy, volume — anything quantifiable]
> **POC implementation (for context only):** [the specific tool used, one short phrase]
> **Production options EA should consider:** [2–4 categories of solution, not specific products]

Capabilities likely to appear in an RPA POC include some subset of the following. Only include the ones the POC actually exercises:

- Automation execution engine (runs the bot logic against UI/applications)
- Orchestration platform (schedules, queues, dispatches bots; tracks runs)
- Credential vault (stores and provides credentials to unattended bots — almost always required for production)
- Unattended execution infrastructure (server, VM, or cloud capacity to run bots without an interactive user)
- Source system access (read from email, file share, API, database, or UI scrape)
- Target system access (write into the application being automated — UI automation, API, or both)
- Exception handling and dead-letter queue (where do failed transactions go for human review)
- Logging and audit trail (per-transaction record of what was attempted, succeeded, or failed)
- Monitoring and alerting (notification when bots fail, queues back up, or success rates drop)
- Identity for the bot itself (the bot is a "user" in the target systems — it needs its own service account or identity, and this is usually a real provisioning conversation)
- Version control and deployment for bot definitions
- Test environment that mirrors production target systems

### Section 5: Where POC Technology Is Non-Negotiable

Most RPA POC choices are convenience. A few may be load-bearing.

For each non-negotiable item:

> **Component:** [name the specific product]
> **Why it is non-negotiable:** [what specifically this product does that alternatives in the category do not]
> **What changes if EA substitutes:** [the honest answer about what value is lost or what risk is added]

Common reasons something might genuinely be non-negotiable for RPA:

- The customer already has enterprise licensing and a CoE on a specific platform — substituting means duplicating their existing investment
- A specific connector or integration exists in only one platform for one of the target systems
- A unique capability (computer vision for legacy green-screen apps, a particular AI-augmented step) is materially better in one platform

If nothing is genuinely non-negotiable, write: "Nothing in this POC's technology stack is non-negotiable. EA has full latitude on platform selection within the capability constraints in Section 4." This is common — RPA platforms are largely substitutable for straightforward automations.

### Section 6: What EA Needs to Decide

Each decision gets a one-line description, options as understood, and an ownership tag.

**Ownership tags:**

- **EA decides** — purely an architecture or infrastructure call
- **Product/Business decides** — the business owns this; EA implements
- **Joint decision** — needs both perspectives

Decisions almost always present for an RPA POC:

- RPA platform selection, if customer doesn't already have one — *EA decides*, informed by the existing customer estate
- Attended vs. unattended execution model — *Joint* (business decides what the process needs, EA decides what infrastructure supports it)
- Bot identity and service account model — *EA decides*, informed by IT security
- Credential vault and rotation policy — *EA decides*
- Where bots run (on-prem VM, cloud VM, RPA vendor cloud, hybrid) — *EA decides*
- Orchestration and scheduling approach — *EA decides*
- Exception handling and human-in-the-loop queue — *Joint* (business defines what gets escalated, EA builds the queue)
- Monitoring, alerting, and on-call ownership — *EA decides*, informed by who supports the process today
- Change management for the target systems — *Joint* (if the target app's UI changes, who knows, and how does the bot get updated)
- Production SLA and acceptable failure rate — *Product/Business decides*

### Section 7: Known Open Problems

Forward-looking work items. RPA-specific items to surface if applicable:

- Conversion from attended to unattended execution
- Credential handling for unattended runs
- Exception handling design (what to do when the bot fails mid-transaction)
- Target system change management (what happens when the target app updates)
- Selector resilience (how the bot handles UI changes)
- Scale and concurrency testing
- Integration with the customer's existing RPA CoE practices, if one exists

### Section 8: POC Artifacts

- Link to the bot definition file or export
- Screenshots or screen recording of the bot running
- Sample input and output data
- Link to workshop session notes or demo recording
- Names and contact info for the builder, the SE, and the executive sponsor
- If the POC was timed or measured, link to the results

---

## Self-check (run before presenting to the builder)

1. **Vendor neutrality.** Search the draft for specific product names outside Sections 5 and 8. Rewrite as capability descriptions or flag.

2. **No invented facts.** Every quantitative claim traces to the project. If not, replace with `[NEEDS INPUT FROM BUILDER: <question>]`.

3. **Honesty sections populated.** Section 3.2 and 3.3 each have at least two items. RPA POCs almost always have things in both buckets — empty sections mean the builder is being optimistic.

4. **Attended-to-unattended question surfaced.** If the POC ran in attended mode (which most workshop POCs do), the production transition to unattended is explicitly named in Section 6 or Section 7.

5. **Bot identity question surfaced.** Section 6 names the bot identity / service account decision. This is the most common production blocker for RPA and gets ignored in POCs.

6. **Decision ownership tagged.** Every Section 6 item carries one of the three tags exactly.

7. **Open problems are forward-looking.** Section 7 items describe work to do, not findings. Findings belong in Section 3.

8. **Capability requirements match what the POC did.** Cross-reference Section 4 against the actual bot. Remove unused capabilities. Add missing ones.

9. **Executive summary stands alone.** Section 1 reads as self-contained.

When self-check passes, present draft to builder with remaining `[NEEDS INPUT FROM BUILDER]` items called out at the top.

## Output format

Default to producing the draft as markdown in the conversation so the builder can read and react quickly. When the builder is satisfied, offer to export as .docx for sharing with EA.
