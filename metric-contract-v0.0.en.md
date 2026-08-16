[🇫🇷 Version française](./metric-contract-v0.0.fr.md) · [← Repository](./README.md)

> **Draft notice — v0.0**
>
> This document defines the current semantic boundaries of the NeoMundi measurement layer.
>
> The JSON structures, field names, identifiers, enums and examples presented in this version are operational or illustrative references. They do not yet constitute the definitive schema of the **NeoMundi Runtime Interoperability Contract**.
>
> The machine-readable representation may evolve as interoperability is formalized. Any change that materially alters the meaning of a measurement or signal must, however, be explicitly versioned.

# NeoMundi Metric Contract

**Version:** 0.0
**Status:** Draft
**Date:** August 16, 2026
**Maintainer:** NeoMundi
**Scope:** Runtime measurement of observable AI-system behaviour

---

## 0. Normative language

In this document:

* **MUST / MUST NOT** denotes a binding semantic requirement;
* **SHOULD / SHOULD NOT** denotes a recommended requirement from which an implementation may depart with justification;
* **MAY** denotes permitted but non-mandatory behaviour.

The normative scope of this contract is limited to the **meaning, interpretation and semantic boundaries** of the NeoMundi measurement layer.

Exact field names, JSON locations, enums, serialization, transport and version negotiation do not become normative merely because they appear in current representations.

These elements primarily belong to the **Runtime Interoperability Contract**.

---

## 1. Purpose

The **NeoMundi Metric Contract** defines the meaning, interpretation conditions, limitations and boundaries of measurements and signals produced by the NeoMundi runtime measurement layer.

It defines **what a NeoMundi measurement means**.

It does not define the policy, operational decision or execution authorization that an external system may derive from that measurement.

> **The Metric Contract MUST NOT define a policy decision, execution authorization or operational action as the intrinsic meaning of a NeoMundi measurement.**

**NeoMundi measures. The consuming system retains authority over decisions and actions.**

---

## 2. Current runtime reference

Draft v0.0 is anchored in the structure of the observation payload currently exposed by the NeoMundi API.

Example structure:

```json id="rf62sa"
{
  "schema_version": "neomundi_observation_payload_v0.1",
  "observation_id": "nm-syn-001",
  "generated_at": "2026-06-28T06:42:47Z",
  "synthetic": true,
  "source": {},
  "measurement": {},
  "known_limitations": [],
  "measurement_boundary": []
}
```

This structure constitutes the contract's **current operational anchor**.

It does not constitute a complete normative JSON Schema.

The Metric Contract **MUST NOT** introduce a competing machine representation that alters the meaning of measurements actually exposed by NeoMundi.

Exact structure, serialization and transport requirements are deferred to the **Runtime Interoperability Contract**.

---

## 3. Fundamental concepts

The Metric Contract distinguishes several related but non-interchangeable concepts.

### 3.1 Observation

An **observation** is a runtime event or set of runtime executions under declared conditions from which one or more measurements may be produced.

A NeoMundi measurement **MUST remain attributable to the observation to which it belongs**.

A synthetic observation **MUST NOT be represented as a production observation**.

### 3.2 Metric

A **metric** defines an observable property measured by NeoMundi together with the semantics required to interpret its values or signals.

### 3.3 Measurement

A **measurement** is a result obtained from an observation according to a declared method or protocol.

The measurement represents a specific result.

It does not, by itself, constitute an external decision.

### 3.4 Metrological state

A measurement **MAY** be associated with a metrological state or classification according to a declared and versioned taxonomy or reference.

A metrological state describes an observed condition.

It does not automatically constitute a policy decision or execution authorization.

### 3.5 Conceptual relationship

The general relationship is:

**Observation → measurement or signal → optional metrological interpretation → external consumption**

The resulting decision or action remains outside the Metric Contract.

---

## 4. Source context

A measurement does not exist independently of its observation context.

Declared information about the source, model, provider, environment, configuration or protocol may contribute to its interpretation.

Information that is not explicitly present or declared in the observation context **MUST NOT be silently inferred as measured or known**.

In particular, a model alias alone does not constitute proof of:

* provider identity;
* the exact model version;
* the execution configuration;
* a particular runtime environment.

Unknown, unavailable or undeclared elements must remain distinguishable from elements that were actually observed or declared.

---

## 5. Measurement and interpretation context

A NeoMundi value or signal must not be interpreted in isolation when its meaning materially depends on:

* observation conditions;
* coverage;
* measurement status;
* known limitations;
* the applicable measurement boundary.

> A measurement or signal **MUST NOT be interpreted independently of applicable limitations and boundaries when those materially affect its meaning**.

The presence of a numerical value does not, by itself, imply that all measurement dimensions were assessed.

---

## 6. Measurement status and coverage

An observation may be complete, partial, not assessed or otherwise limited.

A partial observation **MUST NOT be represented as complete**.

The presence of a value within a partial observation **MUST NOT imply that all measurement dimensions were assessed**.

Known incomplete coverage **MUST remain distinguishable from complete coverage** during interpretation.

The exact field name, format and encoding belong to the Runtime Interoperability Contract.

---

## 7. `stability_score`

`stability_score` describes a level of observed behavioural stability under the declared measurement conditions.

It does not intrinsically measure factual truth.

> **`stability_score` MUST NOT be interpreted as a measure of factual truth.**

`stability_score` **MUST NOT**, by itself, establish:

* safety;
* compliance;
* admissibility;
* execution authorization;
* the overall quality of a system.

High stability **MAY** coexist with a factually incorrect output.

Behavioural variation may also coexist with factually correct outputs.

### Principle

> **Stability is not truth.**

---

## 8. `coherence_score`

`coherence_score` describes an observed coherence property according to the applicable definition and measurement conditions.

`coherence_score` **MUST NOT**, by itself, be interpreted as:

* factual validation;
* safety validation;
* a compliance determination;
* execution authorization.

It may be consumed together with other measurements or signals.

The Metric Contract does not prescribe the policy an external system should apply based on this information.

---

## 9. `factual_validity_signal`

A runtime factual-validity signal represents information produced within the limits of its associated measurement method.

A signal based only on runtime-available evidence **MUST NOT be presented as independent verification of truth when no independent validation has been performed**.

A `null`, `unknown` or `not_assessed` value **MUST NOT be interpreted as**:

* factual validation;
* absence of error;
* zero factual risk.

Taxonomies, states and enums observed in current representations remain descriptive until they are explicitly stabilized and versioned.

---

## 10. `semantic_variability_signal`

`semantic_variability_signal` describes semantic variability observed under the applicable conditions.

Low semantic variability **MUST NOT be interpreted as evidence of factual correctness**.

Elevated variability **MUST NOT**, by itself:

* identify the cause of the variation;
* identify which output is correct;
* constitute proof of error.

It is a measurement signal, not a policy decision.

---

## 11. Latency and cost

NeoMundi may expose measurements, signals or bands relating to latency and cost.

In Draft v0.0, the Metric Contract does not freeze:

* definitive enums;
* exact thresholds;
* band boundaries;
* operational or routing consequences.

An explicitly unknown latency or cost value **MUST NOT be silently interpreted as a measured value**.

Structural definitions and any future thresholds must be specified and versioned separately.

---

## 12. `risk_signal`

A NeoMundi `risk_signal` constitutes a measurement or interpretation signal within the declared limits of the metric.

It **MUST NOT automatically mean**:

* `ALLOW`;
* `BLOCK`;
* `ACCEPT`;
* `REJECT`;
* `COMPLIANT`;
* `NON_COMPLIANT`;
* `SAFE`;
* `UNSAFE`;
* `ADMISSIBLE`;
* `NON_ADMISSIBLE`.

A consuming system **MAY** use this signal as an input to its own policy.

The resulting decision remains outside the Metric Contract.

Risk levels, types and enums currently observed do not automatically become normative categories in Draft v0.0.

---

## 13. Known limitations

A NeoMundi measurement may include limitations relating to:

* the method;
* available data;
* the protocol;
* coverage;
* context;
* the measurement domain;
* possible interpretation.

Any limitation that materially affects interpretation of a measurement **MUST remain semantically associated with that measurement when it is interpreted or transmitted to support a decision**.

The exact format, location and serialization of those limitations belong to interoperability.

---

## 14. Measurement boundary

A NeoMundi measurement describes an observable property within a declared scope.

It does not automatically constitute a general conclusion about the observed system.

> **A NeoMundi measurement MUST NOT, by itself, be interpreted as establishing truth, safety, authority, downstream permission or execution admissibility, unless a future explicitly versioned metric defines otherwise.**

This boundary is one of the fundamental normative principles of the Metric Contract.

---

## 15. Separation between measurement and decision

NeoMundi is responsible for the declared semantics of its measurements.

The consuming system remains responsible for the policy or operational action it derives from those measurements.

External infrastructures **MAY**, for example, use a NeoMundi measurement to:

* log;
* verify;
* regenerate;
* reroute;
* escalate;
* request human supervision;
* interrupt a workflow;
* support a compliance process;
* produce or enrich an evidence artifact.

This list is illustrative and non-exhaustive.

None of these actions constitutes the intrinsic meaning of the measurement.

---

## 16. Infrastructure neutrality

The Metric Contract **MUST NOT impose one downstream use as the unique interpretation of a measurement**.

The same measurement **MAY** be consumed:

* by multiple infrastructures;
* within multiple architectures;
* for multiple purposes.

Infrastructure-specific policy logic remains outside the Metric Contract.

This separation allows NeoMundi to provide a common measurement layer without imposing the decision architecture that consumes it.

---

## 17. Unknown, null and non-assessed values

The following states are not semantically equivalent:

* measured value;
* unavailable value;
* `null`;
* `unknown`;
* `not_assessed`.

`null` **MUST NOT be interpreted as zero**.

`unknown` **MUST NOT be interpreted as a measured, low or high category**.

`not_assessed` **MUST NOT be interpreted as absence of risk or successful validation**.

The distinction between measured, unknown, unavailable and non-assessed values **MUST be preserved semantically**.

Exact encoding rules belong to interoperability.

---

## 18. Partial measurements

An observation or measurement may be partial.

A partial observation **MUST NOT be represented as complete**.

A value available within a partial observation **MUST NOT imply that all measurement dimensions were assessed**.

Consuming systems **SHOULD** consider declared coverage when interpreting partial measurements.

---

## 19. Traceability

The current representation may include identifiers such as:

* `observation_id`;
* `source_batch_id`;
* `trace_id`;
* a timestamp;
* version references.

A measurement **SHOULD remain attributable to its originating observation**.

Draft v0.0 does not yet freeze the mandatory traceability structure.

Requirements concerning:

* hashes;
* signatures;
* receipts;
* mandatory identifiers;
* cryptographic integrity;
* evidence artifacts;

belong to the Runtime Interoperability Contract or dedicated specifications.

---

## 20. Confidentiality and non-exposed data

The Metric Contract describes the data and measurements exposed by the measurement interface.

It does not imply access to the internal computation data of an AI system.

Current synthetic examples may not expose, among other things:

* raw prompts;
* raw model outputs;
* certain provider information;
* customer data;
* proprietary structures.

This observation does not constitute a universal normative prohibition in Draft v0.0.

Confidentiality, security and data-exposure requirements must be defined in the appropriate specifications.

---

## 21. Boundary with the Runtime Interoperability Contract

The **Metric Contract** defines the semantics of measurements and signals.

The **Runtime Interoperability Contract** defines, or will define, how the corresponding objects are exchanged and consumed between systems.

It may notably specify:

* machine-readable structures;
* required fields;
* types;
* enums;
* serialization;
* compatibility;
* version negotiation;
* integration constraints.

The Metric Contract **MUST NOT freeze transport or interoperability details that have not yet been validated**.

The conceptual architecture is therefore:

**AI execution → NeoMundi observation → NeoMundi measurement or signal → interoperability → consuming system → external decision or action**

---

## 22. Semantic versioning

A material change in the meaning of a measurement or signal **MUST be explicitly versioned**.

This includes changes that materially affect:

* the measured property;
* the semantic definition;
* the interpretation domain;
* fundamental limitations;
* the meaning of a value or signal.

Historical observations **SHOULD remain interpretable according to the semantic version under which they were produced**.

Structural or serialization changes that do not alter meaning **MAY** be handled exclusively through schema or interoperability versioning.

---

## 23. Operational reference example

The following example illustrates the current structure around which Draft v0.0 is built:

```json id="cj1qix"
{
  "schema_version": "neomundi_observation_payload_v0.1",
  "observation_id": "nm-syn-001",
  "generated_at": "2026-06-28T06:42:47Z",
  "synthetic": true,
  "source": {},
  "measurement": {},
  "known_limitations": [],
  "measurement_boundary": []
}
```

### Status of this example

**Operational reference example — not a complete normative JSON Schema.**

In particular:

* example values are non-normative;
* example identifiers are non-normative;
* exact field names and locations are not frozen by the Metric Contract;
* observed enums are not automatically normative;
* exact transport requirements remain subject to the Runtime Interoperability Contract.

In this example, `synthetic: true` explicitly indicates that the observation is synthetic.

It must not be presented as a production observation.

---

## 24. Explicitly deferred elements

Draft v0.0 does not yet freeze:

* the definitive JSON Schema;
* the definitive list of required fields;
* the definitive names of all fields;
* definitive enums;
* the transport protocol;
* version negotiation;
* cryptographic receipts;
* signatures and hashes;
* evidence packaging;
* provider disclosure rules;
* exact threshold values;
* exact band boundaries;
* automatic routing rules;
* automatic stop rules;
* compliance determination;
* admissibility determination;
* execution authorization.

These elements may belong to:

* the **Runtime Interoperability Contract**;
* evidence specifications;
* product configuration;
* consuming-system policy;
* future explicitly versioned specifications.

---

## 25. Normative core v0.0

Draft v0.0 deliberately concentrates its normative requirements on a limited number of durable boundaries:

1. A measurement **MUST** remain attributable to its observation.
2. A synthetic observation **MUST NOT** be represented as a production observation.
3. Undeclared information **MUST NOT** be silently inferred as measured or known.
4. A partial measurement **MUST NOT** be represented as complete.
5. Stability **MUST NOT** be interpreted as factual truth.
6. Coherence **MUST NOT**, by itself, be interpreted as factual validation.
7. A runtime factual signal **MUST NOT** be presented as independent verification of truth when it is not based on independent validation.
8. Semantic variability **MUST NOT** be treated as proof of factual correctness or as identification of the correct output.
9. A risk signal **MUST NOT** automatically become a policy or execution decision.
10. Material measurement limitations **MUST** remain semantically associated with interpretation.
11. A NeoMundi measurement **MUST NOT**, by itself, establish truth, safety, authority, downstream permission or execution admissibility.
12. Unknown, null or non-assessed states **MUST NOT** be silently converted into measured values.
13. Any material semantic change **MUST** be explicitly versioned.
14. The Metric Contract **MUST NOT** freeze unvalidated transport or interoperability details.
15. The consuming system retains authority over the decisions and actions it derives from NeoMundi measurements.

This deliberately reduced normative core is intended to make Draft v0.0 more robust than a contract containing numerous structural obligations that remain premature.

---

## 26. Fundamental principle

**NeoMundi provides contextualized measurements and signals about observable AI-system behaviour within declared runtime boundaries.**

These measurements may be consumed by multiple systems, infrastructures and use cases without transferring downstream decision authority to NeoMundi.

> **Measured by NeoMundi.**
> **Used according to the authority of the consuming system.**

---

**NeoMundi Metric Contract — Draft v0.0**
