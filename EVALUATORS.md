# Understanding NeoMundi evaluators

[English](EVALUATORS.md) · [Français](EVALUATORS.fr.md)

**Status:** explanatory guide, 13 November 2026. **Not** a validated inventory of evaluators currently running in production, a new measurement schema, or a claim of scientific calibration.

## What an evaluator does

An evaluator contributes an observed signal under specific execution conditions. The **signal**, the **evaluator used to produce it**, and any **aggregate** derived from signals are different objects. A numerical output is not automatically a probability, a ground-truth verdict or a calibrated confidence interval.

The Metric Contract defines signal meaning; the measurement service emits values and execution metadata; [Metrology Validation](https://github.com/neomundi-io/neomundi-metrology-validation) documents experiments. For implementation-specific field semantics, consult the [API interpretation table](https://github.com/neomundi-io/neomundi-runtime-measurement/blob/main/docs/MEASUREMENT_INTERPRETATION_TABLE.md) and the versioned response actually received.

## Reading common signal families

The following are **descriptive categories**, not a statement that each evaluator always runs, nor a universal mapping across API versions.

| Signal family | What it can indicate | What it does not establish |
|---|---|---|
| Coherence / semantic relevance | The degree of relationship between the question and response under a specified comparison method (historically, embedding similarity) | Factual correctness, usefulness for every task, or human-understood agreement |
| Factual-risk / hallucination assessment | A judge's estimated concern about factual claims in the supplied response; historical implementations use an LLM-as-judge | Independent verification of truth without an appropriate reference source and evaluation protocol |
| Semantic risk / moderation | Whether a configured classifier reports a risk category or condition on evaluated content | A comprehensive safety guarantee, a well-calibrated probability, or coverage of content not assessed |
| Ambiguity / semantic instability | An evaluator's characterization of ambiguous or unstable interpretation under its declared protocol | Measured variation across repeated executions unless repetitions and their protocol actually occurred |
| Stability / aggregates | A derived reading combining declared inputs under a specified version and configuration | A standalone physical quantity, independent constituent measurements, or an invariant score across versions |

Do not conflate `G`, `g_score`, `g_final` and `stability_score`. Their mapping and arithmetic require a version-specific source, and some older public calculations explicitly concern historical implementations.

## Required evidence to interpret a reading

1. **Observable:** the exact input, response and scope the evaluator assessed.
2. **Measurement status:** `measured` vs `not_measured`, `degraded`, incomplete or undeclared as supported by the applicable contract. A missing measurement must not be turned into zero or a favourable default.
3. **Coverage:** which required signals were attempted, succeeded or were unavailable; a decision such as `ALLOW` is not necessarily evidence that checks ran.
4. **Identity and configuration:** measurement version **plus** the actual evaluator/model served and the available configuration fingerprints. A model configured or requested is not necessarily the model that responded. Do not invent missing fingerprints.
5. **Validity:** date and scope of any calibration, reference corpus, comparator, false-positive/false-negative analysis, repeatability measurement and independent replication. If not available, state **not established**.
6. **Comparability:** compare values only where their relevant protocols, versions, coverage, evaluator identity and conditions are sufficiently aligned. Identical field names or measurement versions alone do not establish equivalence.

## Examples of correct interpretation

- `coherence_score` present: *a relevance-related signal was returned under this method* — **not** “the answer is true.”
- Factual-risk score low: *this evaluator detected little factual concern under these conditions* — **not** “there is no hallucination.”
- `status: not_measured`, `value: null`: *no numeric measurement is available* — **not** zero and **not** success.
- A technical smoke test passes: *the tested pipeline worked on those cases* — **not** a general validation or certification.

## Calibration and confidentiality

Calibration and validation are **per signal, version, domain and experiment**. Do not describe the entire suite as calibrated based on a single small experiment. Refer to the [experimental qualification programme](https://github.com/neomundi-io/neomundi-metrology-validation) and its [disclosure policy](https://github.com/neomundi-io/neomundi-metrology-validation/blob/main/PUBLIC_DISCLOSURE_POLICY.md).

Publishing the operational meaning, limitations and evidence does not require releasing private prompts, live routing, credentials or sensitive datasets. Conversely, confidentiality must not conceal scientific limitations or imply reproducibility that the disclosed evidence cannot support. Historical frozen experimental artefacts remain historical, not a current production configuration.

## Related references

- [Understand the measurements](READING_MEASUREMENTS.md)
- [Metric Contract v0.0 — Draft](metric-contract-v0.0.en.md)
- [API interpretation table](https://github.com/neomundi-io/neomundi-runtime-measurement/blob/main/docs/MEASUREMENT_INTERPRETATION_TABLE.md)
- [Metrology Validation](https://github.com/neomundi-io/neomundi-metrology-validation)
- [SDK historical limitations](https://github.com/neomundi-io/neomundi-python-sdk/blob/main/docs/LIMITES.md)

**Boundary:** measurements support technical interpretation and human oversight; they do not certify truth, safety, lawfulness or compliance and do not grant execution authority.
