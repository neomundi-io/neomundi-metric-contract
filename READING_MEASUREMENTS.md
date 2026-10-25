# Understand the measurements
[English](READING_MEASUREMENTS.md) · [Français](READING_MEASUREMENTS.fr.md)

## Read a result
Read the value together with its state, coverage, versions and execution conditions.
Null is not zero. Matching field names do not establish matching computations.

## What does G mean?
Earlier documents describe G as a signal related to runtime stability. Those
documents alone are not a current computation specification.

Keep `G`, `g_score`, `g_final` and `stability_score` distinct.
Their mapping, aggregated inputs and normalization must be documented for a
specific engine version. Do not infer a general formula from a name or a legacy note.

The [API interpretation table](https://github.com/neomundi-io/neomundi-runtime-measurement/blob/main/docs/MEASUREMENT_INTERPRETATION_TABLE.md)
distinguishes normative, experimental and illustrative vocabulary.
This guide does not promote those descriptions into scientific validation.

## What a calculation sheet needs
| Question | Required information |
|---|---|
| What is measured? | Object, unit, range and direction |
| What goes in? | Elementary inputs and whether they are local, declared or evaluated |
| What is aggregated? | Included/excluded components, dependencies and normalization |
| What happens when an input is missing? | Availability, partial computation, refusal and null propagation |
| Which formula applies? | Versioned expression, coefficients, bounds and configuration |
| How can it be checked? | Recalculable reference examples and expected outputs |
| What are the limits? | Protocol, tested domain, uncertainty and unestablished claims |

Meaning and aggregation need to be understandable. Publication of exact formulas
must be reconciled with the deployed code, its version and publication rights.
This page does not claim that a complete reproducible formula is already public.

## Read the current documents
- [Semantic contract EN](metric-contract-v0.0.en.md) · [FR](metric-contract-v0.0.fr.md) — Draft v0.0.
- [Reference framework EN](measurement_reference_framework_en.md) · [FR](measurement_reference_framework_fr.md).
- [Interpretation and consumption EN](signal_interpretation_and_consumption_rules.en.md) · [FR](signal_interpretation_and_consumption_rules.fr.md).
- [Qualification and methods](https://github.com/neomundi-io/neomundi-metrology-validation).
- [Earlier telemetry documents](https://github.com/neomundi-io/runtime-telemetry-signals) — historical references, not a current production specification.
- [API](https://github.com/neomundi-io/neomundi-runtime-measurement) · [Station](https://github.com/neomundi-io/neomundi-station-public).

A measurement grants no execution permission. Interpretation and decisions remain with the consumer.
