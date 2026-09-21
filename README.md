# NeoMundi Metric Contract

**Version :** 0.0  
**Statut :** Draft  
**Mainteneur :** NeoMundi  
**Périmètre :** mesure runtime du comportement observable des systèmes d’IA

[🇬🇧 English](./README.md) · [🇫🇷 Français](./README_FR.md)

## Comprendre ce que signifient les mesures NeoMundi — et ce qu’elles ne prouvent pas

Le NeoMundi Metric Contract définit la frontière sémantique des mesures et
signaux exposés par la NeoMundi Runtime Measurement Layer.

Il fournit le langage commun nécessaire pour :

- interpréter les signaux de mesure de manière cohérente ;
- distinguer les valeurs mesurées, partielles, inconnues et non évaluées ;
- préserver leur signification entre les versions et les infrastructures ;
- consommer les mesures dans des systèmes d’audit, d’observabilité, de
  gouvernance, d’assurance et d’aide à la décision ;
- maintenir une séparation entre mesure, politique, autorisation et exécution.

> **NeoMundi définit la sémantique de mesure. Le système consommateur conserve
> l’autorité d’interprétation, de politique et de décision.**

### Commencer

| Besoin | Référence |
|---|---|
| Comprendre le cadre de mesure | [Measurement Reference Framework — EN](./measurement_reference_framework_en.md) |
| Appliquer les règles d’interprétation et de consommation | [FR](./signal_interpretation_and_consumption_rules.fr.md) · [EN](./signal_interpretation_and_consumption_rules.en.md) |
| Lire le Metric Contract complet | [FR](./metric-contract-v0.0.fr.md) · [EN](./metric-contract-v0.0.en.md) |
| Produire des mesures NeoMundi | [Runtime Measurement Layer](https://github.com/neomundi-io/neomundi-runtime-measurement) |
| Échanger les mesures entre systèmes | [Measurement Interoperability](https://github.com/neomundi-io/neomundi-measurement-interoperability) |
| Créer un compte et une clé API | [Plateforme NeoMundi](https://controltower.neomundi.io/welcome) |

---

## Objet

Le **NeoMundi Metric Contract** définit la frontière sémantique des mesures et
signaux exposés par la NeoMundi Runtime Measurement Layer.

Cette **version Draft v0.0** est ancrée dans la structure du payload
d’observation actuellement exposé par l’API NeoMundi :

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

Le Metric Contract explique la **signification, l’interprétation et les
limites** des mesures et signaux contenus dans ce payload.

Il ne définit **ni la décision de politique, ni l’autorisation d’exécution, ni
l’action opérationnelle** qu’un système externe peut dériver de ces mesures.

> **NeoMundi mesure. Le système consommateur conserve l’autorité de décision et
> d’action.**

## Metric Contract v0.0

→ [Lire la version française complète](./metric-contract-v0.0.fr.md)

## Ce que définit le contrat

Le contrat définit actuellement l’interprétation sémantique de :

- l’objet d’observation NeoMundi ;
- le versionnement du payload ;
- le contexte source ;
- le statut et la couverture de mesure ;
- `stability_score` ;
- `coherence_score` ;
- `factual_validity_signal` ;
- `semantic_variability_signal` ;
- les bandes de latence et de coût ;
- `risk_signal` ;
- les limitations connues ;
- la frontière de mesure ;
- les identifiants de traçabilité ;
- les valeurs inconnues, nulles ou non évaluées ;
- les mesures partielles ;
- la séparation entre mesure et décision ;
- la neutralité vis-à-vis de l’infrastructure ;
- la frontière avec l’interopérabilité des mesures.

## Source de vérité

Le Metric Contract n’introduit pas de représentation machine parallèle.

Ses définitions sémantiques sont ancrées dans la structure effectivement exposée
par l’API NeoMundi.

Les exemples de référence actuels sont explicitement identifiés comme
synthétiques (`synthetic: true`) et servent à documenter la structure du payload
et la sémantique des mesures.

Ils ne sont **pas** présentés comme des observations de production.

## Metric Contract et Measurement Interoperability

Le **Metric Contract** définit ce que signifient les mesures et signaux NeoMundi.

Le contrat **Measurement Interoperability** définit comment les enregistrements
de mesure sont structurés, versionnés, échangés et consommés entre des systèmes
indépendants, notamment leur provenance, leur intégrité et les frontières de
responsabilité.

Les deux contrats portent donc sur des couches distinctes :

**sémantique de mesure → interopérabilité → décision ou action externe**

## Statut Draft

La version **0.0** constitue une première spécification sémantique.

La représentation exploitable par machine pourra évoluer avec la formalisation
du contrat NeoMundi Measurement Interoperability.

Toute évolution modifiant matériellement la signification d’une mesure ou d’un
signal devra rester explicitement versionnée.

---

## Structure du dépôt

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

Des schémas lisibles par machine et des artefacts d’interopérabilité
supplémentaires pourront être introduits à mesure que la spécification mûrit.

---

**NeoMundi Metric Contract — Draft v0.0**

*Mesuré par NeoMundi. Utilisé selon l’autorité du système consommateur.*
