---
name: ea-handoff-predictive-ml
description: Produces an Enterprise Architecture handoff document for a Predictive ML POC built in an AI Builder workshop. Use this skill whenever the user asks to write a handoff doc, prepare a POC for enterprise architecture review, hand off a prototype to EA or IT, create a production-ready version of a POC, or document a machine learning model for the architecture team — when the POC is a classification model, a forecasting model, a scoring or ranking model, a churn or fraud predictor, a recommendation model, or any other trained model that produces predictions on tabular or structured data. Also use when the user mentions taking their model to the enterprise architect, moving from a notebook to production, deploying a model, or operationalizing predictions. Do not use for LLM/Agent POCs, RPA POCs, or Document Intelligence POCs — those have their own handoff templates.
---

# EA Handoff Document — Predictive ML POCs

## What this skill is for

You are helping a workshop participant produce a handoff document that gets their Predictive ML POC into the hands of their Enterprise Architect. The EA was not in the workshop. They need enough context to evaluate the POC, decide what to build for production, and understand what they're committing to — without being told which specific ML platform to use.

## Audience and tone

**Audience for the finished document:** the customer's Enterprise Architect or equivalent, sometimes a Data and Analytics leader, sometimes an MLOps or ML Platform lead if the customer is mature in this space. Technically sophisticated. Skeptical of "great accuracy on the holdout set" claims. Trusts honesty about deployability more than polish about model performance.

**Tone:** plain, specific, businesslike. No hedging. No marketing language. Numbers where the project supports them, with sample sizes and methodology stated.

## Core principle: capabilities, not vendors

The POC was likely built in a notebook (Jupyter, Colab, Databricks, Hex), possibly using AutoML (Azure ML Designer, Vertex AI AutoML, H2O), or a hand-built scikit-learn / XGBoost / similar pipeline. The EA will likely build the production version using the customer's existing ML platform if one exists, or stand up new platform capability if not.

**The handoff document describes what the prediction pipeline needs to do, not which products do it.** Every section except Section 5 and Section 8 should be written so a reader could not tell which specific tools were used.

If you find yourself naming a specific product (scikit-learn, XGBoost, Azure ML, SageMaker, Vertex AI, Databricks, MLflow, etc.) outside Sections 5 and 8, rewrite as a capability description.

## Specific failure modes for Predictive ML POCs

Predictive ML POCs systematically over-promise in four ways. Probe for each when reading the project context:

1. **The model has good metrics on a static dataset but no deployment story.** The POC produces a notebook with accuracy numbers; production needs feature pipelines, model serving infrastructure, monitoring, and a retraining workflow. None of these typically exist in the POC.

2. **The train/test split doesn't represent production.** Random splits leak information when there's a time dimension (temporal leakage), an entity dimension (predicting customer churn when the same customer appears in train and test), or when the holdout was hand-curated. A 92% accuracy on a random split can collapse to 65% on a proper out-of-time evaluation.

3. **The feature pipeline only exists at training time.** The model was trained on features that took hours to compute from a data warehouse. In production, predictions need to be made in seconds, and the source data may not be available at all in the production system. This is the most common showstopper for ML POCs going to production.

4. **The business integration is unclear.** Even if the model is deployed, who acts on the predictions? When? In what tool? POCs almost universally hand-wave this. A model that nobody uses produces zero value regardless of its accuracy.

Surface all four when filling out the document. The feature pipeline question and the business integration question are the two most important.

## Workflow

1. **Read the project context first.** Look for the notebook, the dataset, accuracy results, train/test split methodology, any feature engineering steps, conversation history with the builder, and workshop notes.

2. **Identify gaps.** List questions for the builder rather than guessing.

3. **Draft the document section by section** using the structure below.

4. **Run the self-check** before presenting to the builder.

5. **Present the draft** with any remaining `[NEEDS INPUT FROM BUILDER]` items called out at the top.

6. **When the builder is satisfied**, offer to export as .docx for sharing with EA.

## Never invent facts

`[NEEDS INPUT FROM BUILDER: <specific question>]` when you don't know. Especially do not invent or estimate model metrics — they are the most poisonous invented fact in a Predictive ML handoff. And if the project's reported metrics come from a methodology that has obvious problems (random split on time-series data, evaluation on data used for feature selection), say so explicitly rather than reporting the metric uncritically.

---

## Document structure

### Section 1: Executive Summary

One paragraph, no more than 120 words. States:

- The prediction task and what business decision it informs
- What the POC demonstrated, including headline metric, sample size, and a note on evaluation methodology
- The order-of-magnitude business value if built for production
- The headline ask of EA

No vendor names. No hedging.

### Section 2: Business Case

**2.1 The prediction problem and the decision it informs.** What is being predicted, for whom, at what cadence. Critically: what business decision changes based on the prediction. A model that produces predictions nobody acts on has no business value, so this question must be answered concretely. "Predict customer churn 60 days out so that retention team can prioritize outreach to high-risk accounts" is a real answer; "predict customer churn" is not.

**2.2 The value if operationalized.** Common Predictive ML value drivers: revenue acceleration (better targeting, better pricing), capacity unlock (better prioritization of human effort), risk reduction (fraud detection, default prediction), cost reduction (predictive maintenance, demand forecasting). Quantify if the project supports it. The math usually requires assumptions about how the prediction translates to action and how the action translates to outcome — be explicit about these assumptions.

**2.3 How the POC demonstrated value.** Sample size. Train/test split methodology. Headline metric (accuracy, AUC, RMSE, F1, business-specific metric). Critically: was the evaluation methodology appropriate for the production use case? If the data has a time dimension and the split was random, say so. If the model was evaluated on data used for feature selection, say so.

### Section 3: What Was Learned

Three subsections, each a short list.

**3.1 Assumptions that held.** Examples: "The source data has sufficient signal — the model achieves materially better performance than the naive baseline." "Class imbalance was manageable with standard techniques."

**3.2 Assumptions that broke.** Predictive ML-specific examples to probe for: missing data patterns that weren't handled, features that turned out to be unavailable at prediction time (target leakage), categorical levels in production that didn't exist in training, distributions that drifted between the data the builder pulled and the data available now, business definitions that were unclear (what counts as "churn"), the naive baseline being closer to the model than expected.

**3.3 Things deliberately not tested.** Predictive ML-specific examples to surface: out-of-time evaluation, evaluation on a separate population, feature availability at prediction time in the production system, latency requirements for real-time scoring, scaling to full production volume, drift over time, fairness across protected attributes, retraining workflow, A/B test or shadow mode comparison to baseline.

If 3.2 or 3.3 is empty, the builder is being optimistic — ask before shipping.

### Section 4: Capability Requirements

Use this format for each capability:

> **Capability:** [one-sentence description]
> **Constraints learned in POC:** [latency, throughput, freshness, accuracy threshold — quantifiable]
> **POC implementation (for context only):** [specific tool, short phrase]
> **Production options EA should consider:** [2–4 categories, not specific products]

Capabilities likely in a Predictive ML POC. Only include the ones the POC exercises or that production clearly requires:

- Source data access (where the features come from — data warehouse, transactional database, event stream)
- Feature pipeline (computing features from source data, both at training time and at prediction time — usually two pipelines that must produce identical outputs)
- Feature store, if features are shared across models or need to be served at low latency
- Model training pipeline (versioned, reproducible — the notebook is not this)
- Model registry (storing trained models with metadata, versions, lineage to training data)
- Model serving infrastructure (real-time API, batch scoring, or streaming — depends on the use case)
- Prediction storage (where predictions go, for later analysis and monitoring)
- Business system integration (how predictions reach the user or downstream system — CRM, dashboard, workflow tool, automated decision)
- Monitoring for data drift (input feature distributions changing over time)
- Monitoring for model performance drift (accuracy degrading over time, requires labeled outcomes)
- Retraining workflow (how often, on what data, with what evaluation gates)
- Experimentation infrastructure (A/B testing or shadow mode for comparing model versions)
- Fairness and bias evaluation, especially if the prediction affects people (hiring, lending, insurance, healthcare)
- Explainability, if regulated or if business users need to understand individual predictions

### Section 5: Where POC Technology Is Non-Negotiable

For each non-negotiable item:

> **Component:** [specific product]
> **Why it is non-negotiable:** [specific capability that alternatives lack]
> **What changes if EA substitutes:** [what value is lost or risk added]

Common reasons something might be genuinely non-negotiable for Predictive ML:

- A specific model architecture where performance on the task is materially better than alternatives, backed by side-by-side eval results
- A pretrained model where retraining from scratch on the customer's data is impractical
- A feature engineering approach that depends on a specific tool's capability (rare)

If nothing is genuinely non-negotiable, write: "Nothing in this POC's technology stack is non-negotiable. EA has full latitude on platform selection within the capability constraints in Section 4." This is common — most predictive models can be retrained on whatever platform the customer prefers.

### Section 6: What EA Needs to Decide

Ownership tags: **EA decides** / **Product/Business decides** / **Joint decision**

Decisions almost always present for a Predictive ML POC:

- ML platform selection — *EA decides*, informed by the customer's existing data and analytics estate
- Real-time vs. batch scoring — *Joint* (business defines the decision cadence, EA decides what infrastructure supports it)
- Feature pipeline architecture (offline-only, online-only, or unified feature store) — *EA decides*
- Source data access and refresh cadence — *EA decides*, informed by data ownership
- Where predictions are stored and for how long — *EA decides*, informed by legal
- Business system integration approach — *Joint* (business owns the user experience, EA builds the integration)
- Retraining cadence and triggers — *Joint*
- Acceptable model performance and degradation thresholds — *Product/Business decides*
- Fairness evaluation requirements — *Joint*, often with legal involvement
- Explainability requirements — *Joint*, often with compliance involvement
- A/B test or shadow mode plan before full rollout — *Joint*
- Monitoring and alerting ownership — *EA decides*

### Section 7: Known Open Problems

Forward-looking work items. Predictive ML-specific:

- Feature availability at prediction time in the production system
- Production-grade feature pipeline (vs. the training-time pipeline in the notebook)
- Model serving infrastructure
- Monitoring for data drift and model performance drift
- Retraining workflow and decision criteria
- Business system integration for predictions
- A/B test or shadow mode before full production rollout
- Fairness evaluation, if applicable
- Out-of-time validation, if the POC didn't include one

### Section 8: POC Artifacts

- Link to the training notebook
- Link to the dataset used for training and evaluation
- Train/test split methodology statement
- Accuracy or metric results with sample sizes
- Feature list with definitions and source data references
- Link to workshop session notes or demo recording
- Names and contact info for the builder, the SE, and the executive sponsor

---

## Self-check (run before presenting to the builder)

1. **Vendor neutrality.** Specific product names outside Sections 5 and 8 are rewritten or flagged.

2. **No invented facts.** Especially: no invented metrics, no metrics reported without sample size and evaluation methodology.

3. **Train/test split methodology stated.** Any metric in the document is accompanied by a note on how train and test were separated. If random split was used on data with a time dimension, flag this for the builder.

4. **Feature availability at prediction time addressed.** The handoff explicitly answers whether features used for training are available in the production prediction context. If not addressed in the project, ask the builder — this is the single most common showstopper.

5. **Business integration addressed.** The handoff explicitly answers what decision the prediction informs and how it reaches the decision-maker. A model with no integration story is a model with no value.

6. **Honesty sections populated.** Section 3.2 and 3.3 each have at least two items.

7. **Decision ownership tagged.** Every Section 6 item carries one of the three tags exactly.

8. **Open problems are forward-looking.** Section 7 items describe work to do, not findings.

9. **Capability requirements match what the POC did and what production clearly requires.**

10. **Executive summary stands alone.**

When self-check passes, present draft to builder with remaining `[NEEDS INPUT FROM BUILDER]` items at the top.

## Output format

Default to markdown in conversation. Offer .docx export when the builder is satisfied.
