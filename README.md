# NeoMundi Metric Contract

[English](README.md) · [**Français**](README_FR.md)

### Measured signals. Clear meaning. Decisions remain yours.

**A measurement is only useful when you know what it means.** The Metric Contract provides a shared language for reading, comparing and using NeoMundi signals across systems, while keeping measurement and decision-making distinct.

**One measurement primitive. Multiple applications. Multiple infrastructures.**

**[Understand the measurements](READING_MEASUREMENTS.md)** · **[Interoperability](https://github.com/neomundi-io/neomundi-measurement-interoperability)**

---

**Version:** 0.0  
**Status:** Draft  
**Maintainer:** NeoMundi  
**Scope:** Runtime measurement of observable AI system behavior

[English measurement contract](./metric-contract-v0.0.en.md)

[**Understand the measurements →**](READING_MEASUREMENTS.md)

**Evaluators — interpretation and limits:** [English guide](EVALUATORS.md). An explanatory guide, not a calibration certification or a production inventory.

## Understand NeoMundi measurements — and use them without losing their meaning

The NeoMundi Metric Contract defines the semantic boundaries of measurements and signals exposed by the NeoMundi Runtime Measurement Layer.

It provides a common language to:

- interpret measurement signals consistently;
- distinguish measured, partial, unknown and not-evaluated values;
- preserve their meaning across versions and infrastructures;
- consume measurements in audit, observability, governance, insurance and decision-support systems;
- keep measurement, policy, authorization and execution separate.

> **NeoMundi defines measurement semantics. Consuming systems retain authority over interpretation, policy and decisions.**

### Get started

| Need | Reference |
| --- | --- |
| Understand the measurement framework | [Measurement Reference Framework](./measurement_reference_framework_en.md) |
| Apply interpretation and consumption rules | [Signal interpretation and consumption rules](./signal_interpretation_and_consumption_rules.en.md) |
| Read the full Metric Contract | [English contract](./metric-contract-v0.0.en.md) |
| Produce NeoMundi measurements | [Runtime Measurement Layer](https://github.com/neomundi-io/neomundi-runtime-measurement) |
| Exchange measurements between systems | [Measurement Interoperability](https://github.com/neomundi-io/neomundi-measurement-interoperability) |
| Create an account and API key | [NeoMundi platform](https://controltower.neomundi.io/welcome) |

---

## Purpose

The **NeoMundi Metric Contract** defines the semantic boundaries of measurements and signals exposed by the NeoMundi Runtime Measurement Layer.

This **Draft v0.0** is grounded in the observation payload structure currently exposed by the NeoMundi API:

```json
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

The Metric Contract explains the **meaning, interpretation and limits** of the measurements and signals in this payload.

It does **not define policy decisions, execution authorization or operational actions** that an external system might derive from these measurements.

> **NeoMundi measures. The consuming system retains authority over decisions and actions.**

## Metric Contract v0.0

→ [Read the full English contract](./metric-contract-v0.0.en.md)

## What the contract defines

The contract currently defines the semantic interpretation of:

- the NeoMundi observation object;
- payload versioning;
- source context;
- measurement status and coverage;
- `stability_score`;
- `coherence_score`;
- `factual_validity_signal`;
- `semantic_variability_signal`;
- latency and cost bands;
- `risk_signal`;
- known limitations;
- the measurement boundary;
- traceability identifiers;
- unknown, null or not-evaluated values;
- partial measurements;
- separation of measurement and decision-making;
- infrastructure neutrality;
- the boundary with measurement interoperability.

## Source of truth

The Metric Contract introduces no parallel machine-readable representation.

Its semantic definitions are grounded in the structure actually exposed by the NeoMundi API.

Current reference examples are explicitly marked as synthetic (`synthetic: true`). They document the payload structure and measurement semantics.

They are **not** presented as production observations.

## Metric Contract and Measurement Interoperability

The **Metric Contract** defines what NeoMundi measurements and signals mean.

The **Measurement Interoperability** contract defines how measurement records are structured, versioned, exchanged and consumed across independent systems, including their provenance, integrity and responsibility boundaries.

The two contracts address distinct layers:

**measurement semantics → interoperability → external decision or action**

## Draft status

Version **0.0** is an initial semantic specification.

The machine-consumable representation may evolve as the NeoMundi Measurement Interoperability contract is formalized.

Any change that materially alters the meaning of a measurement or signal must remain explicitly versioned.

---

## Repository structure

```text
neomundi-metric-contract/
├── README.md
├── README_FR.md
├── measurement_reference_framework_en.md
├── signal_interpretation_and_consumption_rules.en.md
├── signal_interpretation_and_consumption_rules.fr.md
├── metric-contract-v0.0.en.md
└── metric-contract-v0.0.fr.md
```

Additional machine-readable schemas and interoperability artifacts may be introduced as the specification matures.

---

**NeoMundi Metric Contract — Draft v0.0**

*Measured by NeoMundi. Used under the authority of the consuming system.*
