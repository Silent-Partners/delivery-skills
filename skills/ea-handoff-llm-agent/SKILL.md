---
name: ea-handoff-llm-agent
description: Produces an Enterprise Architecture handoff document for an LLM-based or Agent-based POC built in an AI Builder workshop. Use this skill whenever the user asks to write a handoff doc, prepare a POC for enterprise architecture review, hand off a prototype to EA or IT, create a production-ready version of a POC, get a workshop POC "made real," or document a POC for the architecture team — even if they do not use the exact phrase "handoff document." Also use when the user mentions taking their POC to the enterprise architect, getting EA sign-off, or moving from prototype to production. Specifically scoped to LLM and Agent-pattern POCs (chatbots, RAG, agents with tool use, single-turn LLM workflows). Do not use for RPA, traditional Document Intelligence (OCR/extraction), or Predictive ML POCs — those have their own handoff templates.
---

# EA Handoff Document — LLM and Agent POCs

## What this skill is for

You are helping a workshop participant — usually an executive or business builder, occasionally the supporting Solution Engineer — produce a handoff document that gets their LLM or Agent POC into the hands of their Enterprise Architect. The EA was not in the workshop. They need enough context to evaluate the POC, decide what to build for production, and understand what they're committing to — without being told which specific products to use.

## Audience and tone

**Audience for the finished document:** the customer's Enterprise Architect or equivalent (sometimes titled "Solution Architect," "Platform Lead," or "Director of Engineering"). Technically sophisticated. Skeptical of marketing language. Trusts honesty about limitations more than polish about capabilities.

**Tone:** plain, specific, businesslike. No hedging ("we think," "it might"). No marketing words ("revolutionary," "game-changing," "leveraging"). Numbers where the project supports them. Where it doesn't, name the gap rather than papering over it.

## Core principle: capabilities, not vendors

The POC was built fast on whatever tools were available in the workshop — commonly Claude, OpenRouter, Supabase, Firebase, and similar. The EA will likely build the production version on a different stack (commonly Azure-centric, sometimes AWS or GCP).

**The handoff document describes what the system needs to do, not which products do it.** Every section except Section 5 ("Where POC Technology Is Non-Negotiable") and Section 8 ("POC Artifacts") should be written so that a reader could not tell which specific products were used in the POC.

If you find yourself writing the name of a specific product (Supabase, Pinecone, OpenRouter, Firebase, Claude, GPT, ChromaDB, etc.) anywhere outside Sections 5 and 8, stop and rewrite the sentence as a capability description. The only exception is when a specific product is the only viable option in its category for this use case — and if you believe that, it belongs in Section 5 with a justification.

## Workflow

1. **Read the project context first.** Look for the POC code, prompts, conversation history with the builder, workshop notes, screenshots, test data, and any earlier discussions in this Claude project. The builder has typically been working on this POC for several days inside this project. Most of the raw material is already here. Do not start asking the builder questions until you have read what's already available.

2. **Identify gaps.** Anywhere the project does not give you a clear answer to a required section, list the question for the builder rather than guessing. Group questions so the builder can answer many at once rather than being interrogated turn by turn.

3. **Draft the document section by section** using the structure below.

4. **Run the self-check** (final section of this skill) against your own draft before presenting it to the builder.

5. **Present the draft to the builder** with any remaining `[NEEDS INPUT FROM BUILDER]` items called out clearly at the top.

6. **When the builder is satisfied**, offer to export the final document as a .docx file for sharing with EA. Most ECAs prefer Word format for review and comment.

## Never invent facts

When you do not know something, write `[NEEDS INPUT FROM BUILDER: <specific question>]` inline. Do not estimate, infer, or use plausible-sounding placeholder numbers. The handoff document loses all credibility with EA the moment they spot a confident statement that turns out to be a guess. One invented number poisons the whole document.

---

## Document structure

Use this exact eight-section structure. Do not add sections. Do not rename sections. EA reviewers across customers benefit from seeing the same shape every time.

### Section 1: Executive Summary

One paragraph, no more than 120 words. It states:

- The business problem the POC addresses
- What the POC demonstrated
- The order-of-magnitude business value if built for production
- The headline ask of EA (typically: partnership to define the production architecture)

No vendor names. No hedging language. If you cannot write this paragraph confidently from the project context, the rest of the document is not ready — go back and ask the builder for what's missing before drafting more sections.

### Section 2: Business Case

Three short subsections. Each is a paragraph, not a list, unless the source material is genuinely enumerable.

**2.1 The problem.** Who has it, how often, what does it cost them today — time, money, errors, risk, missed opportunity. Use the builder's own words from the workshop where possible. EA needs to hear the problem the way the business hears it.

**2.2 The value if solved.** Translate the problem into a business outcome from one of: revenue acceleration, capacity unlock, risk reduction, cost reduction. Quantify if the project contains data to support it. If not, state the value qualitatively and explicitly flag quantification as a follow-up before production scoping.

**2.3 How the POC demonstrated value.** What did the builder test? Who used it? What was the result? If the POC was demoed to stakeholders, capture their reaction. If the POC produced measurable improvement on a test set, cite the numbers with the sample size.

### Section 3: What Was Learned

This is the most important section in the document. EA will trust the rest of the document in proportion to how honest this section is. Three subsections, each a short list of 3–6 items.

**3.1 Assumptions that held.** Things the builder expected to work, that worked. Be specific. "The model can extract structured data from source documents reliably enough for downstream use" is useful. "AI is good at this" is not.

**3.2 Assumptions that broke.** Things the builder expected to work, that did not, or that worked only with workarounds. This is where audit-trail-style honesty lives — if a step in the POC was actually manual when the builder thought it would be automated, name it here. If the model needed three prompt rewrites to produce useful output, name it here. If a particular type of input consistently failed, name it here.

**3.3 Things deliberately not tested.** Edge cases, volume, adversarial input, integration with real source systems, real user load, security review. Name them explicitly so EA knows the scope of what's been validated and what hasn't.

If 3.2 or 3.3 is empty, that is almost certainly the builder being too optimistic, not a clean POC. Ask the builder before letting either section ship empty.

### Section 4: Capability Requirements

This is where the vendor-neutrality rule matters most. Describe each capability the production system will need, the constraints learned in the POC, and a brief context note on what the POC used.

Use this format for each capability:

> **Capability:** [one-sentence description of what this component does]
> **Constraints learned in POC:** [latency, accuracy, volume, data shape — anything quantifiable]
> **POC implementation (for context only):** [the specific tool used, one short phrase]
> **Production options EA should consider:** [2–4 categories of solution, not specific products, unless the category contains only one viable option]

Capabilities likely to appear in an LLM or Agent POC include some subset of the following. Only include the ones the POC actually exercises — do not pad the list:

- Language model with the reasoning depth required for the task
- Prompt and context management (system prompts, few-shot examples, output schemas)
- Conversation state, if the system is multi-turn
- Tool execution layer, if the agent calls tools (describe the tools by capability, not vendor)
- Retrieval over enterprise content, if the system uses RAG (describe content type, freshness requirements, and access control needs)
- Vector or hybrid search, if retrieval is semantic
- Structured data store for application state
- User-facing interface (chat, embedded, voice, API)
- Identity and authorization (almost always required; describe as "the customer's enterprise identity provider," not a specific product)
- Logging, tracing, and observability (every model call, prompt version, tool call, latency, cost)
- Evaluation harness (how the team will know quality has not regressed after changes)
- Human-in-the-loop review, if output is high-stakes or low-confidence cases need escalation
- Cost management and rate limiting
- Content safety and output filtering, if the system is user-facing or handles regulated content

### Section 5: Where POC Technology Is Non-Negotiable

This section is short and used sparingly. Most POC choices were convenience. A few may be load-bearing — meaning, if EA swaps them out, the value demonstrated in the POC does not transfer.

For each non-negotiable item, write:

> **Component:** [name the specific product]
> **Why it is non-negotiable:** [what specifically this product does that alternatives in the category do not, that the POC depended on]
> **What changes if EA substitutes:** [the honest answer about what value is lost or what risk is added]

A non-negotiable claim must be backed by something concrete — eval results, a specific capability gap in alternatives, or a unique integration. "We prefer it" is not a justification.

If nothing in the POC's stack is genuinely non-negotiable, write: "Nothing in this POC's technology stack is non-negotiable. EA has full latitude on tool selection within the capability constraints in Section 4." This is a perfectly fine and common answer.

### Section 6: What EA Needs to Decide

A list of decisions EA must make before production work begins. Each decision gets a one-line description, the options as understood, and an ownership tag.

**Ownership tags — use exactly these three:**

- **EA decides** — purely an architecture or infrastructure call
- **Product/Business decides** — the business owns this; EA implements
- **Joint decision** — needs both perspectives

Decisions almost always present for an LLM or Agent POC:

- Identity and SSO model — typically *EA decides*
- Model hosting and provider — typically *Joint* (EA owns landing zone, business owns whether quality differences matter)
- Data residency and which environments the system can access — *EA decides*
- Logging, observability, and audit trail stack — *EA decides*
- Secrets and key management — *EA decides*
- Cost monitoring and budget alerting — *Joint*
- Evaluation and regression testing approach — *Joint*
- Human-in-the-loop escalation path, if applicable — *Product/Business decides*
- Acceptable error rate and failure modes — *Product/Business decides*
- Retention, deletion, and data subject request handling — *EA decides*, informed by legal

Only include decisions this specific POC actually surfaces. Do not pad.

### Section 7: Known Open Problems

Forward-looking work items — different from Section 3.2 ("assumptions that broke"), which is findings. Short list, each item one or two sentences. Examples:

- Manual steps in the POC workflow that need automation
- Integrations that were mocked or stubbed
- Error handling that was not implemented
- Edge cases observed but not resolved
- Scale, latency, or cost characteristics that are unknown
- Security or compliance review that has not happened yet

### Section 8: POC Artifacts

The one section where naming specific tools is encouraged. EA will use this to dig deeper.

- Link to the POC application or notebook
- Link to prompts and system messages used
- Link to test data and example inputs
- Link to evaluation results, if any exist
- Link to workshop session notes or demo recording
- Names and contact info for the builder, the SE, and the executive sponsor

---

## Self-check (run before presenting to the builder)

Run through every item below against your own draft. Report any failures inline above the draft when you present it. Do not silently pass.

1. **Vendor neutrality.** Search the draft for specific product names outside Sections 5 and 8. If any are present, rewrite them as capability descriptions or flag them.

2. **No invented facts.** Confirm every quantitative claim (time saved, accuracy, volume, cost) traces to something in the project context. If it does not, replace it with `[NEEDS INPUT FROM BUILDER: <specific question>]`.

3. **Honesty sections populated.** Section 3.2 and 3.3 each have at least two items. If either is empty, ask the builder before shipping.

4. **Decision ownership tagged.** Every item in Section 6 carries one of the three ownership tags exactly as written ("EA decides," "Product/Business decides," "Joint decision").

5. **Open problems are forward-looking.** Section 7 items describe work to do, not findings. If an item describes something that happened during the POC, move it to Section 3.

6. **Capability requirements match what the POC did.** Cross-reference Section 4 against the POC code and prompts. Remove capabilities the POC did not exercise. Add capabilities the POC exercised but Section 4 missed.

7. **Executive summary stands alone.** A reader who reads only Section 1 should know what is being asked and roughly why. If not, rewrite it.

When the self-check passes, present the draft to the builder with a note at the top listing any remaining `[NEEDS INPUT FROM BUILDER]` items.

## Output format

Default to producing the draft as markdown in the conversation so the builder can read and react quickly. When the builder is satisfied with the content, offer to export the final document as a .docx file for sharing with EA — this is the format most EA reviewers prefer for comment and markup.
