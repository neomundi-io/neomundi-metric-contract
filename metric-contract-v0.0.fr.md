[🇬🇧 English version](./metric-contract-v0.0.en.md) · [← Repository](./README.md)

> **Note de statut — v0.0**
>
> Ce document définit les frontières sémantiques actuelles de la couche de mesure NeoMundi.
>
> Les structures JSON et les identifiants présentés dans cette version sont des représentations illustratives des concepts métriques. Ils ne constituent pas encore le schéma définitif du NeoMundi Runtime Interoperability Contract.
>
> La représentation exploitable par machine pourra évoluer avec la formalisation du Runtime Interoperability Contract, sans modifier la sémantique sous-jacente de la mesure sauf versionnement explicite.

# NeoMundi Metric Contract

**Version :** 0.0
**Statut :** Draft
**Date :** 12 août 2026
**Mainteneur :** NeoMundi
**Périmètre :** Mesure runtime des systèmes d’IA

[🇬🇧 English version](./metric-contract-v0.0.en.md) · [← Repository](./README.md)

---

## 1. Objet

Le NeoMundi Metric Contract définit la signification, les conditions, les limites et la sortie attendue d’une mesure runtime NeoMundi.

Son objectif est de garantir qu’une mesure puisse être :

* identifiée ;
* interprétée de manière cohérente ;
* reproduite dans des conditions déclarées ;
* transportée entre différentes infrastructures ;
* comparée dans le temps ;
* référencée de manière indépendante ;
* utilisée par des systèmes externes de gouvernance, de supervision, de conformité ou de preuve.

Le Metric Contract définit une **mesure**.

Il ne définit ni la décision de politique ni l’action opérationnelle pouvant résulter de cette mesure.

> **NeoMundi mesure. Les systèmes externes décident quoi faire de la mesure.**

---

## 2. Identité de la métrique

Chaque métrique DOIT exposer une identité stable.

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

La combinaison :

`metric id + metric version`

identifie de manière unique la définition de mesure appliquée à une observation.

Toute modification affectant la signification ou le calcul de la métrique DOIT entraîner une nouvelle version.

---

## 3. Objet de la mesure

La métrique mesure une propriété observable du comportement d’un système d’IA dans des conditions runtime déclarées.

Exemple :

```json
{
  "object": {
    "entity_type": "ai_system",
    "property": "behavioural_stability",
    "observation_mode": "repeated_runtime_execution"
  }
}
```

La métrique ne prétend pas mesurer l’état interne, les poids, l’architecture ou les intentions du système d’IA, sauf mention explicite.

Elle mesure un **comportement observable en conditions runtime**.

---

## 4. Conditions de mesure

Toute mesure DOIT être associée à un contexte suffisant pour comprendre dans quelles conditions elle a été produite.

Contexte minimal :

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

Des paramètres supplémentaires PEUVENT notamment inclure :

* température ;
* seed ;
* system prompt ;
* identifiant d’entrée ;
* nombre de répétitions ;
* zone géographique d’exécution ;
* configuration API ;
* disponibilité des outils ;
* environnement d’exécution.

Les paramètres inconnus NE DOIVENT PAS être déduits silencieusement.

Ils DEVRAIENT être explicitement représentés comme inconnus, indisponibles ou non déclarés.

---

## 5. Protocole de mesure

Le contrat DOIT identifier le protocole utilisé pour produire la mesure.

Exemple :

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

La spécification du protocole PEUT exister sous la forme d’un document versionné distinct.

Le Metric Contract DOIT référencer la version du protocole utilisée.

---

## 6. Valeur de mesure

Une mesure NeoMundi DOIT exposer une valeur exploitable par machine.

Exemple :

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

La valeur seule n’est pas suffisante.

Son interprétation dépend de la définition de la métrique, du protocole, de sa version et des conditions de mesure.

---

## 7. Sémantique de la mesure

Chaque métrique DOIT définir explicitement ce que signifient l’augmentation ou la diminution de sa valeur.

Exemple pour la stabilité runtime :

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

Pour cette métrique :

**la stabilité n’implique pas la vérité.**

Un système hautement stable peut répéter systématiquement une réponse incorrecte.

Un système variable peut produire des réponses correctes.

La métrique NE DOIT donc PAS être interprétée en dehors de son domaine de mesure déclaré.

---

## 8. États métrologiques

NeoMundi PEUT associer les valeurs de mesure à des états métrologiques versionnés.

Exemple :

```json
{
  "state": {
    "code": "VARIATION",
    "taxonomy_version": "0.0"
  }
}
```

Selon la métrique, les états possibles peuvent notamment inclure :

* `NORMAL`
* `VARIATION`
* `FACTUAL_ALERT`
* `INCOMPLETE`
* `UNMEASURABLE`

Ces états décrivent une **condition métrologique observée**.

Ils ne constituent pas des décisions d’autorisation.

Ils NE DOIVENT PAS être interprétés directement comme :

* `ALLOW`
* `BLOCK`
* `ACCEPT`
* `REJECT`
* `COMPLIANT`
* `NON-COMPLIANT`

Ces décisions appartiennent au système consommateur ou à l’autorité désignée.

---

## 9. Limite des seuils

NeoMundi PEUT utiliser des seuils définis en interne et versionnés afin d’identifier certains moments métrologiques particuliers.

Lorsqu’ils sont exposés, le contrat DOIT identifier la version des seuils.

```json
{
  "threshold_reference": {
    "version": "neomundi.thresholds.0.0",
    "state": "VARIATION"
  }
}
```

Les seuils caractérisent des états de mesure.

Ils ne constituent ni des règles métier, ni des décisions de politique, ni des autorisations d’exécution.

Un système externe PEUT décider indépendamment de :

* poursuivre ;
* journaliser ;
* demander une vérification ;
* rerouter ;
* escalader ;
* régénérer ;
* demander une supervision humaine ;
* interrompre l’exécution.

Ces actions restent en dehors du Metric Contract.

---

## 10. Couverture de la mesure

Chaque observation DEVRAIT exposer la couverture effective de mesure.

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

Une mesure NE DOIT PAS présenter silencieusement une couverture incomplète comme une mesure complète.

---

## 11. Limites de la métrique

Chaque métrique DOIT déclarer explicitement son périmètre de mesure et ses limites.

Exemple :

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

Des limitations supplémentaires spécifiques à chaque métrique PEUVENT être déclarées.

---

## 12. Traçabilité

Chaque mesure produite DEVRAIT pouvoir être reliée à un événement d’observation unique.

Identifiants minimaux :

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

Les implémentations PEUVENT également fournir :

* hash du payload ;
* hash des entrées ;
* hash des sorties ;
* reçus d’exécution ;
* éléments de preuve signés ;
* mécanismes cryptographiques d’intégrité.

Ces mécanismes appartiennent aux couches de traçabilité et de preuve et ne modifient pas la signification de la métrique sous-jacente.

---

## 13. Frontière d’interopérabilité

Le Metric Contract définit la sémantique de la mesure.

Un **NeoMundi Runtime Interoperability Contract** distinct définit la manière dont les observations et les mesures sont échangées entre systèmes.

L’architecture attendue est donc :

**Exécution IA**
**→ observation NeoMundi**
**→ mesure NeoMundi**
**→ événement runtime interopérable**
**→ système externe de gouvernance / monitoring / conformité / preuve**
**→ décision ou action externe**

L’infrastructure réceptrice n’a pas besoin de reproduire le moteur interne de mesure NeoMundi pour consommer la mesure.

---

## 14. Neutralité vis-à-vis de l’infrastructure

Une métrique NeoMundi DEVRAIT rester indépendante de l’infrastructure qui la consomme.

La même mesure runtime PEUT ainsi alimenter plusieurs usages, notamment :

* contrôle continu ;
* monitoring IA ;
* gouvernance runtime ;
* supervision du risque ;
* preuve de conformité ;
* surveillance des SLA ;
* routage de modèles ;
* audit ;
* reconstruction d’incident ;
* recherche.

Le Metric Contract NE DOIT PAS imposer l’un de ces usages comme interprétation unique de la mesure.

---

## 15. Règle de versionnement

Une nouvelle version de la métrique DOIT être créée lorsqu’une modification affecte matériellement :

* la propriété mesurée ;
* la méthode de calcul ;
* l’échelle ;
* l’interprétation sémantique ;
* le protocole de mesure ;
* les seuils applicables ;
* le périmètre de mesure.

Les observations historiques DOIVENT conserver la version de métrique sous laquelle elles ont été produites.

---

## 16. Représentation minimale exploitable par machine

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

## 17. Principe fondamental

**Une métrique NeoMundi est un énoncé versionné portant sur une propriété observée d’un système d’IA dans des conditions runtime déclarées.**

Elle fournit un fait mesurable commun pouvant être consommé par différents systèmes et différentes infrastructures, sans transférer l’autorité de décision à NeoMundi.

**Mesuré par NeoMundi.**
**Utilisé selon l’autorité du système consommateur.**

---

**NeoMundi Metric Contract — Draft v0.0**
