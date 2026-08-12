[🇫🇷 Version française](./metric-contract-v0.0.fr.md) · [← Repository](./README.md)

# NeoMundi Metric Contract

**Version:** 0.0
**Status:** Draft
**Date:** August 12, 2026
**Maintainer:** NeoMundi
**Scope:** Runtime measurement of AI systems

---

## 1. Purpose

The NeoMundi Metric Contract defines the meaning, conditions, limitations and expected output of a NeoMundi runtime measurement.

Its purpose is to ensure that a measurement can be:

* identified;
* interpreted consistently;
* reproduced under declared conditions;
* transported across different infrastructures;
* compared over time;
* referenced independently;
* consumed by external governance, supervision, compliance or evidence systems.

The Metric Contract defines a **measurement**.

It does not define the policy decision or operational action that may result from that measurement.

> **NeoMundi measures. External systems decide what to do with the measurement.**

---

## 2. Metric identity

Each metric MUST expose a stable identity.

```json
{
  "metric_contract": {
    "id": "neomundi.runtime.stability",
    "version": "0.0",
    "name": "Runtime Stability",
    "measurement_layer": "runtime",
    "publisher": "NeoMundi"
  }
}
```

The combination:

`metric id + metric version`

uniquely identifies the measurement definition applied to an observation.

Any modification affecting the meaning or calculation of the metric MUST result in a new version.

---

## 3. Object of measurement

The metric measures an observable property of the behaviour of an AI system under declared runtime conditions.

Example:

```json
{
  "object": {
    "entity_type": "ai_system",
    "property": "behavioural_stability",
    "observation_mode": "repeated_runtime_execution"
  }
}
```

The metric does not claim to measure the internal state, weights, architecture or intentions of the AI system unless explicitly stated.

It measures **observable behaviour under runtime conditions**.

---

## 4. Measurement conditions

Every measurement MUST be associated with sufficient context to understand the conditions under which it was produced.

Minimum context:

```json
{
  "conditions": {
    "provider": "declared-provider",
    "model": "declared-model",
    "endpoint": "declared-or-hashed-endpoint",
    "timestamp": "ISO-8601",
    "measurement_window": {},
    "protocol_version": "declared-version",
    "sample_size": 0
  }
}
```

Additional parameters MAY include:

* temperature;
* seed;
* system prompt;
* input identifier;
* number of repetitions;
* execution geographic region;
* API configuration;
* tool availability;
* execution environment.

Unknown parameters MUST NOT be silently inferred.

They SHOULD be explicitly represented as unknown, unavailable or undeclared.

---

## 5. Measurement protocol

The contract MUST identify the protocol used to produce the measurement.

Example:

```json
{
  "protocol": {
    "id": "neomundi.protocol.runtime-repeat",
    "version": "0.0",
    "method": "repeated_execution",
    "input_control": "fixed",
    "comparison_scope": "within_observation_window"
  }
}
```

The protocol specification MAY exist as a separate versioned document.

The Metric Contract MUST reference the version of the protocol used.

---

## 6. Measurement value

A NeoMundi measurement MUST expose a machine-readable value.

Example:

```json
{
  "measurement": {
    "metric_id": "neomundi.runtime.stability",
    "metric_version": "0.0",
    "value": 0.92,
    "scale": {
      "minimum": 0.0,
      "maximum": 1.0
    },
    "unit": "dimensionless"
  }
}
```

The value alone is not sufficient.

Its interpretation depends on the metric definition, the protocol, its version and the measurement conditions.

---

## 7. Measurement semantics

Each metric MUST explicitly define what an increase or decrease in its value means.

Example for runtime stability:

```json
{
  "semantics": {
    "higher_value": "greater observed behavioural stability",
    "lower_value": "greater observed behavioural variation",
    "truth_claim": false,
    "quality_claim": false,
    "safety_claim": false
  }
}
```

For this metric:

**stability does not imply truth.**

A highly stable system may consistently repeat an incorrect answer.

A variable system may produce correct answers.

The metric MUST therefore NOT be interpreted outside its declared measurement domain.

---

## 8. Metrological states

NeoMundi MAY associate measurement values with versioned metrological states.

Example:

```json
{
  "state": {
    "code": "VARIATION",
    "taxonomy_version": "0.0"
  }
}
```

Depending on the metric, possible states may include:

* `NORMAL`
* `VARIATION`
* `FACTUAL_ALERT`
* `INCOMPLETE`
* `UNMEASURABLE`

These states describe an **observed metrological condition**.

They are not authorization decisions.

They MUST NOT be interpreted directly as:

* `ALLOW`
* `BLOCK`
* `ACCEPT`
* `REJECT`
* `COMPLIANT`
* `NON-COMPLIANT`

These decisions belong to the consuming system or designated authority.

---

## 9. Threshold boundary

NeoMundi MAY use internally defined and versioned thresholds to identify specific metrological moments.

When exposed, the contract MUST identify the threshold version.

```json
{
  "threshold_reference": {
    "version": "neomundi.thresholds.0.0",
    "state": "VARIATION"
  }
}
```

Thresholds characterize measurement states.

They are not business rules, policy decisions or execution authorizations.

An external system MAY independently decide to:

* continue;
* log;
* request verification;
* reroute;
* escalate;
* regenerate;
* request human supervision;
* interrupt execution.

These actions remain outside the Metric Contract.

---

## 10. Measurement coverage

Each observation SHOULD expose the effective measurement coverage.

```json
{
  "coverage": {
    "requested_observations": 100,
    "valid_observations": 100,
    "coverage_ratio": 1.0,
    "measurement_status": "complete"
  }
}
```

A measurement MUST NOT silently present incomplete coverage as a complete measurement.

---

## 11. Metric limitations

Each metric MUST explicitly declare its measurement scope and limitations.

Example:

```json
{
  "limitations": {
    "measures_truth": false,
    "measures_intent": false,
    "measures_internal_model_state": false,
    "constitutes_policy_decision": false,
    "constitutes_execution_authorization": false
  }
}
```

Additional metric-specific limitations MAY be declared.

---

## 12. Traceability

Each produced measurement SHOULD be linkable to a unique observation event.

Minimum identifiers:

```json
{
  "traceability": {
    "observation_id": "uuid",
    "trace_id": "trace-identifier",
    "timestamp": "ISO-8601",
    "metric_contract_version": "0.0",
    "protocol_version": "0.0"
  }
}
```

Implementations MAY also provide:

* payload hash;
* input hash;
* output hash;
* execution receipts;
* signed evidence artifacts;
* cryptographic integrity mechanisms.

These mechanisms belong to the traceability and evidence layers and do not alter the meaning of the underlying metric.

---

## 13. Interoperability boundary

The Metric Contract defines the semantics of the measurement.

A distinct **NeoMundi Runtime Interoperability Contract** defines how observations and measurements are exchanged between systems.

The expected architecture is therefore:

**AI execution**
**→ NeoMundi observation**
**→ NeoMundi measurement**
**→ interoperable runtime event**
**→ external governance / monitoring / compliance / evidence system**
**→ external decision or action**

The receiving infrastructure does not need to reproduce NeoMundi's internal measurement engine in order to consume the measurement.

---

## 14. Infrastructure neutrality

A NeoMundi metric SHOULD remain independent of the infrastructure consuming it.

The same runtime measurement MAY therefore support multiple uses, including:

* continuous control;
* AI monitoring;
* runtime governance;
* risk supervision;
* compliance evidence;
* SLA monitoring;
* model routing;
* audit;
* incident reconstruction;
* research.

The Metric Contract MUST NOT impose any of these uses as the unique interpretation of the measurement.

---

## 15. Versioning rule

A new metric version MUST be created when a modification materially affects:

* the measured property;
* the calculation method;
* the scale;
* the semantic interpretation;
* the measurement protocol;
* applicable thresholds;
* the measurement scope.

Historical observations MUST retain the metric version under which they were produced.

---

## 16. Minimum machine-readable representation

```json
{
  "metric_contract": {
    "id": "neomundi.runtime.stability",
    "version": "0.0"
  },
  "object": {
    "entity_type": "ai_system",
    "property": "behavioural_stability"
  },
  "protocol": {
    "id": "neomundi.protocol.runtime-repeat",
    "version": "0.0"
  },
  "measurement": {
    "value": 0.92,
    "scale": {
      "minimum": 0.0,
      "maximum": 1.0
    },
    "unit": "dimensionless"
  },
  "state": {
    "code": "NORMAL",
    "taxonomy_version": "0.0"
  },
  "coverage": {
    "coverage_ratio": 1.0
  },
  "traceability": {
    "observation_id": "uuid",
    "timestamp": "ISO-8601"
  },
  "limitations": {
    "measures_truth": false,
    "constitutes_policy_decision": false,
    "constitutes_execution_authorization": false
  }
}
```

---

## 17. Fundamental principle

**A NeoMundi metric is a versioned statement about an observed property of an AI system under declared runtime conditions.**

It provides a common measurable fact that can be consumed by different systems and infrastructures without transferring decision authority to NeoMundi.

**Measured by NeoMundi.**
**Used according to the authority of the consuming system.**

---

**NeoMundi Metric Contract — Draft v0.0**
