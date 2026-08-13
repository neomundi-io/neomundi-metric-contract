🇬🇧 English version · ← Repository

    Note de statut — v0.0

    Ce document définit les frontières sémantiques actuelles de la couche de mesure NeoMundi à partir de la structure des payloads d’observation effectivement exposés par l’API NeoMundi.

    La structure de référence utilisée dans cette Draft est identifiée dans les payloads par :

    schema_version: "neomundi_observation_payload_v0.1"

    Les exemples utilisés pour établir cette version du contrat sont des observations synthétiques explicitement identifiées par synthetic: true. Ils servent à documenter la structure et la sémantique du payload et ne constituent pas des observations de production.

    Le Metric Contract définit la signification et les frontières des mesures et signaux présents dans ce payload.

    Il ne constitue pas lui-même le schéma d’interopérabilité définitif. Les règles de transport, d’intégration et d’échange entre systèmes relèvent du NeoMundi Runtime Interoperability Contract.

NeoMundi Metric Contract

Version : 0.0
Statut : Draft
Date : 13 août 2026
Mainteneur : NeoMundi
Périmètre : Mesure runtime du comportement observable des systèmes d’IA
Langage normatif

Les termes DOIT, NE DOIT PAS, DEVRAIT, NE DEVRAIT PAS, PEUT et OPTIONNEL indiquent le niveau d’exigence exprimé par cette spécification.

    DOIT / NE DOIT PAS indiquent une exigence du Metric Contract.

    DEVRAIT / NE DEVRAIT PAS indiquent une exigence recommandée à laquelle il est possible de déroger lorsqu’une justification existe.

    PEUT / OPTIONNEL indiquent un comportement autorisé mais non obligatoire.

Dans cette Draft v0.0, les exigences normatives portent sur la signification, l’interprétation et les frontières de la couche de mesure.

La présence d’un champ dans les payloads de référence ne signifie pas automatiquement que son nom, son type ou son emplacement sont figés à long terme.

Toute évolution de représentation machine qui modifierait la signification d’une mesure ou d’un signal DEVRA toutefois faire l’objet d’un versionnement explicite.
1. Objet

Le NeoMundi Metric Contract définit comment interpréter les mesures et signaux contenus dans une observation runtime NeoMundi.

Il établit notamment :

    ce qui constitue une observation NeoMundi ;

    où se trouvent les mesures associées à cette observation ;

    comment leur contexte source est déclaré ;

    comment leur couverture et leur statut sont représentés ;

    comment leurs limitations sont exprimées ;

    quelles conclusions peuvent ou ne peuvent pas être tirées des mesures ;

    où s’arrête l’autorité de la couche de mesure NeoMundi.

Le contrat porte sur la couche de mesure.

Il ne définit ni une politique métier, ni une décision d’admissibilité, ni une autorisation d’exécution, ni l’action qu’un système consommateur doit entreprendre.

    NeoMundi mesure. Le système consommateur conserve l’autorité de décision et d’action.

2. Objet runtime de référence

L’unité machine de référence exposée par l’API est un payload d’observation NeoMundi.

Sa structure actuellement observée est de la forme :

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

Le payload regroupe donc dans un même objet :

identité de l’observation → contexte source → mesures et signaux → limitations connues → frontière d’interprétation

Cette structure constitue l’ancrage opérationnel de la présente Draft.

Le Metric Contract ne crée pas une représentation parallèle de cet objet.
3. Observation

Une observation est l’objet runtime auquel les mesures NeoMundi sont rattachées.

Chaque payload observé expose notamment :

{
  "observation_id": "nm-syn-001",
  "generated_at": "2026-06-28T06:42:47Z",
  "synthetic": true
}

observation_id identifie l’observation.

generated_at indique le moment auquel le payload a été généré.

synthetic indique si l’observation représentée est synthétique.

Une mesure NeoMundi DOIT pouvoir être rattachée à l’observation dans laquelle elle a été produite ou exposée.

Une observation synthétique NE DOIT PAS être présentée comme une observation de production.
4. Version du payload

Le champ :

{
  "schema_version": "neomundi_observation_payload_v0.1"
}

identifie la version de la représentation machine du payload d’observation.

La schema_version versionne la structure du payload.

Elle ne doit pas être confondue avec la valeur d’une mesure individuelle.

Une modification incompatible de la structure ou de la signification des champs exposés DEVRA entraîner une évolution explicite de version.

Le versionnement du payload et le versionnement futur de définitions métriques plus spécifiques PEUVENT évoluer indépendamment s’ils sont explicitement distingués.
5. Source de l’observation

Le bloc source fournit le contexte permettant d’identifier l’origine déclarée de l’observation.

La structure de référence observée est :

{
  "source": {
    "source_batch_id": "nm-synthetic-demo-v0.1",
    "trace_id": "syn-trace-001",
    "model_alias": "system_alpha",
    "prompt_class": "general_explanatory"
  }
}

Dans les payloads de référence :

    source_batch_id identifie le lot ou ensemble source ;

    trace_id fournit un identifiant de trace ;

    model_alias identifie le système observé sous la forme déclarée par le payload ;

    prompt_class qualifie la classe de tâche ou de prompt observée.

Ces éléments décrivent le contexte disponible.

Ils NE DOIVENT PAS être interprétés comme fournissant des informations qui ne sont pas explicitement présentes dans le payload.

En particulier, un alias de modèle ne permet pas à lui seul de déduire silencieusement un fournisseur, une version exacte de modèle ou une configuration d’exécution.
6. Bloc de mesure

Les résultats de mesure sont exposés dans le bloc :

{
  "measurement": {}
}

Ce bloc contient les valeurs, statuts, bandes et signaux produits pour l’observation considérée.

Dans les payloads de référence, il peut notamment contenir :

{
  "measurement_status": "complete",
  "stability_score": 0.94,
  "coherence_score": 0.88,
  "factual_validity_signal": {},
  "semantic_variability_signal": {},
  "latency_band": "medium",
  "cost_band": "low",
  "risk_signal": {}
}

Lorsque la mesure est partielle, un champ measurement_coverage est également observé.

Les éléments présents dans measurement constituent des résultats de mesure ou des signaux de mesure.

Ils NE DOIVENT PAS être interprétés isolément de leurs limitations et de leur frontière de mesure.
7. Statut de mesure

Le champ :

{
  "measurement_status": "complete"
}

qualifie l’état de complétude de la mesure exposée.

Les payloads de référence montrent au moins les valeurs :

complete
partial

Lorsque measurement_status vaut partial, la mesure NE DOIT PAS être interprétée comme disposant de la même couverture qu’une mesure déclarée complete.

Une mesure partielle PEUT néanmoins contenir certaines valeurs calculées.

La présence d’une valeur numérique dans une mesure partielle NE DOIT PAS être interprétée comme une preuve de complétude de l’observation.
8. Couverture de mesure

Lorsqu’une observation est partielle, le payload de référence peut exposer :

{
  "measurement_coverage": 0.54
}

measurement_coverage qualifie la couverture effective de la mesure telle qu’exposée par le payload.

Une couverture incomplète DOIT rester distinguable d’une mesure complète lorsqu’elle est connue.

La couverture constitue une information nécessaire à l’interprétation des valeurs disponibles.

Une mesure présentant une couverture limitée NE DOIT PAS être présentée silencieusement comme une observation exhaustive.
9. Stability score

Le champ :

{
  "stability_score": 0.94
}

représente un score de stabilité observée produit par la couche de mesure NeoMundi pour l’observation considérée.

Une valeur élevée de stability_score caractérise une stabilité observée plus forte dans le périmètre de mesure appliqué.

Elle ne démontre pas à elle seule :

    la vérité factuelle ;

    la qualité générale de la réponse ;

    la sécurité du système ;

    l’absence de risque ;

    la conformité ;

    l’admissibilité d’une exécution.

En particulier :

    Une réponse peut être stable et néanmoins factuellement fragile ou incorrecte.

Le stability_score NE DOIT PAS être transformé en verdict de vérité ou d’autorisation sans une règle externe explicitement assumée par le système consommateur.
10. Coherence score

Le champ :

{
  "coherence_score": 0.88
}

représente un signal quantifié de cohérence dans le périmètre de l’observation mesurée.

Il constitue une dimension distincte du stability_score.

Un niveau de cohérence élevé NE DOIT PAS être interprété à lui seul comme une validation factuelle, une garantie de sécurité ou une autorisation d’utilisation.

Les systèmes consommateurs PEUVENT utiliser ce signal conjointement avec d’autres dimensions selon leurs propres règles.
11. Signal de validité factuelle

Le bloc de référence est :

{
  "factual_validity_signal": {
    "status": "fragile",
    "factual_hallucination_score": 0.41,
    "evidence_basis": "runtime signal only"
  }
}

Ce bloc expose un signal runtime relatif à la dimension factuelle.

Les payloads fournis montrent plusieurs statuts possibles, notamment :

fragile
uncertain
supported_with_limitations
contradiction_or_overclaim_signal
not_assessed

Le champ :

{
  "factual_hallucination_score": 0.41
}

constitue un signal quantifié associé à cette dimension lorsqu’il peut être produit.

Il PEUT être null lorsque la dimension n’a pas été évaluée.

Le champ :

{
  "evidence_basis": "runtime signal only"
}

précise la base déclarée sur laquelle repose le signal.

Un factual_validity_signal produit à partir de signaux runtime seulement NE DOIT PAS être présenté comme une vérification indépendante de la vérité.

Même un statut supported_with_limitations reste borné au contexte et à la base d’évidence déclarés.
12. Signal de variabilité sémantique

Le bloc de référence est :

{
  "semantic_variability_signal": {
    "status": "elevated",
    "semantic_instability_score": 0.58
  }
}

Ce signal qualifie la variabilité sémantique observée dans le périmètre de mesure.

Les payloads de référence exposent notamment des statuts tels que :

low
elevated
low_observed_variability

Le champ :

{
  "semantic_instability_score": 0.58
}

constitue le signal quantifié associé à cette dimension.

Une variabilité faible NE DOIT PAS être interprétée comme une preuve de vérité.

Une variabilité élevée NE permet pas, à elle seule, d’identifier la cause de cette variation ni de déterminer quelle réponse est correcte.
13. Bandes de latence et de coût

Les payloads peuvent exposer :

{
  "latency_band": "medium",
  "cost_band": "low"
}

Ces champs qualifient respectivement la latence et le coût selon les catégories produites par la couche de mesure.

Les exemples observés comportent notamment :

low
medium
unknown

Une valeur unknown indique que la dimension considérée ne doit pas être supposée connue.

Les bandes de latence ou de coût PEUVENT être consommées par des systèmes externes comme informations runtime.

Elles ne constituent pas à elles seules une décision de routage, de politique ou d’exécution.
14. Risk signal

Le bloc de référence est :

{
  "risk_signal": {
    "level": "high",
    "types": [
      "semantic_variability",
      "uncertainty"
    ]
  }
}

risk_signal constitue un signal de mesure relatif à des conditions de risque observées.

Il peut comporter :

    un level ;

    une ou plusieurs catégories dans types.

Les payloads fournis exposent notamment des niveaux :

low
medium
high

et des types tels que :

factual_fragility
semantic_variability
uncertainty
scope_limitation
contradiction_signal
overclaim_signal
factual_risk
incomplete_observation
coverage_limitation

Ces valeurs décrivent des signaux produits par la couche de mesure.

Elles NE constituent PAS automatiquement :

ALLOW
BLOCK
ACCEPT
REJECT
COMPLIANT
NON-COMPLIANT
SAFE
UNSAFE
ADMISSIBLE
NON-ADMISSIBLE

Un système consommateur PEUT utiliser un risk_signal pour déclencher ses propres règles.

La décision résultante reste extérieure au Metric Contract.
15. Limitations connues

Le tableau :

{
  "known_limitations": [
    "Low semantic variability does not demonstrate factual correctness.",
    "No external grounding or independent verification evidence is supplied."
  ]
}

déclare les limitations connues pertinentes pour l’interprétation de l’observation.

Ces limitations font partie du contexte sémantique de la mesure.

Elles NE DEVRAIENT PAS être supprimées lorsqu’une mesure est transmise à un système qui doit l’interpréter.

Une limitation peut notamment préciser :

    qu’une faible variabilité ne démontre pas la correction factuelle ;

    qu’une instabilité observée n’en identifie pas la cause ;

    qu’un signal est borné au contexte observé ;

    qu’un signal de contradiction ou de sur-affirmation n’est pas un verdict ;

    qu’une couverture partielle limite l’interprétation des valeurs disponibles.

Les limitations déclarées ne sont pas des métadonnées décoratives.

Elles participent à la définition de ce que la mesure permet raisonnablement de conclure.
16. Frontière de mesure

Le tableau measurement_boundary déclare explicitement la frontière d’autorité et d’interprétation de la mesure.

Les payloads de référence exposent notamment :

{
  "measurement_boundary": [
    "Synthetic payload for a non-production interoperability demonstration.",
    "No raw prompt, raw model output, provider identity, customer data or proprietary schema is included.",
    "This payload is a runtime observation input only.",
    "It does not establish truth, safety, authority, downstream permission or execution admissibility."
  ]
}

La measurement_boundary est une composante fondamentale du contrat sémantique.

Une mesure NeoMundi NE DOIT PAS être interprétée comme établissant, par elle-même :

    la vérité ;

    la sécurité ;

    une autorité ;

    une permission downstream ;

    l’admissibilité d’une exécution.

Lorsqu’un payload n’expose pas le prompt brut, la sortie brute du modèle, l’identité du fournisseur, des données client ou un schéma propriétaire, ces informations NE DOIVENT PAS être supposées présentes ou reconstruites à partir de la mesure seule.
17. Séparation entre mesure et décision

La couche NeoMundi produit des observations, mesures et signaux.

Un système externe peut ensuite choisir de transformer ces signaux en règles opérationnelles.

Par exemple, selon sa propre autorité, il PEUT décider de :

    poursuivre l’exécution ;

    journaliser l’événement ;

    déclencher une vérification ;

    demander une nouvelle génération ;

    rerouter vers un autre système ;

    escalader ;

    demander une supervision humaine ;

    suspendre ou interrompre une exécution ;

    produire un objet de preuve ;

    appliquer une politique de conformité ou d’admissibilité.

Aucune de ces décisions n’est implicitement contenue dans une valeur NeoMundi.

La frontière fondamentale est :

Observation runtime
→ mesure et signaux NeoMundi
→ consommation par une infrastructure externe
→ règle ou décision relevant de cette infrastructure

NeoMundi reste autoritatif sur la signification de ses mesures.

Le système consommateur reste autoritatif sur la politique et l’action qu’il applique à ces mesures.
18. Neutralité vis-à-vis de l’infrastructure

Le payload d’observation NeoMundi est conçu comme un objet de mesure pouvant être consommé par différents systèmes.

La même couche de mesure PEUT ainsi alimenter plusieurs usages, notamment :

    contrôle continu ;

    monitoring ;

    gouvernance runtime ;

    supervision du risque ;

    conformité ;

    traçabilité ;

    audit ;

    routage ;

    supervision humaine ;

    reconstruction d’incident ;

    evidence ;

    recherche.

Le Metric Contract NE DOIT PAS imposer l’un de ces usages comme interprétation unique de la mesure.

La consommation d’un signal par une infrastructure externe ne transfère pas à NeoMundi l’autorité décisionnelle de cette infrastructure.
19. Valeurs inconnues, nulles ou non évaluées

Le payload peut explicitement représenter l’absence de mesure ou de connaissance.

Les exemples observés comprennent notamment :

{
  "factual_hallucination_score": null,
  "latency_band": "unknown",
  "cost_band": "unknown"
}

ainsi que :

{
  "status": "not_assessed"
}

Une valeur inconnue, null ou non évaluée NE DOIT PAS être remplacée silencieusement par une valeur supposée.

not_assessed ne signifie pas qu’aucun risque n’existe.

unknown ne signifie ni faible ni élevé.

null ne signifie pas zéro.

La distinction entre absence de signal, signal non évalué et valeur mesurée DOIT être préservée lors de l’interprétation.
20. Mesure partielle

Le payload de référence suivant démontre qu’une observation peut être partielle :

{
  "measurement": {
    "measurement_status": "partial",
    "measurement_coverage": 0.54,
    "stability_score": 0.92,
    "coherence_score": 0.79,
    "factual_validity_signal": {
      "status": "not_assessed",
      "factual_hallucination_score": null,
      "evidence_basis": "insufficient measurement coverage"
    },
    "semantic_variability_signal": {
      "status": "low_observed_variability",
      "semantic_instability_score": 0.08
    },
    "latency_band": "unknown",
    "cost_band": "unknown",
    "risk_signal": {
      "level": "medium",
      "types": [
        "incomplete_observation",
        "coverage_limitation"
      ]
    }
  }
}

Cet exemple établit une règle sémantique essentielle :

    Une valeur disponible n’implique pas que toutes les dimensions de l’observation ont été mesurées.

Une forte stabilité observée dans une mesure partielle NE DOIT PAS être interprétée comme une preuve complète sur le comportement du système.

Les dimensions manquantes ou non évaluées peuvent modifier matériellement l’usage que fera un système consommateur de l’observation.
21. Traçabilité

Le payload expose deux niveaux d’identification actuellement observés :

{
  "observation_id": "nm-syn-001",
  "source": {
    "source_batch_id": "nm-synthetic-demo-v0.1",
    "trace_id": "syn-trace-001"
  }
}

observation_id identifie l’observation NeoMundi.

source_batch_id permet de rattacher l’observation à son lot source déclaré.

trace_id fournit un identifiant de trace associé à la source.

Ces identifiants participent à la traçabilité de l’observation.

Le Metric Contract ne présume pas, dans cette Draft v0.0, de mécanismes cryptographiques, signatures, hashes ou reçus qui ne sont pas présents dans les payloads de référence.

Ces mécanismes PEUVENT être définis dans d’autres couches ou versions du système sans modifier la signification fondamentale des mesures existantes.
22. Confidentialité et données non exposées

Les payloads de référence indiquent explicitement qu’ils n’incluent pas :

    le prompt brut ;

    la sortie brute du modèle ;

    l’identité du fournisseur ;

    les données client ;

    le schéma propriétaire interne.

Le Metric Contract porte donc sur les mesures et signaux effectivement exposés, et non sur les données internes nécessaires à leur calcul.

Un système consommateur n’a pas besoin, du seul fait qu’il reçoit un payload de mesure, d’obtenir accès au moteur interne NeoMundi ou aux données non exposées par le payload.

La publication d’une mesure ne signifie pas la publication de la méthode interne complète ayant permis de la produire.
23. Frontière d’interopérabilité

Le Metric Contract et le Runtime Interoperability Contract remplissent deux fonctions distinctes.
Metric Contract

Il définit :

    ce que signifient les mesures ;

    quelles limites leur sont attachées ;

    quelles interprétations sont permises ou interdites ;

    où s’arrête l’autorité de la couche de mesure.

Runtime Interoperability Contract

Il définit ou définira :

    comment les objets sont échangés ;

    quelles structures sont requises ;

    quels champs et types sont contractuels ;

    comment les versions sont négociées ;

    comment les systèmes producteurs et consommateurs interopèrent.

La représentation machine actuelle constitue la base opérationnelle à partir de laquelle cette interopérabilité est formalisée.

Le Metric Contract NE DOIT PAS inventer une représentation machine concurrente de celle effectivement produite par NeoMundi.
24. Règle de versionnement sémantique

Une évolution qui modifie matériellement la signification d’un signal DEVRA être rendue explicitement distinguable de la définition antérieure.

Cela concerne notamment une modification substantielle de :

    ce qui est effectivement mesuré ;

    la manière dont une valeur doit être interprétée ;

    la signification d’un statut ;

    la signification d’un niveau ou d’une catégorie de risque ;

    la frontière de ce que la mesure permet de conclure.

Une simple modification de transport ou de sérialisation qui ne modifie pas la sémantique de la mesure peut relever du versionnement du schéma d’interopérabilité plutôt que du Metric Contract.

Les observations historiques DEVRAIENT rester interprétables selon la version sous laquelle elles ont été produites.
25. Payload de référence

La structure suivante rassemble les éléments effectivement observés dans les payloads fournis :

{
  "schema_version": "neomundi_observation_payload_v0.1",
  "observation_id": "nm-syn-001",
  "generated_at": "2026-06-28T06:42:47Z",
  "synthetic": true,
  "source": {
    "source_batch_id": "nm-synthetic-demo-v0.1",
    "trace_id": "syn-trace-001",
    "model_alias": "system_alpha",
    "prompt_class": "general_explanatory"
  },
  "measurement": {
    "measurement_status": "complete",
    "stability_score": 0.94,
    "coherence_score": 0.88,
    "factual_validity_signal": {
      "status": "fragile",
      "factual_hallucination_score": 0.41,
      "evidence_basis": "runtime signal only"
    },
    "semantic_variability_signal": {
      "status": "low",
      "semantic_instability_score": 0.06
    },
    "latency_band": "medium",
    "cost_band": "low",
    "risk_signal": {
      "level": "medium",
      "types": [
        "factual_fragility"
      ]
    }
  },
  "known_limitations": [
    "Low semantic variability does not demonstrate factual correctness.",
    "No external grounding or independent verification evidence is supplied."
  ],
  "measurement_boundary": [
    "Synthetic payload for a non-production interoperability demonstration.",
    "No raw prompt, raw model output, provider identity, customer data or proprietary schema is included.",
    "This payload is a runtime observation input only.",
    "It does not establish truth, safety, authority, downstream permission or execution admissibility."
  ]
}

Ce payload est inclus ici comme référence de structure et de sémantique observée pour la Draft v0.0.

Les valeurs numériques, identifiants et classifications de cet exemple décrivent une observation synthétique particulière et NE constituent PAS des valeurs normatives du Metric Contract.
26. Principe fondamental

Une observation NeoMundi expose un ensemble contextualisé de mesures et de signaux portant sur le comportement observable d’un système d’IA dans un périmètre runtime déclaré.

Ces mesures constituent des faits de mesure dans les limites explicitement déclarées par leur contexte, leur couverture, leurs limitations et leur frontière d’interprétation.

Elles peuvent être consommées par plusieurs systèmes, plusieurs usages et plusieurs infrastructures sans que ces systèmes aient à reproduire le moteur interne de mesure NeoMundi.

La mesure ne transfère pas l’autorité de décision à NeoMundi.

Elle fournit un signal commun sur lequel d’autres systèmes peuvent exercer leur propre autorité.

    Mesuré par NeoMundi.

    Décidé et utilisé selon l’autorité du système consommateur.

NeoMundi Metric Contract — Draft v0.0
