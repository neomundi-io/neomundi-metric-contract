# NeoMundi Metric Contract — Normative Audit v0.0

**Status: Working document — Non-normative**  
**Purpose: Audit of normative language used in the NeoMundi Metric Contract Draft v0.0**  
**Maintainer: NeoMundi**  
**Date: 13 August 2026**

[🇬🇧 English](#english) · [🇫🇷 Français](#francais)

---

<a id="english"></a>

## 🇬🇧 English

### 1. Purpose of this audit

This document reviews the normative language used in the NeoMundi Metric Contract Draft v0.0.

It does not replace the Metric Contract.

Its purpose is to determine, section by section, which statements are mature enough to be expressed as:

- MUST / MUST NOT — binding semantic requirements;

- SHOULD / SHOULD NOT — recommended requirements from which a justified implementation may depart;

- MAY / OPTIONAL — permitted but non-mandatory behaviour;

- DESCRIPTIVE — explanatory language that should not be normative;

DEFER — a point that should remain outside the Metric Contract until the Runtime Interoperability Contract or another specification defines it.

The objective is to avoid false MUST statements and to ensure that every normative requirement expresses a boundary NeoMundi is genuinely prepared to maintain.

### 2. Sources used for the audit

This audit uses four sources with different roles.

#### A. NeoMundi API observation payload

The operational reference is the structure currently exposed by the NeoMundi API, including:

- schema_version;

- observation_id;

- generated_at;

- synthetic;

- source;

- measurement;

- known_limitations;

measurement_boundary.

This is the primary operational anchor for what the measurement layer currently exposes.

#### B. NeoMundi Metric Contract Draft v0.0

The current contract defines the semantic interpretation of the API payload and the boundary between measurement and external decision-making.

#### C. Pape Malick DIOP — external exploratory runtime-signal audit

This report is used as evidence for interpretation boundaries, in particular:

- runtime stability is not factual truth;

- a single signal is not sufficient as a decision basis;

- FLAG is an attention/escalation signal rather than automatic proof of error;

runtime signals should be interpreted in combination and context.

The report remains exploratory and does not constitute final mathematical validation or product certification.

#### D. Fatima Ezzahrae GOUARAB — external exploratory replication note

The document provided for this audit is the replication note on the stability and reproducibility of NeoMundi ControlTower signals.

It is not the earlier actionability report itself.

It is used here to support methodological caution around:

- reproducibility;

- provider, prompt and configuration effects;

- the stability of observed signal behaviour;

the need to avoid generalising experimental findings beyond tested conditions.

### 3. Hierarchy used during the audit

For Draft v0.0, the following hierarchy is applied:

The API payload anchors what NeoMundi currently exposes operationally.

The Metric Contract defines what those exposed measurements and signals mean.

External exploratory reports support or challenge interpretation, but do not define the API schema.

The Runtime Interoperability Contract will define transport, field requirements, types, negotiation and integration constraints.

If a semantic rule is not sufficiently supported or stable, it should not become a MUST.

If a rule concerns transport or integration structure rather than measurement meaning, it should normally be deferred to the Runtime Interoperability Contract.

### 4. Audit decisions

The following labels are used:

- KEEP — retain the normative force.

- STRENGTHEN — increase the normative force because the statement defines a fundamental semantic boundary.

- WEAKEN — reduce normative force because the rule is not mature enough to be mandatory.

- DESCRIPTIVE — remove normative wording and keep the statement explanatory.

- DEFER — move the requirement to the Runtime Interoperability Contract or another future specification.

### 5. Section-by-section audit

#### Normative language

**Decision: KEEP, with one precision.**

Keep the definitions of MUST / MUST NOT / SHOULD / SHOULD NOT / MAY / OPTIONAL.

The normative scope should remain limited to the meaning, interpretation and semantic boundaries of the measurement layer.

Field names, exact JSON locations, enums and transport constraints should not become normative merely because they appear in the current payload.

> A change that materially changes measurement meaning **MUST** be explicitly versioned.

#### Section 1 — Purpose

**Decision: STRENGTHEN the authority boundary.**

The distinction between measurement and external decision-making is foundational.

**Recommended normative rule:**

> The Metric Contract ****MUST** NOT** define a policy decision, execution authorization or operational action as an intrinsic meaning of a NeoMundi measurement.

The statement that the consuming system retains decision and action authority should remain a core semantic principle.

#### Section 2 — Reference runtime object

**Decision: KEEP descriptive structure; KEEP one strong boundary.**

The current API payload structure should be documented as the operational reference.

However, exact field placement and machine structure should not yet be frozen by the Metric Contract.

**Recommended normative rule:**

> The Metric Contract ****MUST** NOT** introduce a competing machine representation that changes the meaning of the measurements actually exposed by NeoMundi.

Exact serialization requirements should be deferred to the Runtime Interoperability Contract.

#### Section 3 — Observation

**Decision: KEEP.**

**Recommended rules:**

> A NeoMundi measurement **MUST** remain attributable to the observation to which it belongs.

> A synthetic observation ****MUST** NOT** be represented as a production observation.

These rules concern traceability and semantic integrity rather than transport format.

#### Section 4 — Payload version

**Decision: MODIFY.**

schema_version describes the machine representation of the payload.

The Metric Contract should explain this fact but should not define every future schema-versioning rule.

**Recommended rules:**

> A change that materially alters the semantic meaning of an exposed measurement **MUST** be explicitly distinguishable through versioning.

Exact schema compatibility and negotiation rules should be DEFERRED to the Runtime Interoperability Contract.

Avoid using MUST for purely structural changes that do not affect semantics.

#### Section 5 — Observation source

**Decision: KEEP.**

**Recommended strong rule:**

> Information that is not explicitly present or declared in the observation context ****MUST** NOT** be silently inferred as measured or known.

In particular, a model alias alone must not be treated as proof of provider identity, exact model version or execution configuration.

This is a semantic integrity rule.

#### Section 6 — Measurement block

**Decision: KEEP, but avoid making container structure normative.**

The measurement block is the currently observed container for measurement outputs.

Its exact JSON placement belongs to interoperability.

**Recommended semantic rule:**

> A measurement or signal ****MUST** NOT** be interpreted independently of applicable measurement limitations and boundaries when those limitations materially affect its meaning.

#### Section 7 — Measurement status

**Decision: KEEP.**

**Recommended rules:**

> partial ****MUST** NOT** be interpreted as equivalent to complete.

> The presence of a numerical value ****MUST** NOT** by itself imply complete measurement coverage.

These rules protect against overinterpretation of incomplete observations.

#### Section 8 — Measurement coverage

**Decision: KEEP, with wording focused on meaning rather than field presence.**

Recommended rule:

> Known incomplete coverage **MUST** remain distinguishable from complete coverage in any interpretation of the measurement.

Do not require the exact field name or serialization here.

Those requirements belong to interoperability.

#### Section 9 — Stability score

**Decision: KEEP and STRENGTHEN.**

This is one of the strongest normative boundaries in the contract.

**Recommended rules:**

> stability_score ****MUST** NOT** be interpreted as factual truth.

> stability_score ****MUST** NOT** by itself establish safety, compliance, admissibility or execution authorization.

A high stability value MAY coexist with a factually incorrect output.

**The Metric Contract should preserve the principle:**

> **Stability is not truth.**

#### Section 10 — Coherence score

**Decision: KEEP.**

Recommended rule:

> coherence_score ****MUST** NOT** by itself be interpreted as factual validation, safety validation, compliance determination or execution authorization.

The score may be consumed together with other signals, but the Metric Contract should not prescribe the consumer's policy.

#### Section 11 — Factual validity signal

**Decision: KEEP, with caution.**

**Recommended rules:**

> A runtime factual signal based only on runtime evidence ****MUST** NOT** be represented as independent verification of truth.

> A null or not_assessed factual score ****MUST** NOT** be interpreted as zero factual risk or as factual correctness.

Status labels observed in current payload examples should remain descriptive until their enum is formally frozen.

The exact taxonomy should be deferred to the interoperability/schema work unless separately versioned as a semantic taxonomy.

#### Section 12 — Semantic variability signal

**Decision: KEEP.**

**Recommended rules:**

> Low observed semantic variability ****MUST** NOT** be interpreted as evidence of factual correctness.

> Elevated semantic variability ****MUST** NOT** by itself identify the cause of variation.

> Elevated semantic variability ****MUST** NOT** by itself identify which output is correct.

These are interpretation boundaries, not policy decisions.

#### Section 13 — Latency and cost bands

**Decision: WEAKEN / mostly DESCRIPTIVE.**

The existence of latency and cost bands may be documented.

However, the Metric Contract should not yet freeze:

- the exact allowed enum values;

- thresholds defining low, medium or other bands;

routing consequences.

Recommended rule:

> An explicitly unknown latency or cost value ****MUST** NOT** be silently interpreted as a measured value.

Everything else should remain descriptive or be deferred.

#### Section 14 — Risk signal

**Decision: KEEP and STRENGTHEN the decision boundary.**

**Recommended rules:**

> A NeoMundi risk_signal ****MUST** NOT** automatically mean ALLOW, BLOCK, ACCEPT, REJECT, COMPLIANT, NON-COMPLIANT, SAFE, UNSAFE, ADMISSIBLE or NON-ADMISSIBLE.

> A consuming system **MAY** use the signal as an input to its own policy.

The resulting policy decision remains external to the Metric Contract.

The currently observed levels and risk types should not automatically become frozen enums in Draft v0.0.

#### Section 15 — Known limitations

**Decision: STRENGTHEN semantics, avoid requiring literal strings.**

The current wording saying limitations "SHOULD NOT be removed" is too tied to transport.

Better normative rule:

> Any limitation that materially affects interpretation of a measurement **MUST** remain semantically associated with that measurement when it is interpreted or transmitted for decision support.

The literal wording, storage location or serialization of limitations should be deferred to interoperability.

#### Section 16 — Measurement boundary

**Decision: KEEP and STRENGTHEN.**

This is a core normative section.

Recommended rule:

> A NeoMundi measurement ****MUST** NOT**, by itself, be interpreted as establishing truth, safety, authority, downstream permission or execution admissibility unless a future explicitly versioned metric states otherwise.

This boundary protects the distinction between measurement and decision.

#### Section 17 — Separation between measurement and decision

**Decision: KEEP and STRENGTHEN.**

**Recommended rules:**

> NeoMundi **MUST** remain authoritative over the declared semantics of its measurements.

> The consuming system **MUST** remain responsible for the policy or operational action it derives from those measurements.

> External systems **MAY** use NeoMundi measurements for logging, verification, regeneration, rerouting, escalation, human supervision, interruption, compliance workflows or evidence workflows.

The examples of actions are non-exhaustive and non-normative.

#### Section 18 — Infrastructure neutrality

**Decision: KEEP.**

**Recommended rules:**

> The Metric Contract ****MUST** NOT** impose one downstream use as the unique interpretation of a measurement.

> A measurement **MAY** be consumed by multiple infrastructures and for multiple purposes.

Infrastructure-specific policy logic should remain outside the Metric Contract.

#### Section 19 — Unknown, null and non-assessed values

**Decision: KEEP and STRENGTHEN.**

**Recommended rules:**

null MUST NOT be interpreted as zero.

unknown MUST NOT be interpreted as low, high or any measured category.

not_assessed MUST NOT be interpreted as absence of risk or successful validation.

The distinction between unavailable, unknown, not assessed and measured values MUST be preserved semantically.

Exact encoding rules belong to interoperability.

#### Section 20 — Partial measurement

**Decision: KEEP.**

**Recommended rules:**

A partial observation MUST NOT be represented as complete.

A value available within a partial observation MUST NOT imply that all measurement dimensions were assessed.

Consumers SHOULD consider declared coverage when interpreting partial observations.

#### Section 21 — Traceability

**Decision: WEAKEN and DEFER structural requirements.**

The current presence of:

- observation_id;

- source_batch_id;

- trace_id;

should be documented.

However, the Metric Contract should not yet impose the exact required traceability payload structure.

**Recommended semantic rule:**

> A measurement **SHOULD** remain attributable to its originating observation.

Requirements for hashes, signatures, receipts, required identifiers and cryptographic integrity should be deferred to the Runtime Interoperability Contract or evidence-layer specifications.

#### Section 22 — Confidentiality and non-exposed data

**Decision: DESCRIPTIVE / DEFER.**

The current synthetic payload examples state that they do not expose raw prompts, raw model outputs, provider identity, customer data or proprietary schema.

This observation should not automatically become a universal Metric Contract prohibition.

Privacy and data-exposure requirements should be specified separately or in interoperability/security documentation.

**Recommended wording:**

The Metric Contract describes only the data and measurements exposed by the measurement interface and does not imply access to internal computation data.

Do not introduce a MUST NOT expose raw prompt requirement here unless NeoMundi explicitly chooses to make that a permanent product-level contract.

#### Section 23 — Interoperability boundary

**Decision: KEEP.**

**Recommended rules:**

> The Metric Contract **MUST** define measurement semantics.

The Runtime Interoperability Contract SHOULD define transport, required fields, data types, compatibility and version negotiation.

> The Metric Contract ****MUST** NOT** freeze transport details that have not yet been validated as part of interoperability.

This section should preserve the separation between semantic contract and transport contract.

#### Section 24 — Semantic versioning rule

**Decision: KEEP, with stronger wording.**

**Recommended rules:**

> A material change in the meaning of a measurement or signal **MUST** be explicitly versioned.

> Historical observations **SHOULD** remain interpretable according to the semantic version under which they were produced.

> Pure serialization changes that do not alter meaning **MAY** be handled by schema/interoperability versioning instead.

#### Section 25 — Reference payload

**Decision: KEEP as NON-NORMATIVE example.**

The reference payload is essential because it anchors the contract in the real API representation.

However:

- example numeric values are non-normative;

- example identifiers are non-normative;

- observed status labels and enums are not automatically frozen;

exact field requirements remain subject to interoperability formalisation.

**The payload should be explicitly labelled:**

Operational reference example — not a complete normative JSON Schema.

#### Section 26 — Fundamental principle

**Decision: KEEP.**

The section should remain mostly declarative rather than overloaded with normative keywords.

**The core principle is:**

NeoMundi provides contextualised measurements and signals about observable AI-system behaviour within declared runtime boundaries. Those measurements can be consumed by multiple systems and infrastructures without transferring downstream decision authority to NeoMundi.

This is the semantic centre of the Metric Contract.

### 6. Normative core emerging from the audit

After review, the strongest MUST / MUST NOT requirements should concentrate on a small number of durable boundaries:

> A measurement **MUST** remain attributable to its observation.

Synthetic observations MUST NOT be represented as production observations.

Undeclared information MUST NOT be silently inferred as measured or known.

Partial measurement MUST NOT be represented as complete.

Stability MUST NOT be interpreted as factual truth.

Coherence MUST NOT be interpreted as factual validation by itself.

Runtime factual signals MUST NOT be presented as independent truth verification when they are not independently grounded.

Semantic variability MUST NOT be treated as proof of factual correctness or as identification of the correct output.

Risk signals MUST NOT automatically become policy or execution decisions.

Material measurement limitations MUST remain semantically associated with interpretation.

> A NeoMundi measurement ****MUST** NOT** by itself establish truth, safety, authority, downstream permission or execution admissibility.

Unknown, null and non-assessed states MUST NOT be silently converted into measured values.

Material semantic changes MUST be versioned.

> The Metric Contract ****MUST** NOT** freeze unvalidated transport or interoperability details.

The consuming system retains authority over the decisions and actions it derives from NeoMundi measurements.

This deliberately small normative core is stronger than a contract containing many weak or premature MUST statements.

### 7. Elements explicitly deferred

The following elements should not be frozen by Metric Contract Draft v0.0 unless separately validated:

- definitive JSON Schema;

- definitive required-field list;

- definitive field names;

- definitive enums;

- transport protocol;

- version negotiation;

- cryptographic receipts;

- signatures and hashes;

- evidence packaging;

- provider disclosure rules;

- exact threshold values;

- exact band boundaries;

- automatic routing rules;

- automatic stop rules;

- compliance determination;

- admissibility determination;

execution authorization.

These belong to the Runtime Interoperability Contract, evidence specifications, product configuration or consuming-system policy.

### 8. Next step

Once this audit is accepted:

- rewrite metric-contract-v0.0.fr.md using the audited normative core;

- remove or weaken premature normative statements;

- preserve the actual API payload as the operational reference;

- generate the English version as a strict semantic mirror;

- perform a final FR/EN consistency review;

only then prepare the next version or machine-readable schema work.

---

<a id="francais"></a>

## 🇫🇷 Français

### 1. Objet de cet audit

Ce document examine le langage normatif utilisé dans le NeoMundi Metric Contract Draft v0.0.

Il ne remplace pas le Metric Contract.

Son objectif est de déterminer, section par section, quelles formulations sont suffisamment mûres pour être exprimées comme :

- DOIT / NE DOIT PAS — exigences sémantiques obligatoires ;

- DEVRAIT / NE DEVRAIT PAS — exigences recommandées auxquelles une implémentation peut déroger avec justification ;

- PEUT / OPTIONNEL — comportement autorisé mais non obligatoire ;

- DESCRIPTIF — formulation explicative qui ne doit pas être normative ;

À DIFFÉRER — point qui doit rester hors du Metric Contract tant que le Runtime Interoperability Contract ou une autre spécification ne l’a pas défini.

L’objectif est d’éliminer les faux DOIT et de garantir que chaque exigence normative corresponde à une frontière que NeoMundi est réellement prêt à maintenir.

### 2. Sources utilisées pour l’audit

Cet audit utilise quatre sources ayant des rôles différents.

#### A. Payload d’observation de l’API NeoMundi

La référence opérationnelle est la structure actuellement exposée par l’API NeoMundi, notamment :

- schema_version ;

- observation_id ;

- generated_at ;

- synthetic ;

- source ;

- measurement ;

- known_limitations ;

measurement_boundary.

Elle constitue l’ancrage opérationnel principal de ce que la couche de mesure expose actuellement.

#### B. NeoMundi Metric Contract Draft v0.0

Le contrat actuel définit l’interprétation sémantique du payload API et la frontière entre mesure et décision externe.

#### C. Pape Malick DIOP — audit exploratoire externe des signaux runtime

Ce rapport est utilisé comme appui pour les frontières d’interprétation, notamment :

- la stabilité runtime n’est pas la vérité factuelle ;

- un signal isolé n’est pas une base suffisante de décision ;

- FLAG est un signal d’attention ou d’escalade et non une preuve automatique d’erreur ;

les signaux runtime doivent être interprétés de manière combinée et contextualisée.

Le rapport reste exploratoire et ne constitue ni une validation mathématique définitive ni une certification du produit.

#### D. Fatima Ezzahrae GOUARAB — note exploratoire externe de réplication

Le document fourni pour cet audit est la note portant sur la stabilité et la reproductibilité des signaux NeoMundi ControlTower.

Il ne s’agit pas du rapport d’actionnabilité antérieur lui-même.

Il est utilisé ici pour soutenir la prudence méthodologique concernant :

- la reproductibilité ;

- les effets du provider, du prompt et de la configuration ;

- la stabilité du comportement observé des signaux ;

la nécessité de ne pas généraliser les résultats expérimentaux au-delà des conditions testées.

### 3. Hiérarchie utilisée pendant l’audit

Pour la Draft v0.0, la hiérarchie suivante est appliquée :

Le payload API ancre ce que NeoMundi expose actuellement de manière opérationnelle.

Le Metric Contract définit ce que signifient les mesures et signaux exposés.

Les rapports exploratoires externes soutiennent ou questionnent l’interprétation, mais ne définissent pas le schéma API.

Le Runtime Interoperability Contract définira le transport, les champs requis, les types, la négociation de version et les contraintes d’intégration.

Lorsqu’une règle sémantique n’est pas suffisamment établie ou stable, elle ne doit pas devenir un DOIT.

Lorsqu’une règle concerne le transport ou la structure d’intégration plutôt que la signification de la mesure, elle doit normalement être différée vers le Runtime Interoperability Contract.

### 4. Décisions utilisées dans l’audit

Les catégories suivantes sont utilisées :

- GARDER — conserver la force normative.

- RENFORCER — augmenter la force normative car la formulation définit une frontière sémantique fondamentale.

- AFFAIBLIR — réduire la force normative car la règle n’est pas suffisamment mûre pour être obligatoire.

- DESCRIPTIF — retirer le langage normatif et conserver une explication.

- À DIFFÉRER — renvoyer l’exigence au Runtime Interoperability Contract ou à une future spécification.

### 5. Audit section par section

#### Langage normatif

**Décision : GARDER, avec une précision.**

Conserver les définitions de DOIT / NE DOIT PAS / DEVRAIT / NE DEVRAIT PAS / PEUT / OPTIONNEL.

Le périmètre normatif doit rester limité à la signification, l’interprétation et les frontières sémantiques de la couche de mesure.

Les noms de champs, leur emplacement JSON exact, les enums et les contraintes de transport ne doivent pas devenir normatifs uniquement parce qu’ils apparaissent dans le payload actuel.

> Une modification qui change matériellement la signification d’une mesure **DOIT** faire l’objet d’un versionnement explicite.

#### Section 1 — Objet

**Décision : RENFORCER la frontière d’autorité.**

La distinction entre mesure et décision externe est fondamentale.

**Règle normative recommandée :**

Le Metric Contract NE DOIT PAS définir une décision de politique, une autorisation d’exécution ou une action opérationnelle comme signification intrinsèque d’une mesure NeoMundi.

Le principe selon lequel le système consommateur conserve l’autorité de décision et d’action doit rester central.

#### Section 2 — Objet runtime de référence

**Décision : GARDER la structure comme descriptive ; GARDER une frontière forte.**

La structure actuelle du payload API doit être documentée comme référence opérationnelle.

En revanche, l’emplacement exact des champs et la structure machine ne doivent pas encore être figés par le Metric Contract.

**Règle normative recommandée :**

Le Metric Contract NE DOIT PAS introduire une représentation machine concurrente qui modifierait la signification des mesures effectivement exposées par NeoMundi.

Les exigences exactes de sérialisation doivent être différées vers le Runtime Interoperability Contract.

#### Section 3 — Observation

**Décision : GARDER.**

**Règles recommandées :**

> Une mesure NeoMundi **DOIT** rester attribuable à l’observation à laquelle elle appartient.

> Une observation synthétique **NE **DOIT** PAS** être présentée comme une observation de production.

Ces règles concernent la traçabilité et l’intégrité sémantique, pas le format de transport.

#### Section 4 — Version du payload

**Décision : MODIFIER.**

schema_version décrit la représentation machine du payload.

Le Metric Contract doit expliquer ce fait, mais il ne doit pas définir toutes les futures règles de versionnement du schéma.

**Règles recommandées :**

> Une modification qui altère matériellement la signification sémantique d’une mesure exposée **DOIT** être explicitement distinguable par versionnement.

Les règles exactes de compatibilité de schéma et de négociation de version doivent être DIFFÉRÉES vers le Runtime Interoperability Contract.

Éviter un DOIT pour les changements purement structurels qui ne modifient pas la sémantique.

#### Section 5 — Source de l’observation

**Décision : GARDER.**

**Règle forte recommandée :**

> Une information qui n’est pas explicitement présente ou déclarée dans le contexte d’observation **NE **DOIT** PAS** être silencieusement déduite comme mesurée ou connue.

En particulier, un alias de modèle ne doit pas être considéré comme une preuve de l’identité du provider, de la version exacte du modèle ou de la configuration d’exécution.

Il s’agit d’une règle d’intégrité sémantique.

#### Section 6 — Bloc de mesure

**Décision : GARDER, sans rendre la structure du conteneur normative.**

Le bloc measurement constitue le conteneur actuellement observé pour les sorties de mesure.

Son emplacement JSON exact relève de l’interopérabilité.

**Règle sémantique recommandée :**

> Une mesure ou un signal **NE **DOIT** PAS** être interprété indépendamment des limitations et frontières de mesure applicables lorsque celles-ci affectent matériellement sa signification.

#### Section 7 — Statut de mesure

**Décision : GARDER.**

**Règles recommandées :**

> partial **NE **DOIT** PAS** être interprété comme équivalent à complete.

> La présence d’une valeur numérique **NE **DOIT** PAS**, à elle seule, impliquer une couverture complète de mesure.

Ces règles protègent contre la surinterprétation d’observations incomplètes.

#### Section 8 — Couverture de mesure

**Décision : GARDER, avec une formulation centrée sur la signification plutôt que sur la présence d’un champ.**

Règle recommandée :

> Une couverture incomplète connue **DOIT** rester distinguable d’une couverture complète lors de toute interprétation de la mesure.

Ne pas imposer ici le nom exact du champ ou sa sérialisation.

Ces exigences relèvent de l’interopérabilité.

#### Section 9 — Stability score

**Décision : GARDER et RENFORCER.**

Il s’agit de l’une des frontières normatives les plus fortes du contrat.

**Règles recommandées :**

> stability_score **NE **DOIT** PAS** être interprété comme une mesure de vérité factuelle.

> stability_score **NE **DOIT** PAS**, à lui seul, établir la sécurité, la conformité, l’admissibilité ou une autorisation d’exécution.

Une valeur élevée de stabilité PEUT coexister avec une sortie factuellement incorrecte.

**Le Metric Contract doit préserver le principe :**

> **Stabilité ≠ vérité.**

#### Section 10 — Coherence score

**Décision : GARDER.**

Règle recommandée :

> coherence_score **NE **DOIT** PAS**, à lui seul, être interprété comme une validation factuelle, une validation de sécurité, une détermination de conformité ou une autorisation d’exécution.

Le score peut être consommé avec d’autres signaux, mais le Metric Contract ne doit pas imposer la politique du système consommateur.

#### Section 11 — Signal de validité factuelle

**Décision : GARDER, avec prudence.**

**Règles recommandées :**

> Un signal factuel runtime fondé uniquement sur des éléments runtime **NE **DOIT** PAS** être présenté comme une vérification indépendante de la vérité.

> Un score factuel null ou not_assessed **NE **DOIT** PAS** être interprété comme un risque factuel nul ou comme une validation factuelle.

Les labels de statut observés dans les payloads actuels doivent rester descriptifs tant que leur enum n’est pas formellement figé.

La taxonomie exacte doit être différée au travail d’interopérabilité ou de schéma, sauf si elle fait l’objet d’un versionnement sémantique séparé.

#### Section 12 — Signal de variabilité sémantique

**Décision : GARDER.**

**Règles recommandées :**

> Une faible variabilité sémantique observée **NE **DOIT** PAS** être interprétée comme une preuve de correction factuelle.

> Une variabilité sémantique élevée **NE **DOIT** PAS**, à elle seule, identifier la cause de la variation.

> Une variabilité élevée **NE **DOIT** PAS**, à elle seule, identifier quelle sortie est correcte.

Ce sont des frontières d’interprétation, pas des décisions de politique.

#### Section 13 — Bandes de latence et de coût

**Décision : AFFAIBLIR / principalement DESCRIPTIF.**

L’existence de bandes de latence et de coût peut être documentée.

Le Metric Contract ne doit cependant pas encore figer :

- les enums autorisés exacts ;

- les seuils définissant low, medium ou d’autres bandes ;

les conséquences de routage.

Règle recommandée :

Une valeur de latence ou de coût explicitement inconnue NE DOIT PAS être silencieusement interprétée comme une valeur mesurée.

Le reste doit rester descriptif ou être différé.

#### Section 14 — Risk signal

**Décision : GARDER et RENFORCER la frontière de décision.**

**Règles recommandées :**

Un risk_signal NeoMundi NE DOIT PAS signifier automatiquement ALLOW, BLOCK, ACCEPT, REJECT, COMPLIANT, NON-COMPLIANT, SAFE, UNSAFE, ADMISSIBLE ou NON-ADMISSIBLE.

> Un système consommateur **PEUT** utiliser ce signal comme entrée de sa propre politique.

La décision de politique qui en résulte reste extérieure au Metric Contract.

Les niveaux et types de risque actuellement observés ne doivent pas automatiquement devenir des enums figés dans la Draft v0.0.

#### Section 15 — Limitations connues

**Décision : RENFORCER la sémantique sans imposer les chaînes de caractères littérales.**

La formulation actuelle indiquant que les limitations « NE DEVRAIENT PAS être supprimées » est trop liée au transport.

**Meilleure règle normative :**

> Toute limitation affectant matériellement l’interprétation d’une mesure **DOIT** rester sémantiquement associée à cette mesure lorsqu’elle est interprétée ou transmise pour soutenir une décision.

La formulation littérale, l’emplacement de stockage et la sérialisation des limitations doivent être différés vers l’interopérabilité.

#### Section 16 — Frontière de mesure

**Décision : GARDER et RENFORCER.**

Il s’agit d’une section normative centrale.

Règle recommandée :

> Une mesure NeoMundi **NE **DOIT** PAS**, à elle seule, être interprétée comme établissant la vérité, la sécurité, une autorité, une permission downstream ou l’admissibilité d’une exécution, sauf si une future métrique explicitement versionnée le définit autrement.

Cette frontière protège la séparation entre mesure et décision.

#### Section 17 — Séparation entre mesure et décision

**Décision : GARDER et RENFORCER.**

**Règles recommandées :**

> NeoMundi **DOIT** rester autoritatif sur la sémantique déclarée de ses mesures.

> Le système consommateur **DOIT** rester responsable de la politique ou de l’action opérationnelle qu’il dérive de ces mesures.

Les systèmes externes PEUVENT utiliser les mesures NeoMundi pour la journalisation, la vérification, la régénération, le reroutage, l’escalade, la supervision humaine, l’interruption, la conformité ou les workflows d’evidence.

Les exemples d’actions sont non exhaustifs et non normatifs.

#### Section 18 — Neutralité vis-à-vis de l’infrastructure

**Décision : GARDER.**

**Règles recommandées :**

Le Metric Contract NE DOIT PAS imposer un usage downstream comme interprétation unique d’une mesure.

> Une mesure **PEUT** être consommée par plusieurs infrastructures et pour plusieurs usages.

La logique de politique propre à l’infrastructure doit rester hors du Metric Contract.

#### Section 19 — Valeurs inconnues, nulles ou non évaluées

**Décision : GARDER et RENFORCER.**

**Règles recommandées :**

null NE DOIT PAS être interprété comme zéro.

unknown NE DOIT PAS être interprété comme faible, élevé ou comme une catégorie mesurée.

not_assessed NE DOIT PAS être interprété comme une absence de risque ou une validation réussie.

La distinction entre valeur indisponible, inconnue, non évaluée et mesurée DOIT être préservée sémantiquement.

Les règles d’encodage exactes relèvent de l’interopérabilité.

#### Section 20 — Mesure partielle

**Décision : GARDER.**

**Règles recommandées :**

Une observation partielle NE DOIT PAS être présentée comme complète.

Une valeur disponible dans une observation partielle NE DOIT PAS impliquer que toutes les dimensions de mesure ont été évaluées.

Les systèmes consommateurs DEVRAIENT tenir compte de la couverture déclarée lors de l’interprétation d’observations partielles.

#### Section 21 — Traçabilité

**Décision : AFFAIBLIR et DIFFÉRER les exigences structurelles.**

La présence actuelle de :

- observation_id ;

- source_batch_id ;

- trace_id ;

doit être documentée.

Le Metric Contract ne doit cependant pas encore imposer la structure exacte du payload de traçabilité.

**Règle sémantique recommandée :**

> Une mesure **DEVRAIT** rester attribuable à son observation d’origine.

Les exigences relatives aux hashes, signatures, reçus, identifiants obligatoires et mécanismes cryptographiques d’intégrité doivent être différées vers le Runtime Interoperability Contract ou les spécifications de la couche d’evidence.

#### Section 22 — Confidentialité et données non exposées

**Décision : DESCRIPTIF / À DIFFÉRER.**

Les payloads synthétiques actuels indiquent qu’ils n’exposent pas les prompts bruts, sorties brutes de modèle, identité du fournisseur, données client ou schéma propriétaire.

Cette observation ne doit pas automatiquement devenir une interdiction universelle du Metric Contract.

Les exigences de confidentialité et d’exposition des données doivent être spécifiées séparément ou dans la documentation d’interopérabilité et de sécurité.

**Formulation recommandée :**

Le Metric Contract décrit uniquement les données et mesures exposées par l’interface de mesure et n’implique pas un accès aux données internes de calcul.

Ne pas introduire de règle NE DOIT PAS exposer le prompt brut dans ce contrat tant que NeoMundi n’a pas explicitement décidé d’en faire un engagement produit permanent.

#### Section 23 — Frontière d’interopérabilité

**Décision : GARDER.**

**Règles recommandées :**

> Le Metric Contract **DOIT** définir la sémantique de la mesure.

> Le Runtime Interoperability Contract **DEVRAIT** définir le transport, les champs requis, les types, la compatibilité et la négociation de version.

Le Metric Contract NE DOIT PAS figer des détails de transport qui n’ont pas encore été validés dans le cadre de l’interopérabilité.

Cette section doit maintenir la séparation entre contrat sémantique et contrat de transport.

#### Section 24 — Règle de versionnement sémantique

**Décision : GARDER, avec une formulation plus forte.**

**Règles recommandées :**

> Une modification matérielle de la signification d’une mesure ou d’un signal **DOIT** être explicitement versionnée.

Les observations historiques DEVRAIENT rester interprétables selon la version sémantique sous laquelle elles ont été produites.

Les changements de sérialisation qui ne modifient pas la signification PEUVENT relever du versionnement du schéma ou de l’interopérabilité.

#### Section 25 — Payload de référence

**Décision : GARDER comme exemple NON NORMATIF.**

Le payload de référence est essentiel car il ancre le contrat sur la représentation réelle de l’API.

Cependant :

- les valeurs numériques d’exemple ne sont pas normatives ;

- les identifiants d’exemple ne sont pas normatifs ;

- les labels de statut et enums observés ne sont pas automatiquement figés ;

les exigences exactes de champs restent soumises à la formalisation de l’interopérabilité.

**Le payload doit être explicitement présenté comme :**

Exemple de référence opérationnelle — ne constitue pas un JSON Schema normatif complet.

#### Section 26 — Principe fondamental

**Décision : GARDER.**

Cette section doit rester principalement déclarative et ne pas être surchargée de termes normatifs.

**Le principe central est :**

NeoMundi fournit des mesures et signaux contextualisés portant sur le comportement observable des systèmes d’IA dans des frontières runtime déclarées. Ces mesures peuvent être consommées par plusieurs systèmes et plusieurs infrastructures sans transférer à NeoMundi l’autorité de décision downstream.

Il s’agit du centre sémantique du Metric Contract.

### 6. Noyau normatif issu de l’audit

Après revue, les exigences DOIT / NE DOIT PAS les plus fortes devraient se concentrer sur un nombre limité de frontières durables :

> Une mesure **DOIT** rester attribuable à son observation.

> Une observation synthétique **NE **DOIT** PAS** être présentée comme une observation de production.

> Une information non déclarée **NE **DOIT** PAS** être silencieusement déduite comme mesurée ou connue.

> Une mesure partielle **NE **DOIT** PAS** être présentée comme complète.

La stabilité NE DOIT PAS être interprétée comme la vérité factuelle.

La cohérence NE DOIT PAS, à elle seule, être interprétée comme une validation factuelle.

> Un signal factuel runtime **NE **DOIT** PAS** être présenté comme une vérification indépendante de la vérité lorsqu’il ne repose pas sur une validation indépendante.

La variabilité sémantique NE DOIT PAS être traitée comme preuve de correction factuelle ou comme identification de la sortie correcte.

Les signaux de risque NE DOIVENT PAS devenir automatiquement des décisions de politique ou d’exécution.

Les limitations matérielles de mesure DOIVENT rester sémantiquement associées à l’interprétation.

> Une mesure NeoMundi **NE **DOIT** PAS**, à elle seule, établir la vérité, la sécurité, une autorité, une permission downstream ou l’admissibilité d’une exécution.

Les états inconnus, nuls ou non évalués NE DOIVENT PAS être silencieusement transformés en valeurs mesurées.

> Une modification sémantique matérielle **DOIT** être versionnée.

Le Metric Contract NE DOIT PAS figer des détails de transport ou d’interopérabilité non validés.

Le système consommateur conserve l’autorité sur les décisions et actions qu’il dérive des mesures NeoMundi.

Ce noyau normatif volontairement réduit est plus solide qu’un contrat contenant de nombreux DOIT faibles ou prématurés.

### 7. Éléments explicitement différés

Les éléments suivants ne doivent pas être figés par le Metric Contract Draft v0.0 tant qu’ils n’ont pas été validés séparément :

- JSON Schema définitif ;

- liste définitive des champs obligatoires ;

- noms définitifs des champs ;

- enums définitifs ;

- protocole de transport ;

- négociation de version ;

- reçus cryptographiques ;

- signatures et hashes ;

- packaging d’evidence ;

- règles de divulgation du provider ;

- valeurs exactes de seuils ;

- frontières exactes des bandes ;

- règles automatiques de routage ;

- règles automatiques d’arrêt ;

- détermination de conformité ;

- détermination d’admissibilité ;

autorisation d’exécution.

Ces éléments relèvent du Runtime Interoperability Contract, des spécifications d’evidence, de la configuration produit ou de la politique du système consommateur.

### 8. Étape suivante

Une fois cet audit validé :

- réécrire metric-contract-v0.0.fr.md avec le noyau normatif audité ;

- supprimer ou affaiblir les formulations normatives prématurées ;

- conserver le payload API réel comme référence opérationnelle ;

- générer la version anglaise comme miroir sémantique strict ;

- effectuer une revue finale de cohérence FR/EN ;

seulement ensuite préparer la version suivante ou le travail de schéma machine-readable.

# NeoMundi Metric Contract — Normative Audit v0.0
*Working document — Non-normative*
