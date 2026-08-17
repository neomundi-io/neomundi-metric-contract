# NeoMundi Metric Contract

**Version:** 0.0
**Status:** Draft
**Maintainer:** NeoMundi
**Scope:** Runtime measurement of observable AI-system behaviour

[🇬🇧 English](#english) · [🇫🇷 Français](#francais)

---

🇫🇷 Français · 🇬🇧 English

**Cadre de référence de la mesure :**  
[measurement_reference_framework_fr.md](./measurement_reference_framework_fr.md)

---

<a id="english"></a>

## 🇬🇧 English

The **NeoMundi Metric Contract** defines the semantic boundary of the measurements and signals exposed by the NeoMundi runtime measurement layer.

This **Draft v0.0** is anchored in the structure of the observation payload currently exposed by the NeoMundi API:

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

The Metric Contract explains the **meaning, interpretation and limits** of the measurements and signals contained in that payload.

It does **not** define the policy decision, execution authorization or operational action that an external system may derive from those measurements.

> **NeoMundi measures. The consuming system retains authority over decisions and actions.**

### Metric Contract v0.0

→ [Read the English version](./metric-contract-v0.0.en.md)

### What the contract defines

The contract currently defines the semantic interpretation of:

* the NeoMundi observation object;
* payload versioning;
* source context;
* measurement status and coverage;
* `stability_score`;
* `coherence_score`;
* `factual_validity_signal`;
* `semantic_variability_signal`;
* latency and cost bands;
* `risk_signal`;
* known limitations;
* the measurement boundary;
* traceability identifiers;
* unknown, null and non-assessed values;
* partial measurements;
* the separation between measurement and decision;
* infrastructure neutrality;
* the boundary with runtime interoperability.

### Source of truth

The Metric Contract does not introduce a parallel machine representation.

Its semantic definitions are anchored in the structure actually exposed by the NeoMundi API.

The current reference examples are explicitly marked as synthetic (`synthetic: true`) and are used to document payload structure and measurement semantics.

They are **not** presented as production observations.

### Metric Contract vs Runtime Interoperability Contract

The **Metric Contract** defines what NeoMundi measurements and signals mean.

The **Runtime Interoperability Contract** defines, or will define, how those objects are exchanged and consumed between systems, including contractual structure, field requirements, types and version negotiation.

The two contracts therefore address different layers:

**measurement semantics → interoperability → external decision or action**

### Draft status

Version **0.0** is an initial semantic specification.

The machine-readable representation may evolve as the **NeoMundi Runtime Interoperability Contract** is formalized.

Any evolution that materially changes the meaning of a measurement or signal must remain explicitly versioned.

---

<a id="francais"></a>

## 🇫🇷 Français

Le **NeoMundi Metric Contract** définit la frontière sémantique des mesures et signaux exposés par la couche de mesure runtime NeoMundi.

Cette **Draft v0.0** est ancrée sur la structure du payload d’observation actuellement exposé par l’API NeoMundi :

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

Le Metric Contract explique la **signification, l’interprétation et les limites** des mesures et signaux contenus dans ce payload.

Il ne définit **ni la décision de politique, ni l’autorisation d’exécution, ni l’action opérationnelle** qu’un système externe peut dériver de ces mesures.

> **NeoMundi mesure. Le système consommateur conserve l’autorité de décision et d’action.**

### Metric Contract v0.0

→ [Lire la version française](./metric-contract-v0.0.fr.md)

### Ce que définit le contrat

Le contrat définit actuellement l’interprétation sémantique de :

* l’objet d’observation NeoMundi ;
* le versionnement du payload ;
* le contexte source ;
* le statut et la couverture de mesure ;
* `stability_score` ;
* `coherence_score` ;
* `factual_validity_signal` ;
* `semantic_variability_signal` ;
* les bandes de latence et de coût ;
* `risk_signal` ;
* les limitations connues ;
* la frontière de mesure ;
* les identifiants de traçabilité ;
* les valeurs inconnues, nulles ou non évaluées ;
* les mesures partielles ;
* la séparation entre mesure et décision ;
* la neutralité vis-à-vis de l’infrastructure ;
* la frontière avec l’interopérabilité runtime.

### Source de vérité

Le Metric Contract n’introduit pas de représentation machine parallèle.

Ses définitions sémantiques sont ancrées sur la structure effectivement exposée par l’API NeoMundi.

Les exemples de référence actuels sont explicitement identifiés comme synthétiques (`synthetic: true`) et servent à documenter la structure du payload et la sémantique des mesures.

Ils ne sont **pas** présentés comme des observations de production.

### Metric Contract vs Runtime Interoperability Contract

Le **Metric Contract** définit ce que signifient les mesures et signaux NeoMundi.

Le **Runtime Interoperability Contract** définit, ou définira, comment ces objets sont échangés et consommés entre systèmes, notamment la structure contractuelle, les champs requis, les types et la négociation de version.

Les deux contrats portent donc sur des couches distinctes :

**sémantique de mesure → interopérabilité → décision ou action externe**

### Statut Draft

La version **0.0** constitue une première spécification sémantique.

La représentation exploitable par machine pourra évoluer avec la formalisation du **NeoMundi Runtime Interoperability Contract**.

Toute évolution modifiant matériellement la signification d’une mesure ou d’un signal devra rester explicitement versionnée.

---

## Repository

```text
neomundi-metric-contract/
├── README.md
├── metric-contract-v0.0.en.md
└── metric-contract-v0.0.fr.md
```

Additional machine-readable schemas and interoperability artifacts may be introduced as the specification matures.

---

**NeoMundi Metric Contract — Draft v0.0**

*Measured by NeoMundi. Used according to the authority of the consuming system.*
