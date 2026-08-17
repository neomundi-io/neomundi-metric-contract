- 🇬🇧 [NeoMundi Measurement Reference Framework](./measurement_reference_framework_en.md)
- 🇫🇷 [Cadre de référence de la mesure NeoMundi](./measurement_reference_framework_fr.md)

# NeoMundi Measurement Reference Framework

**Version:** 0.1
**Status:** Draft
**Maintainer:** NeoMundi
**Scope:** Reference framework for runtime measurement of observable AI-system behaviour

---

## 1. Purpose

The NeoMundi Measurement Reference Framework defines the principles under which a NeoMundi measurement can be produced, reproduced, transported across infrastructures, interpreted independently, and compared over time.

It complements the NeoMundi Metric Contract.

The **Metric Contract** defines what NeoMundi measurements and signals mean.

The **Measurement Reference Framework** defines the conditions under which those measurements remain meaningful across repeated observations, different AI systems, different technical infrastructures, and different downstream uses.

The framework does not define:

* a policy decision;
* an authorization decision;
* an enforcement action;
* a compliance verdict;
* a safety certification;
* a statement of factual truth.

NeoMundi provides measurement.

The infrastructure consuming that measurement remains responsible for interpretation, policy and action.

---

## 2. Measurement object

The primary measurement object is the **observable behaviour of an AI system at runtime**.

NeoMundi does not assume direct access to:

* model weights;
* internal reasoning;
* training data;
* hidden states;
* proprietary architecture;
* internal provider infrastructure.

The measurement therefore concerns what can be observed from system execution under defined conditions.

Depending on the protocol, observable properties may include:

* response variation;
* consistency;
* recurrence;
* factual signals;
* latency;
* availability;
* behavioural change over time;
* changes between repeated observations;
* changes between versions, providers or execution environments.

A measurement describes an observation of behaviour.

It does not, by itself, explain the internal cause of that behaviour.

---

## 3. Reference principle

A NeoMundi measurement is not defined by the decision it produces.

It is defined by the conditions under which its meaning can remain sufficiently stable across:

* repeated observations;
* different execution times;
* different AI systems;
* different providers;
* different infrastructures;
* different downstream consumers;
* independent interpretations.

The reference framework therefore separates four layers:

**Observation → Measurement → Interpretation → Action**

NeoMundi operates primarily at the observation and measurement layers.

Interpretation and action remain external unless explicitly implemented by another system.

This separation is part of the measurement boundary.

---

## 4. Semantic invariance

A metric should retain the same defined meaning regardless of the infrastructure consuming it.

For example, if a NeoMundi signal represents a measured degree of behavioural consistency, its semantic definition should not change because one infrastructure uses it for:

* monitoring;
* audit;
* human escalation;
* insurance analysis;
* governance;
* cybersecurity;
* model comparison;
* operational control.

Different systems may assign different consequences to the same measurement.

The measurement definition itself should remain unchanged.

This principle can be expressed as:

**same measurement primitive → stable semantic contract → multiple interpretations → multiple actions**

Semantic portability is therefore distinct from decision portability.

NeoMundi does not require downstream systems to reach the same decision from the same measurement.

---

## 5. Reproducibility

A measurement framework must make reproduction possible whenever the underlying systems and execution conditions allow it.

A reproducible NeoMundi observation should therefore preserve, where technically available and relevant:

* protocol identifier;
* protocol version;
* metric definition;
* measurement schema version;
* observation identifier;
* execution timestamp;
* AI provider;
* model identifier;
* model version where available;
* input or prompt reference;
* execution parameters where available;
* repetition count;
* scoring method;
* software or launcher version;
* measurement output;
* known limitations.

Reproducibility does not mean that an AI system must return the same output.

For stochastic or changing systems, non-identical outputs may be precisely what is being measured.

The requirement is that the **measurement procedure** can be reproduced and its conditions inspected.

---

## 6. Repeatability and longitudinal observation

Runtime AI behaviour may vary even when the input appears unchanged.

For this reason, a single execution should not automatically be treated as representative of a system's behavioural state.

NeoMundi measurement protocols may therefore use repeated observations.

Repeated measurement makes it possible to distinguish between:

* isolated variation;
* persistent behaviour;
* recurrent patterns;
* regime changes;
* longitudinal drift;
* temporary anomalies.

The objective is not to force deterministic behaviour.

The objective is to make behavioural variation observable.

Where longitudinal comparison is performed, protocol continuity and version traceability are required so that changes in the measurement process can be distinguished from changes in the observed system.

---

## 7. Cross-infrastructure portability

A measurement layer becomes more useful when its output can be consumed without requiring the downstream infrastructure to adopt the measurement provider's policy model.

NeoMundi therefore seeks to expose measurements through explicit and portable contracts.

A downstream infrastructure may consume the same measurement signal for different purposes.

For example:

```text
AI system
    ↓
NeoMundi observation
    ↓
NeoMundi measurement signal
    ↓
external infrastructure
    ↓
interpretation / policy
    ↓
decision / enforcement / evidence
```

The external infrastructure remains in control of the final layers.

This allows the same measurement primitive to participate in heterogeneous systems without requiring those systems to become NeoMundi systems.

---

## 8. Independent interpretation

Measurement and interpretation must remain distinguishable.

Two independent infrastructures may receive the same NeoMundi measurement and reach different conclusions.

This is not necessarily a measurement failure.

It may reflect:

* different risk tolerances;
* different policies;
* different regulatory environments;
* different operational contexts;
* different decision thresholds;
* different business objectives.

NeoMundi therefore distinguishes:

**what was observed**

from

**what the observation means for a specific actor**

and from

**what that actor decides to do**

This separation prevents a runtime metric from silently becoming a decision authority.

---

## 9. Measurement is not truth

A measurement signal is not equivalent to factual truth.

A system can appear stable while being wrong.

A system can be internally consistent while reproducing the same false information.

Conversely, variation between responses does not necessarily indicate failure.

For this reason, NeoMundi avoids treating any isolated metric as a universal verdict.

Where factual evaluation is included in a protocol, the factual evaluation method must itself be identifiable and subject to its own limitations.

Measurement signals should therefore be interpreted as evidence about observable behaviour, not as self-sufficient claims about truth, safety or compliance.

---

## 10. Multiple independent measurements

AI systems expose multiple behavioural dimensions.

No single runtime metric should automatically be assumed to represent the entire behavioural state of a system.

Where appropriate, NeoMundi may combine or expose multiple independent measurements while preserving their individual meanings.

This allows downstream systems to reason from several signals rather than from a single composite score.

The framework favours:

* explicit signals;
* documented relationships;
* visible uncertainty;
* traceable transformations;

over opaque aggregation.

A composite indicator, when used, should remain traceable to its underlying measurements.

---

## 11. Traceability

A measurement should be connected to sufficient provenance to allow later inspection.

A NeoMundi observation may therefore include or reference:

* when the observation occurred;
* what system was observed;
* under which protocol;
* under which metric definitions;
* with which software version;
* with which inputs;
* with which known limitations;
* and how the resulting signal was produced.

Traceability enables measurements to be reviewed after the execution has occurred.

It also allows later comparisons between observations generated under different conditions.

---

## 12. Versioning

AI providers change.

Models change.

APIs change.

Measurement software changes.

Metrics may also evolve.

Versioning is therefore part of the reference framework.

A measurement should, where possible, distinguish changes in:

* the observed AI system;
* the observation protocol;
* the scoring method;
* the metric definition;
* the output schema;
* the measurement software.

A historical observation should remain interpretable according to the contract and protocol version under which it was produced.

New versions should not silently redefine previous measurements.

---

## 13. Falsifiability and external replication

A measurement framework should permit independent challenge.

Where practical, NeoMundi aims to make available the elements necessary for third parties to:

* reproduce a protocol;
* inspect a measurement definition;
* compare independent observations;
* identify disagreements;
* test alternative interpretations;
* report failures or limitations.

External disagreement is not excluded by the framework.

It is part of the process by which a measurement discipline can be tested and improved.

A measurement claim that cannot, in principle, be questioned or reproduced should not be treated as strong metrological evidence.

---

## 14. Known limitations

Runtime behavioural measurement has inherent limitations.

Among them:

* provider-side systems may change without notice;
* model versions may not always be fully exposed;
* stochastic systems may produce different results under nominally equivalent conditions;
* network conditions may affect latency measurements;
* external evaluators may introduce their own uncertainty;
* prompts and datasets may contain bias or ambiguity;
* observable behaviour cannot reveal every internal property of a system;
* measurements remain dependent on the scope of the protocol used.

These limitations should be documented rather than hidden.

A measurement is meaningful within its declared boundary.

It should not be generalized beyond that boundary without additional evidence.

---

## 15. Evidence accumulation

The strength of a measurement framework does not come only from its specification.

It also comes from repeated use.

Evidence can accumulate through:

* repeated experiments;
* longitudinal observations;
* cross-provider measurements;
* independent replication;
* external integrations;
* heterogeneous downstream infrastructures;
* documented disagreements;
* documented failures;
* protocol revisions.

The fact that different infrastructures can consume the same measurement object without requiring a change to its semantic definition provides evidence of portability.

It does not, by itself, prove universal validity.

---

## 16. Reference architecture

The NeoMundi reference boundary can be represented as:

```text
┌───────────────────────────────┐
│          AI system            │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│      observable execution     │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│     NeoMundi measurement      │
│                               │
│ observation                   │
│ metrics                       │
│ signals                       │
│ provenance                    │
└───────────────┬───────────────┘
                │
        measurement boundary
                │
                ▼
┌───────────────────────────────┐
│   downstream infrastructure   │
│                               │
│ interpretation                │
│ policy                        │
│ authorization                 │
│ decision                      │
│ enforcement                   │
│ reporting                     │
└───────────────────────────────┘
```

The boundary is intentional.

NeoMundi provides the signal.

The consuming infrastructure determines what that signal means in its own context.

---

## 17. Relationship with the Metric Contract

This framework and the NeoMundi Metric Contract serve different purposes.

### Metric Contract

Defines:

* metric semantics;
* signal definitions;
* expected fields;
* interpretation limits;
* known limitations.

### Measurement Reference Framework

Defines:

* the measurement object;
* measurement boundaries;
* reproducibility principles;
* portability principles;
* independence of interpretation;
* traceability requirements;
* versioning principles;
* falsifiability and replication principles.

Together they provide the semantic and methodological foundation of the NeoMundi runtime measurement layer.

---

## 18. Reference statement

The NeoMundi measurement approach can be summarized as follows:

> A NeoMundi measurement is an observation of runtime AI behaviour produced under an explicit protocol and semantic contract. Its meaning should remain traceable across repetitions, time, systems and consuming infrastructures. The measurement informs downstream interpretation and action but does not itself constitute policy, authorization, enforcement, certification or truth.

---

## 19. Status

This document is a **Draft v0.1**.

It is intended to evolve through:

* experimental results;
* external replication;
* interoperability pilots;
* independent review;
* implementation feedback;
* refinement of the NeoMundi Metric Contract.

Changes to this framework should be versioned and documented.

---

**NeoMundi**
*Independent runtime measurement for AI systems.*
