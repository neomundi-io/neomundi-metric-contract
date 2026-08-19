**Document version:** 0.1  
**Status:** Experimental / pre-freeze  
**Standardization status:** Work in progress  
**Version date:** 2026-08-19  

> This version provides a stable reference point for experimental consumer profiles and implementations. It does not represent a final or frozen NeoMundi standard. Material semantic changes require a new document version.

# NeoMundi Metric Contract

**Canonical runtime measurement and consumer contract for NeoMundi signals**

> **Status:** Experimental / pre-freeze contract
> **Scope:** Runtime measurement outputs, signal semantics, consumption boundaries, interoperability, and implementation rules
> **Primary principle:** **NeoMundi measures. The consuming system interprets, governs, and acts.**

---

## 1. Purpose

The NeoMundi Metric Contract defines the observable interface between the NeoMundi runtime measurement layer and external systems that consume NeoMundi signals.

Its purpose is to make NeoMundi measurements:

* explicit;
* machine-consumable;
* versioned;
* interpretable;
* reproducible;
* interoperable;
* safe to consume without reverse-engineering internal NeoMundi logic.

This contract defines **what NeoMundi outputs mean**.

It also defines the boundary between:

1. **Measurement** — what NeoMundi observes and emits;
2. **Interpretation** — what can legitimately be inferred from those measurements;
3. **Consumer policy** — what an external system chooses to do with them;
4. **Execution** — what action is eventually performed on an AI system.

The intended architecture is:

```text
AI generation
     │
     ▼
NeoMundi measurement
     │
     ▼
Runtime signals
     │
     ▼
Consumer interpretation / policy
     │
     ▼
CONTINUE / VERIFY / STOP / REGENERATE / REROUTE / ABSTAIN / other action
```

NeoMundi is therefore a **runtime measurement layer**, not a universal policy engine.

---

# 2. Normative language

The terms **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** are used in their normative sense.

Three information classes are distinguished throughout this repository.

### NORMATIVE

Part of the Metric Contract.

Consumers may implement against it.

### EMPIRICAL

Observed in NeoMundi experiments or external studies.

It MUST NOT automatically be converted into a protocol rule.

### INTERNAL / NON-CONTRACTUAL

Implementation details that may exist inside NeoMundi but are not required for correct external consumption of the measurement.

---

# 3. Fundamental boundary

A NeoMundi signal is a **measurement signal**, not by itself a verdict about truth, safety, legality, or business acceptability.

In particular:

```text
high stability ≠ factual correctness
```

and:

```text
FLAG ≠ proven error
```

and:

```text
ALLOW ≠ proven truth
```

The consuming infrastructure remains responsible for determining what operational consequence should follow from a NeoMundi measurement.

---

# 4. Runtime signal model

The currently documented NeoMundi runtime signal family contains the following principal objects.

| Signal                | Type          | Meaning                             | Contract status                    |
| --------------------- | ------------- | ----------------------------------- | ---------------------------------- |
| `G` / `g_score`       | numerical     | Runtime generative stability signal | Defined conceptually               |
| `stability_score`     | float `[0,1]` | Aggregate generation stability      | Defined                            |
| `delta_g` / `∆G`      | numerical     | Change in runtime stability         | Defined conceptually               |
| `delta_series`        | array         | Evolution of ∆G across generation   | Defined                            |
| `delta_variation`     | numerical     | Overall amplitude of ∆G variation   | Defined                            |
| `delta_profile`       | enum          | Shape of the ∆G trajectory          | Defined                            |
| `decision`            | enum          | Runtime classification              | Defined                            |
| `hallucination_score` | numerical     | Factual-risk-related signal         | Defined as exposed signal          |
| `coherence_score`     | numerical     | Semantic coherence signal           | Defined as exposed signal          |
| `regime`              | enum / state  | Synthetic runtime context           | Defined conceptually               |
| `total_tokens`        | integer       | Number of generated tokens          | Defined                            |
| `g_final`             | numerical     | Final G-related value               | Relationship pending formal freeze |

A field being present in this table does **not** imply that its internal computation is public.

---

# 5. G / G-score

## 5.1 Meaning

`G` represents a runtime property related to the stability, regularity, or coherence of the generative process.

It MUST NOT be interpreted as a direct factual truth score.

A high G value may coexist with an incorrect answer.

This phenomenon is referred to as:

> **deceptive stability**

or:

> **misleading stability**

Example:

```text
high G
+
factually incorrect answer
=
stable but incorrect generation
```

The correct operational conclusion is therefore:

```text
G measures runtime stability.
G does not establish factual correctness.
```

---

# 6. Relationship between G and factuality

No consumer MUST infer:

```text
G ↑  => factual correctness ↑
```

The available experimental work has shown that similar or high G values may coexist with substantially different factual accuracy.

Therefore factual validation MUST remain logically separable from runtime stability measurement.

Where factual correctness matters, an external factual validation layer MAY be used.

---

# 7. stability_score

`stability_score` represents overall generation stability.

Current documented range:

```text
0.0 <= stability_score <= 1.0
```

Interpretation direction:

```text
higher value -> greater measured runtime stability
lower value  -> lower measured runtime stability
```

It MUST NOT be interpreted as:

* factual confidence;
* probability that an answer is correct;
* safety probability;
* compliance probability.

---

# 8. Delta G — ∆G

`∆G` represents change in the runtime stability signal over the course of generation.

Conceptually:

```text
∆G > 0
```

indicates an increase in the measured stability variable over the corresponding interval.

```text
∆G < 0
```

indicates degradation in that measured variable over the corresponding interval.

The exact internal transformation producing ∆G MUST be taken from the implementation/versioned normalizer associated with the measurement.

Consumers MUST NOT manufacture their own ∆G from unrelated fields unless the contract explicitly defines that transformation.

---

# 9. delta_series

`delta_series` contains the temporal evolution of ∆G.

Each observation is associated with a position in the generation process, typically including a token position and numerical ∆G value.

Conceptual representation:

```json
[
  {
    "token": 60,
    "delta_g": -0.08
  },
  {
    "token": 80,
    "delta_g": -0.03
  }
]
```

The JSON above is illustrative only.

The authoritative serialization MUST be defined by the versioned ENF schema.

---

# 10. delta_profile

The currently documented ∆G profiles are:

```text
DROP
FLAT
V_SHAPE
```

### DROP

A degradation of the stability signal without sufficient subsequent recovery.

Operational interpretation:

```text
runtime degradation / rupture / tension
```

### FLAT

A largely stable trajectory without substantial observed degradation or recovery pattern.

Operational interpretation:

```text
stable or approximately stable ∆G trajectory
```

### V_SHAPE

A degradation followed by partial recovery.

Operational interpretation:

```text
temporary degradation followed by recovery
```

These profiles describe the **shape of the signal**.

They MUST NOT automatically be interpreted as factual judgments.

---

# 11. ALLOW and FLAG

The documented final runtime classification is binary:

```text
ALLOW
FLAG
```

## ALLOW

`ALLOW` means that the NeoMundi runtime measurement did not classify the generation as requiring a runtime alert under the applicable measurement configuration.

It MUST NOT mean:

```text
true
safe
approved
compliant
factually verified
```

## FLAG

`FLAG` means that the NeoMundi runtime measurement identified a generation deserving attention under the applicable measurement configuration.

It MAY be consumed as a trigger for:

* additional verification;
* escalation;
* human review;
* regeneration;
* rerouting;
* early stopping;
* logging;
* evidence generation.

It MUST NOT be interpreted automatically as proof that the output is wrong.

---

# 12. Relationship between DROP and FLAG

Experimental NeoMundi replication work has observed a very strong relationship between `DROP` and `FLAG`, including a perfect correspondence in several tested campaigns.

This is currently classified as:

```text
EMPIRICAL
```

unless a specific Metric Contract version explicitly promotes this relationship to:

```text
NORMATIVE
```

A consumer MUST therefore not independently assume:

```text
DROP ⇔ FLAG
```

as a universal invariant solely on the basis of experimental observations.

---

# 13. Runtime regime

A runtime `regime` may provide a synthetic description of system state.

One documented state is:

```text
STABLE
```

`STABLE` MUST be understood as runtime context.

It MUST NOT independently establish:

* factual correctness;
* absence of hallucination;
* absence of FLAG;
* absence of DROP;
* authorization to skip verification.

A consumer SHOULD interpret regime together with other runtime signals.

---

# 14. Multi-signal interpretation

NeoMundi SHOULD be consumed as a multi-signal measurement system.

The minimum conceptual combination is:

```text
G
+
∆G
+
delta_profile
+
decision
+
regime
+
task-relevant validation signals
```

No single signal SHOULD be treated as a complete characterization of generative risk.

Examples:

### High G + factual failure

```text
Interpretation:
deceptive stability

Possible consumer action:
external factual verification
```

### FLAG + DROP

```text
Interpretation:
priority runtime degradation zone

Possible consumer action:
verification / escalation / stopping / regeneration
```

### STABLE alone

```text
Interpretation:
insufficient evidence for operational conclusion

Consumer action:
inspect additional signals
```

### ALLOW + factual failure

```text
Interpretation:
runtime stability did not detect the factual failure

Consumer action:
factual validation remains necessary where the task requires it
```

---

# 15. Factual and semantic signals

NeoMundi may expose complementary signals including:

```text
hallucination_score
coherence_score
```

These signals represent dimensions distinct from generative stability.

Conceptually:

```text
stability
factuality
semantic coherence
```

are separate axes.

They MUST NOT be silently collapsed into a single notion of "quality".

The exact range, normalization, model, calibration, and version of each auxiliary score MUST be provided by its corresponding metric definition before it is used normatively.

---

# 16. Thresholds

## 16.1 No inferred thresholds

Consumers MUST NOT derive official NeoMundi thresholds from published exploratory studies.

For example, the criterion:

```text
∆G < 0
```

has been used experimentally to identify the first observable degradation event in an early-stopping study.

It was intentionally selected as an exploratory detection criterion.

It is **not automatically an official NeoMundi stopping threshold**.

Likewise:

```text
∆G < -0.05
∆G < -0.10
∆G < -0.15
```

have been studied experimentally.

They MUST NOT be treated as normative unless explicitly introduced into a versioned contract.

---

# 17. Provider-, model-, task-, and context-specific thresholds

No consumer SHOULD assume that a numerical threshold is universally portable across:

* providers;
* models;
* model versions;
* prompt classes;
* task families;
* generation configurations;
* normalizer versions.

Research indicates that prompt composition and model/provider conditions may materially affect observed signal distributions.

Any context-specific threshold MUST therefore identify at minimum:

```text
metric_version
normalizer_version
provider/model scope
task/context scope
threshold value
threshold semantics
```

If these elements are not present, the threshold MUST be treated as non-authoritative.

---

# 18. Consumer authority and computational paths

This section is normative.

NeoMundi produces the measurement.

The consuming system owns the execution policy.

Therefore:

```text
NeoMundi
   ↓
measurement
   ↓
consumer C
   ↓
policy decision
   ↓
system B
```

A NeoMundi signal MAY cause C to select a different computational path for B.

Examples include:

```text
continue normally
perform additional verification
perform less verification
stop generation
regenerate
reroute
request human review
abstain
```

However, this occurs because **C defines that policy**, not because the NeoMundi measurement itself possesses universal execution authority.

---

# 19. Computational reduction and Token ROI

NeoMundi measurements MAY be used to study or implement computational reduction.

For example:

```text
runtime degradation detected
        ↓
consumer stopping policy
        ↓
generation terminated
        ↓
tokens avoided
```

An exploratory NeoMundi study defined:

```text
E-token = TotalTokens - TokenDetection
```

where `TokenDetection` was the first token position at which the experimental degradation criterion was detected.

This metric estimates the theoretical number of tokens that might have been avoided under an early-stopping policy.

The research demonstrates **actionability potential**.

It does not establish a universal canonical stopping rule.

---

# 20. Reduced verification

A NeoMundi measurement MAY support a consumer policy that reduces verification or selects a shorter computational path.

However:

> **No current general NeoMundi rule states that a high G, STABLE state, ALLOW state, or any single signal automatically authorizes reduced verification.**

For an experiment wishing to measure Token ROI from reduced verification, the reduction rule MUST therefore be defined explicitly as part of the consumer policy profile.

Example:

```text
NeoMundi measurement
+
C policy conditions satisfied
+
required signals present and current
=
reduced verification path permitted
```

Without such a policy definition:

```text
default assumption = no automatic reduction
```

---

# 21. CONTINUE, STOP and ABSTAIN

`CONTINUE`, `STOP`, and `ABSTAIN` SHOULD be understood as **consumer action states**, unless a future NeoMundi schema explicitly emits them as measurement-layer outputs.

### CONTINUE

Consumer continues the current computational path.

### STOP

Consumer terminates or interrupts the current computational path.

### ABSTAIN

Consumer declines to make an operational decision from the available measurement and transfers control to another path.

Examples:

```text
ABSTAIN -> request more evidence
ABSTAIN -> fallback policy
ABSTAIN -> human review
ABSTAIN -> safe default
```

These states MUST NOT be confused with:

```text
ALLOW
FLAG
STABLE
DROP
```

because the two categories represent different layers.

```text
ALLOW / FLAG / STABLE / DROP
        =
measurement / interpretation states

CONTINUE / STOP / ABSTAIN
        =
consumer action states
```

---

# 22. Conflict resolution

Conflicting signals are expected in a multidimensional measurement system.

Example:

```text
G = high
decision = ALLOW
factual signal = poor
```

This is not necessarily an internal contradiction.

It may indicate:

```text
stable generation
+
factual weakness
```

NeoMundi therefore does not reduce every signal conflict to a single universal scalar verdict.

General principle:

```text
signals retain their semantic dimension
```

rather than:

```text
all signals collapse into one meaning
```

Consumer-specific precedence rules MUST be explicit.

They MUST NOT be reverse-engineered from incidental correlations in research datasets.

---

# 23. Verification rules

NeoMundi distinguishes measurement from validation.

Verification SHOULD be triggered according to the requirements of the consuming application.

Examples where verification may be appropriate:

* `FLAG`;
* `DROP`;
* significant ∆G degradation;
* disagreement among runtime signals;
* high G with external factual concern;
* stale or incomplete evidence;
* high-risk application context;
* consumer-specific policy conditions.

No NeoMundi runtime stability signal alone proves factual correctness.

---

# 24. Missing signals

A consumer MUST distinguish:

```text
signal absent
```

from:

```text
signal observed with neutral / stable value
```

Missing data MUST NOT silently become:

```text
0
ALLOW
STABLE
safe
verified
```

The authoritative fallback policy for each required field MUST be specified in the schema/consumer profile.

Until such a policy is explicitly versioned, consumers SHOULD treat absence of required measurement information as:

```text
measurement unavailable
```

rather than infer a positive result.

---

# 25. Malformed payloads

A malformed payload MUST NOT be interpreted as a valid NeoMundi measurement.

Consumers SHOULD:

```text
reject measurement interpretation
log the validation failure
retain provenance
invoke consumer fallback policy
```

A malformed payload MUST NOT silently produce `ALLOW`.

---

# 26. Partial payloads

Streaming environments may expose partial state before completion.

A consumer MUST distinguish between:

```text
partial runtime observation
```

and:

```text
final measurement
```

Fields representing final aggregation or final classification MUST NOT be assumed valid before the schema-defined completion event.

---

# 27. Stale signals

Runtime measurements are temporally bound.

A consumer MUST NOT apply a signal to an unrelated generation or turn.

Each consumed signal SHOULD be associated with sufficient provenance to identify:

```text
request
generation
turn
timestamp
measurement version
```

A signal that cannot be causally linked to the current consumer decision SHOULD be treated as unavailable.

---

# 28. Turn causality

Default conceptual rule:

```text
measurement(t) applies to the generation / turn from which it was produced
```

A signal from turn `t-1` MUST NOT automatically govern turn `t`.

Cross-turn reuse requires an explicit longitudinal consumer policy.

The authoritative protocol SHOULD bind each signal to identifiers such as:

```text
conversation_id
turn_id
generation_id
measurement_id
timestamp
```

The exact field names belong to the authoritative interoperability schema.

---

# 29. Signal lifetime

NeoMundi does not define an abstract universal time-to-live independent of causality.

The preferred validity rule is identity-based:

```text
signal valid for the generation it measures
```

rather than:

```text
signal valid for N arbitrary seconds
```

Any longer persistence or longitudinal reuse MUST be defined by the consuming application.

---

# 30. Versioning

Every production-grade NeoMundi measurement SHOULD expose sufficient version information to identify its interpretation contract.

The versioning model SHOULD distinguish:

```text
schema_version
metric_version
normalizer_version
```

These versions refer to different things.

### schema_version

Serialization and payload structure.

### metric_version

Definition and semantics of the metric.

### normalizer_version

Transformation/calibration applied to the raw or intermediate measurement.

A change in one MUST NOT silently masquerade as a change in another.

---

# 31. Compatibility

Consumers SHOULD use explicit compatibility rules.

Conceptually:

```text
same major schema version
+ compatible metric semantics
+ recognized normalizer
=
consumable
```

A consumer MUST NOT silently consume an unknown measurement version as if it were semantically identical to a known one.

Unknown incompatible versions SHOULD enter fallback handling.

---

# 32. G, g_score, g_final, stability_score and delta_g

These identifiers MUST NOT be assumed to be synonyms unless explicitly defined as such by the corresponding metric implementation/version.

Current conceptual hierarchy:

```text
G / g_score
     │
     ├── runtime stability-related measurement
     │
     └── evolution through generation
              │
              ▼
             ∆G
```

`stability_score` represents an overall generation-level stability score.

The exact mathematical relationship among:

```text
G
g_score
g_final
stability_score
delta_g
```

is **not fully specified by the external exploratory research reports alone**.

Therefore:

> Consumers MUST NOT invent conversions, equivalences, coefficients, or normalization formulas among these fields.

The authoritative relationship MUST be supplied by the versioned NeoMundi metric implementation contract.

---

# 33. Internal formulas

The Metric Contract distinguishes:

```text
consumer-required semantics
```

from:

```text
internal computation
```

A consumer does not necessarily require access to every proprietary internal formula in order to use a measurement correctly.

Where an internal mechanism remains proprietary, NeoMundi MAY expose instead:

* output semantics;
* domain/range;
* calibration/version identifier;
* interpretation direction;
* uncertainty/limitations;
* invariants required for interoperability.

No consumer SHOULD reverse-engineer missing internal coefficients from empirical outputs.

---

# 34. Normalization

Normalization is part of the measurement definition.

If a metric is normalized, its contract SHOULD specify:

```text
input domain
output domain
normalizer_version
interpretation direction
boundary behavior
invalid-input behavior
```

Consumers MUST NOT independently renormalize NeoMundi values and continue to call them canonical NeoMundi signals unless explicitly allowed by the contract.

---

# 35. Canonical ENF payload

The authoritative ENF payload MUST be defined by a machine-readable JSON Schema stored in this repository.

Recommended repository location:

```text
/schema/enf-runtime.schema.json
```

The README describes semantics.

The JSON Schema defines serialization.

The Metric Contract defines meaning.

The interoperability contract defines exchange.

These layers SHOULD remain separate.

---

# 36. Example conceptual payload

The following example is **illustrative and non-authoritative** until matched to the versioned JSON Schema.

```json
{
  "schema_version": "1.x",
  "metric_version": "1.x",
  "normalizer_version": "1.x",
  "measurement_id": "...",
  "generation_id": "...",
  "timestamp": "...",
  "stability_score": 0.91,
  "delta_profile": "FLAT",
  "delta_variation": 0.02,
  "decision": "ALLOW",
  "hallucination_score": 0.0,
  "coherence_score": 0.96,
  "total_tokens": 241
}
```

Consumers MUST use the JSON Schema rather than this example as the serialization authority.

---

# 37. Example consumer scenarios

## Scenario A — Stable runtime measurement

```text
stability_score = high
delta_profile = FLAT
decision = ALLOW
```

NeoMundi meaning:

```text
generation appears runtime-stable under the applicable measurement
```

Not implied:

```text
factually true
safe
approved
verification unnecessary
```

Possible C behavior:

```text
CONTINUE
```

if explicitly permitted by C policy.

---

## Scenario B — Runtime degradation

```text
delta_profile = DROP
decision = FLAG
```

NeoMundi meaning:

```text
runtime degradation detected
```

Possible C behavior:

```text
VERIFY
STOP
REGENERATE
REROUTE
HUMAN_REVIEW
```

according to consumer policy.

---

## Scenario C — Deceptive stability

```text
G = high
runtime state = stable
external factual validation = fail
```

Interpretation:

```text
stable but incorrect
```

Possible C behavior:

```text
FLAG factual layer
request verification
do not trust stability as factual evidence
```

---

## Scenario D — Missing measurement

```text
required NeoMundi signal unavailable
```

Interpretation:

```text
measurement unavailable
```

Not:

```text
ALLOW
```

Possible C behavior:

```text
ABSTAIN
fallback
full verification
```

according to consumer policy.

---

# 38. Consumer Policy Profile

To make experiments reproducible, systems consuming NeoMundi SHOULD publish a separate policy profile.

For example:

```text
C Policy Profile v1.0
```

containing:

```text
input signals
required signals
optional signals
verification rules
reduced-verification rules
early-stop rules
fallback rules
precedence rules
action mappings
```

This prevents the consumer policy from being confused with the measurement itself.

---

# 39. Experimental policies

An experiment MAY define a policy such as:

```text
if condition X:
    STOP
```

or:

```text
if condition Y:
    reduce verification
```

provided that the experiment clearly labels the rule:

```text
EXPERIMENT-SPECIFIC CONSUMER POLICY
```

and does not describe it as:

```text
NeoMundi universal measurement semantics
```

This distinction is essential for reproducibility.

---

# 40. Token ROI experiments

For Token ROI benchmarking, three quantities SHOULD remain separate:

```text
measurement
policy
savings
```

Example:

```text
NeoMundi detects event at token T
        ↓
C policy chooses STOP
        ↓
generation ends at T
        ↓
Token ROI computed
```

The benchmark therefore evaluates:

> the value of consuming the NeoMundi measurement under a specific policy

rather than:

> a universal property that NeoMundi always stops generation.

---

# 41. Research evidence vs protocol rules

NeoMundi research may reveal statistically or operationally strong relationships.

Examples include:

* correlation between DROP and FLAG;
* lower stability scores among FLAG cases;
* differences between prompt classes;
* early detectability of degradation;
* potential token savings from early stopping.

These findings may inform future protocol versions.

They do not automatically become protocol rules.

The lifecycle is:

```text
observation
    ↓
replication
    ↓
interpretation
    ↓
contract decision
    ↓
versioned normative rule
```

---

# 42. Research basis

The current Metric Contract is informed by external exploratory work including:

### Pape Malick DIOP

**Audit exploratoire des signaux runtime NeoMundi — Analyse de G, ∆G, FLAG, régime et exactitude benchmark**

Key contribution:

* separation between runtime stability and factual accuracy;
* deceptive stability;
* multi-signal interpretation;
* interpretation of G, ∆G, FLAG, DROP and regime.

### Fatima Ezzahrae GOUARAB

**Actionability of the NeoMundi ControlTower Runtime Signal — Evaluation of a Governance Policy Based on Early Stopping of Generation**

Key contribution:

* runtime actionability;
* early detection;
* E-token;
* experimental early-stopping policy;
* distinction between experimental threshold and official threshold.

### Fatima Ezzahrae GOUARAB

**Stabilité et reproductibilité du signal runtime NeoMundi ControlTower — Analyse comparative multi-providers, multi-corpus et multi-configurations**

Key contribution:

* multi-provider replication;
* influence of task/prompt composition;
* reproducibility of signal relationships;
* strong empirical correspondence between DROP and FLAG under tested configurations.

These studies are evidence supporting interpretation.

They are not substitutes for the versioned Metric Contract.

---

# 43. What this repository does not claim

This repository does not claim that:

```text
G measures truth
FLAG proves hallucination
ALLOW proves correctness
DROP universally equals FLAG
STABLE permits verification bypass
∆G < 0 is the universal NeoMundi stopping threshold
```

It also does not require publication of proprietary internal computation unless that computation is necessary for interoperability.

---

# 44. Implementation freeze

A consumer implementation may be considered **frozen against NeoMundi** only when the following are fixed:

```text
schema_version
metric_version
normalizer_version
consumer_policy_version
```

The freeze record SHOULD therefore resemble:

```text
NeoMundi Metric Contract: X
ENF Schema: Y
Normalizer: Z
Consumer Policy C: N
```

This allows later NeoMundi evolution without silently altering an already-running benchmark.

---

# 45. Required artifacts for a complete freeze

A complete implementation package SHOULD contain:

```text
README.md
METRIC_CONTRACT.md
schema/enf-runtime.schema.json
schema/examples/
consumer/C_POLICY_PROFILE.md
VERSIONING.md
CHANGELOG.md
```

Recommended example fixtures:

```text
allow-flat.json
flag-drop.json
stable-factual-failure.json
partial-payload.json
missing-signal.json
malformed-payload.json
unknown-version.json
```

Each fixture SHOULD include the expected consumer interpretation.

---

# 46. Recommended repository structure

```text
neomundi-metric-contract/
│
├── README.md
├── METRIC_CONTRACT.md
├── VERSIONING.md
├── CHANGELOG.md
│
├── schema/
│   ├── enf-runtime.schema.json
│   └── examples/
│       ├── allow-flat.json
│       ├── flag-drop.json
│       ├── partial-payload.json
│       └── missing-signal.json
│
├── consumer/
│   ├── C_POLICY_PROFILE.md
│   └── CONSUMPTION_RULES.md
│
├── research/
│   └── README.md
│
└── docs/
    ├── SIGNAL_DICTIONARY.md
    ├── INTERPRETATION_MATRIX.md
    └── INTEROPERABILITY.md
```

---

# 47. Canonical principle

The NeoMundi architecture can be summarized as:

```text
We measure.
You interpret.
You govern.
You act.
```

Or, technically:

```text
Measurement ≠ Policy ≠ Execution
```

NeoMundi provides the independent runtime measurement layer.

The surrounding infrastructure remains in control of interpretation, policy, and action.

---

# 48. Current contract status

The following elements are sufficiently documented to support implementation work:

* runtime stability as a distinct measurement dimension;
* `stability_score`;
* `ALLOW / FLAG`;
* `DROP / FLAT / V_SHAPE`;
* ∆G trajectory;
* `delta_series`;
* `delta_variation`;
* complementary factual/coherence signals;
* separation of measurement and consumer action;
* possibility of consumer-side early stopping and other computational paths.

The following elements require explicit normative freezing before they can be treated as canonical:

* exact relationship among `G`, `g_score`, `g_final`, `stability_score`, and `delta_g`;
* internal normalization definition where externally required;
* authoritative ENF JSON Schema;
* exhaustive enum set;
* official numerical thresholds, if any;
* exact compatibility table;
* required/optional field matrix;
* canonical malformed/stale/partial payload handling;
* any NeoMundi-defined reduced-verification profile.

Until those elements are versioned, consumers MUST NOT infer them.

---

# 49. Contract philosophy

NeoMundi deliberately preserves a separation between:

```text
what was measured
```

and:

```text
what should be done
```

This separation allows the same measurement primitive to be consumed by different infrastructures without forcing them into a single governance model.

One measurement layer.

Multiple applications.

Multiple policies.

Multiple infrastructures.

Without changing the measurement itself.

---

**NeoMundi Research**
Runtime Measurement for AI Systems
