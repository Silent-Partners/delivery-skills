---
name: ea-handoff-document-intelligence
description: Produces an Enterprise Architecture handoff document for a Document Intelligence POC built in an AI Builder workshop. Use this skill whenever the user asks to write a handoff doc, prepare a POC for enterprise architecture review, hand off a prototype to EA or IT, create a production-ready version of a POC, or document an extraction or classification pipeline for the architecture team — when the POC processes documents to extract fields, classify document types, parse forms, read invoices, summarize contracts, or otherwise turn unstructured document content into structured data. Also use when the user mentions taking their document processing or OCR pipeline to the enterprise architect, going from batch processing to production volume, or moving documents from a sandbox into the customer's real document estate. Do not use for LLM/Agent POCs, RPA POCs, or Predictive ML POCs — those have their own handoff templates.
---

# EA Handoff Document — Document Intelligence POCs

## What this skill is for

You are helping a workshop participant produce a handoff document that gets their Document Intelligence POC into the hands of their Enterprise Architect. The EA was not in the workshop. They need enough context to evaluate the POC, decide what to build for production, and understand what they're committing to — without being told which specific extraction service to use.

## Audience and tone

**Audience for the finished document:** the customer's Enterprise Architect or equivalent. Technically sophisticated. Skeptical. Trusts honesty about limitations.

**Tone:** plain, specific, businesslike. No hedging. No marketing language. Numbers where supported, gaps named where not.

## Core principle: capabilities, not vendors

The POC was likely built on Azure Document Intelligence, AWS Textract, Google Document AI, an LLM-based extraction approach (sending document images or text to a multimodal model), or a combination. The EA will likely build the production version on whatever cloud platform the customer already uses, possibly with a different extraction approach.

**The handoff document describes what the pipeline needs to do, not which products do it.** Every section except Section 5 and Section 8 should be written so a reader could not tell which specific service was used.

If you find yourself naming a specific product (Azure Document Intelligence, Textract, Document AI, Claude, GPT, etc.) outside Sections 5 and 8, rewrite as a capability description.

## Specific failure modes for Document Intelligence POCs

Document Intelligence POCs systematically over-promise in three ways. Probe for each when reading the project context:

1. **The POC ran on a curated sample of documents.** The builder picked 10–30 documents that worked. The real document population has variance the POC never saw: scanned at different angles, multi-page forms with continuation pages, handwritten annotations, watermarks, mixed languages, malformed exports, password-protected files, very large files, very small files, OCR-resistant fonts. Production accuracy on real document populations is almost always meaningfully lower than POC accuracy on curated samples.

2. **Accuracy was measured on the same documents that were used to design the prompts or templates.** This is a train/test leak. The 95% accuracy figure means "this prompt extracts these documents," not "this prompt generalizes to new documents."

3. **The "happy path" hides the unhappy path.** What happens when the model isn't confident? What happens when it extracts a field that doesn't exist in the schema? What happens when a required field is missing from the document? POCs rarely answer these.

Surface all three in Section 3 (What Was Learned) and Section 7 (Known Open Problems).

## Workflow

1. **Read the project context first.** Look for sample documents, extraction prompts or templates, accuracy results, conversation history with the builder, and workshop notes.

2. **Identify gaps.** List questions for the builder rather than guessing.

3. **Draft the document section by section** using the structure below.

4. **Run the self-check** before presenting to the builder.

5. **Present the draft** with any remaining `[NEEDS INPUT FROM BUILDER]` items called out at the top.

6. **When the builder is satisfied**, offer to export as .docx for sharing with EA.

## Never invent facts

`[NEEDS INPUT FROM BUILDER: <specific question>]` when you don't know. Especially do not invent accuracy numbers — they are the single most poisonous invented fact in a Document Intelligence handoff.

---

## Document structure

### Section 1: Executive Summary

One paragraph, no more than 120 words. States:

- The document type(s) and the extraction or classification task
- What the POC demonstrated, including sample size and headline accuracy
- The order-of-magnitude business value if built for production
- The headline ask of EA

No vendor names. No hedging.

### Section 2: Business Case

**2.1 The document processing problem.** Who handles these documents today, how many per month, how long does processing take per document, what does it cost. Name the documents specifically — "vendor invoices in PDF and image formats" not just "documents."

**2.2 The value if automated.** Common Document Intelligence value drivers: capacity unlock (humans freed from data entry), cycle-time compression (documents processed in minutes instead of days), error reduction (consistent extraction vs. human transcription error). Quantify if the project supports it. If the value depends on a target accuracy level, name the level — "automated extraction at 90%+ accuracy with human review for the rest unlocks roughly X hours per month" is honest; "automation eliminates data entry" usually isn't.

**2.3 How the POC demonstrated value.** Sample size. Document variance in the sample. Accuracy by field. Time per document. Comparison to manual baseline if one was measured. If the accuracy figure was measured on the same documents used to design the extraction approach, say so explicitly — this is critical for EA to interpret the number correctly.

### Section 3: What Was Learned

Three subsections, each a short list.

**3.1 Assumptions that held.** Examples: "Vendor invoices follow a small number of layout patterns, and the extraction approach handles all observed patterns." "Field-level accuracy on the top three required fields exceeds the manual baseline."

**3.2 Assumptions that broke.** Document Intelligence-specific examples to probe for: scanned-vs-digital handling differences, fields the builder thought were always present but sometimes weren't, document layouts that broke extraction, multi-page documents where continuation pages caused issues, languages other than the test set, low-quality scans, handwriting, stamps and signatures interfering with text recognition, fields with formatting variance (dates, currencies, addresses).

**3.3 Things deliberately not tested.** Document Intelligence-specific: full document population variance (vs. the curated sample), production volume, latency at scale, low-confidence handling, malformed inputs, document types outside the trained scope, integration with the source system where documents actually live (mailbox, SharePoint, DMS, FTP), integration with the destination system where extracted data goes, retention and deletion of processed documents, PII handling.

If 3.2 or 3.3 is empty, the builder is being optimistic — ask before shipping.

### Section 4: Capability Requirements

Use this format for each capability:

> **Capability:** [one-sentence description]
> **Constraints learned in POC:** [accuracy, throughput, latency, document size, variance — quantifiable]
> **POC implementation (for context only):** [specific tool, short phrase]
> **Production options EA should consider:** [2–4 categories, not specific products]

Capabilities likely in a Document Intelligence POC. Only include the ones the POC actually exercises:

- Document ingestion pipeline (how documents enter the system — email, file share, upload, API)
- Document classification (deciding what kind of document this is, if the pipeline handles multiple types)
- OCR or text extraction (turning images into text, if documents are scanned)
- Structured extraction (turning text into fields per a defined schema)
- Confidence scoring and thresholding (per-field confidence, deciding what's automated vs. reviewed)
- Human-in-the-loop review interface (where humans confirm or correct low-confidence extractions — this is almost always required)
- Output integration (writing extracted data into the destination system: ERP, accounting system, CRM, database)
- Document storage and retention (where original documents live after processing, for how long, with what access controls)
- Audit trail (per-document record of what was extracted, with what confidence, by which version of the model or prompt)
- Quality monitoring (tracking accuracy over time, detecting drift, sampling for manual QA)
- Retraining or prompt-updating workflow (how does the extraction approach improve when new document patterns appear)
- PII identification and handling, if documents contain personal data

### Section 5: Where POC Technology Is Non-Negotiable

For each non-negotiable item:

> **Component:** [specific product]
> **Why it is non-negotiable:** [specific capability that alternatives lack]
> **What changes if EA substitutes:** [what value is lost or risk added]

Common reasons something might be genuinely non-negotiable for Document Intelligence:

- A specific model where extraction accuracy on the customer's document type is materially higher than alternatives, backed by side-by-side eval data
- A specific service where prebuilt models for a regulated document type (e.g., specific tax forms, specific medical documents) exist that would otherwise require custom training
- Multimodal LLM extraction approach where reasoning over document context is required and template-based extractors cannot do it

If nothing is genuinely non-negotiable, write: "Nothing in this POC's technology stack is non-negotiable. EA has full latitude on extraction service selection within the capability constraints in Section 4."

### Section 6: What EA Needs to Decide

Ownership tags: **EA decides** / **Product/Business decides** / **Joint decision**

Decisions almost always present for a Document Intelligence POC:

- Extraction service or platform selection — *EA decides*, informed by the customer's existing cloud estate and accuracy requirements
- Document storage location and retention policy — *EA decides*, informed by legal
- PII and regulated data handling — *EA decides*, informed by compliance
- Confidence threshold for automation vs. human review — *Product/Business decides* (business owns the accuracy/throughput tradeoff)
- Human review interface and workflow — *Joint* (business owns the user experience, EA builds the infrastructure)
- Integration with source system (where documents come from) — *EA decides*
- Integration with destination system (where extracted data goes) — *EA decides*
- Acceptable error rate and how errors propagate downstream — *Product/Business decides*
- Quality monitoring and sampling cadence — *Joint*
- Retraining or prompt-updating cadence — *Joint*

### Section 7: Known Open Problems

Forward-looking work items. Document Intelligence-specific:

- Validation on full document population (not just curated samples)
- Performance on document variance not seen in the POC
- Low-confidence handling and human review workflow
- Source system integration (where documents actually live in production)
- Destination system integration (where extracted data flows)
- PII handling and access control on processed documents
- Quality monitoring infrastructure
- Cost projection at production volume

### Section 8: POC Artifacts

- Sample documents (representative subset, ideally with examples of both successful and failed extractions)
- Extraction prompts, templates, or model configuration
- Accuracy results with sample size and methodology noted
- Confusion matrix or per-field accuracy breakdown if available
- Link to workshop session notes or demo recording
- Names and contact info for the builder, the SE, and the executive sponsor

---

## Self-check (run before presenting to the builder)

1. **Vendor neutrality.** Specific product names outside Sections 5 and 8 are rewritten or flagged.

2. **No invented facts.** Especially: no invented accuracy numbers. If the project didn't measure accuracy, Section 2.3 must say so.

3. **Sample size and methodology stated.** Any accuracy figure in the document is accompanied by the sample size and a note on whether the evaluation set was independent of the design set.

4. **Honesty sections populated.** Section 3.2 and 3.3 each have at least two items. Document Intelligence POCs almost always have things in both buckets.

5. **Document variance question surfaced.** The handoff explicitly addresses what document variance was and wasn't tested.

6. **Human-in-the-loop question surfaced.** Section 4 or Section 6 names the human review workflow. Almost no Document Intelligence pipeline runs at 100% automation in production.

7. **Decision ownership tagged.** Every Section 6 item carries one of the three tags exactly.

8. **Open problems are forward-looking.** Section 7 items describe work to do, not findings.

9. **Capability requirements match what the POC did.**

10. **Executive summary stands alone.**

When self-check passes, present draft to builder with remaining `[NEEDS INPUT FROM BUILDER]` items at the top.

## Output format

Default to markdown in conversation. Offer .docx export when the builder is satisfied.
