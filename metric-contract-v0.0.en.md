[🇬🇧 English version](./metric-contract-v0.0.en.md) · [← Repository](./README.md)

> **Draft notice — v0.0**
>
> Ce document définit les frontières sémantiques actuelles de la couche de mesure NeoMundi.
>
> Les structures JSON, noms de champs, identifiants, enums et exemples présentés dans cette version constituent des références opérationnelles ou illustratives. Ils ne constituent pas encore le schéma définitif du **NeoMundi Runtime Interoperability Contract**.
>
> La représentation exploitable par machine pourra évoluer avec la formalisation de l’interopérabilité. Toute modification qui change matériellement la signification d’une mesure ou d’un signal devra cependant faire l’objet d’un versionnement explicite.

# NeoMundi Metric Contract

**Version :** 0.0
**Statut :** Draft
**Date :** 16 août 2026
**Maintainer :** NeoMundi
**Scope :** Mesure runtime du comportement observable des systèmes d’IA

---

## 0. Langage normatif

Dans ce document :

* **DOIT / NE DOIT PAS** désigne une exigence sémantique obligatoire ;
* **DEVRAIT / NE DEVRAIT PAS** désigne une exigence recommandée à laquelle une implémentation peut déroger avec justification ;
* **PEUT** désigne un comportement autorisé mais non obligatoire.

La portée normative de ce contrat est limitée à la **signification, l’interprétation et aux frontières sémantiques** de la couche de mesure NeoMundi.

Les noms exacts de champs, leur emplacement JSON, les enums, la sérialisation, le transport et la négociation de version ne deviennent pas normatifs du seul fait qu’ils apparaissent dans les représentations actuelles.

Ces éléments relèvent principalement du **Runtime Interoperability Contract**.

---

## 1. Objet

Le **NeoMundi Metric Contract** définit la signification, les conditions d’interprétation, les limitations et les frontières des mesures et signaux produits par la couche de mesure runtime NeoMundi.

Il définit **ce que signifie une mesure NeoMundi**.

Il ne définit pas la politique, la décision opérationnelle ou l’autorisation d’exécution qu’un système externe peut dériver de cette mesure.

> **Le Metric Contract NE DOIT PAS définir une décision de politique, une autorisation d’exécution ou une action opérationnelle comme signification intrinsèque d’une mesure NeoMundi.**

**NeoMundi mesure. Le système consommateur conserve l’autorité de décision et d’action.**

---

## 2. Référence runtime actuelle

La Draft v0.0 est ancrée sur la structure du payload d’observation actuellement exposé par l’API NeoMundi.

Exemple de structure :

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

Cette structure constitue l’**ancrage opérationnel actuel** du contrat.

Elle ne constitue pas un JSON Schema normatif complet.

Le Metric Contract **NE DOIT PAS** introduire une représentation machine concurrente qui modifierait la signification des mesures effectivement exposées par NeoMundi.

Les exigences exactes de structure, de sérialisation et de transport sont différées vers le **Runtime Interoperability Contract**.

---

## 3. Concepts fondamentaux

Le Metric Contract distingue plusieurs concepts liés mais non interchangeables.

### 3.1 Observation

Une **observation** est un événement runtime ou un ensemble d’exécutions runtime dans des conditions déclarées, à partir desquels une ou plusieurs mesures peuvent être produites.

Une mesure NeoMundi **DOIT rester attribuable à l’observation à laquelle elle appartient**.

Une observation synthétique **NE DOIT PAS être présentée comme une observation de production**.

### 3.2 Métrique

Une **métrique** définit une propriété observable mesurée par NeoMundi ainsi que la sémantique permettant d’interpréter ses valeurs ou signaux.

### 3.3 Mesure

Une **mesure** est un résultat obtenu à partir d’une observation selon une méthode ou un protocole déclaré.

La mesure représente un résultat spécifique.

Elle ne constitue pas, à elle seule, une décision externe.

### 3.4 État métrologique

Une mesure **PEUT** être associée à un état ou une classification métrologique selon une taxonomie ou un référentiel déclaré et versionné.

Un état métrologique décrit une condition observée.

Il ne constitue pas automatiquement une décision de politique ou une autorisation d’exécution.

### 3.5 Relation conceptuelle

La relation générale est :

**Observation → mesure ou signal → interprétation métrologique éventuelle → consommation externe**

La décision ou l’action résultante reste extérieure au Metric Contract.

---

## 4. Contexte source

Une mesure n’existe pas indépendamment de son contexte d’observation.

Les informations déclarées sur la source, le modèle, le provider, l’environnement, la configuration ou le protocole peuvent contribuer à son interprétation.

Une information qui n’est pas explicitement présente ou déclarée dans le contexte d’observation **NE DOIT PAS être silencieusement déduite comme mesurée ou connue**.

En particulier, un alias de modèle ne constitue pas à lui seul une preuve :

* de l’identité du provider ;
* de la version exacte du modèle ;
* de la configuration d’exécution ;
* d’un environnement runtime particulier.

Les éléments inconnus, indisponibles ou non déclarés doivent rester distinguables des éléments effectivement observés ou déclarés.

---

## 5. Mesure et contexte d’interprétation

Une valeur ou un signal NeoMundi ne doit pas être interprété isolément lorsque son sens dépend matériellement :

* des conditions d’observation ;
* de la couverture ;
* du statut de mesure ;
* des limitations connues ;
* de la frontière de mesure applicable.

> Une mesure ou un signal **NE DOIT PAS être interprété indépendamment des limitations et frontières applicables lorsque celles-ci affectent matériellement sa signification**.

La présence d’une valeur numérique n’implique pas à elle seule que toutes les dimensions de mesure aient été évaluées.

---

## 6. Statut et couverture de mesure

Une observation peut être complète, partielle, non évaluée ou autrement limitée.

Une observation partielle **NE DOIT PAS être représentée comme complète**.

La présence d’une valeur dans une observation partielle **NE DOIT PAS impliquer que toutes les dimensions de mesure ont été évaluées**.

Une couverture incomplète connue **DOIT rester distinguable d’une couverture complète** lors de l’interprétation.

Le nom exact du champ, son format et son encodage relèvent du Runtime Interoperability Contract.

---

## 7. `stability_score`

`stability_score` décrit un niveau de stabilité comportementale observée dans les conditions déclarées de mesure.

Il ne mesure pas intrinsèquement la vérité factuelle.

> **`stability_score` NE DOIT PAS être interprété comme une mesure de vérité factuelle.**

`stability_score` **NE DOIT PAS**, à lui seul, établir :

* la sécurité ;
* la conformité ;
* l’admissibilité ;
* l’autorisation d’exécution ;
* la qualité globale d’un système.

Une stabilité élevée **PEUT** coexister avec une sortie factuellement incorrecte.

Une variation comportementale peut également coexister avec des sorties factuellement correctes.

### Principe

> **Stabilité ≠ vérité.**

---

## 8. `coherence_score`

`coherence_score` décrit une propriété de cohérence observée selon la définition et les conditions de mesure applicables.

`coherence_score` **NE DOIT PAS**, à lui seul, être interprété comme :

* une validation factuelle ;
* une validation de sécurité ;
* une détermination de conformité ;
* une autorisation d’exécution.

Il peut être consommé avec d’autres mesures ou signaux.

Le Metric Contract ne prescrit pas la politique qu’un système externe doit appliquer à partir de cette information.

---

## 9. `factual_validity_signal`

Un signal de validité factuelle runtime représente une information produite dans les limites de la méthode de mesure qui lui est associée.

Un signal fondé uniquement sur des éléments disponibles au runtime **NE DOIT PAS être présenté comme une vérification indépendante de la vérité lorsqu’aucune validation indépendante n’a été effectuée**.

Une valeur `null`, `unknown` ou `not_assessed` **NE DOIT PAS être interprétée comme** :

* une validation factuelle ;
* une absence d’erreur ;
* un risque factuel nul.

Les taxonomies, états et enums observés dans les représentations actuelles restent descriptifs tant qu’ils ne sont pas explicitement stabilisés et versionnés.

---

## 10. `semantic_variability_signal`

`semantic_variability_signal` décrit une variabilité sémantique observée dans les conditions applicables.

Une faible variabilité sémantique **NE DOIT PAS être interprétée comme une preuve de correction factuelle**.

Une variabilité élevée **NE DOIT PAS**, à elle seule :

* identifier la cause de la variation ;
* identifier quelle sortie est correcte ;
* constituer une preuve d’erreur.

Il s’agit d’un signal de mesure, non d’une décision de politique.

---

## 11. Latence et coût

NeoMundi peut exposer des mesures, signaux ou bandes relatifs à la latence et au coût.

Dans la Draft v0.0, le Metric Contract ne fige pas :

* les enums définitifs ;
* les seuils exacts ;
* les frontières entre bandes ;
* les conséquences opérationnelles ou de routage.

Une valeur explicitement inconnue de latence ou de coût **NE DOIT PAS être silencieusement interprétée comme une valeur mesurée**.

Les définitions structurelles et les seuils éventuels devront être spécifiés et versionnés séparément.

---

## 12. `risk_signal`

Un `risk_signal` NeoMundi constitue un signal de mesure ou d’interprétation dans les limites déclarées de la métrique.

Il **NE DOIT PAS automatiquement signifier** :

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

Un système consommateur **PEUT** utiliser ce signal comme entrée de sa propre politique.

La décision résultante reste extérieure au Metric Contract.

Les niveaux, types et enums de risque actuellement observés ne deviennent pas automatiquement des catégories normatives de la Draft v0.0.

---

## 13. Limitations connues

Une mesure NeoMundi peut comporter des limitations relatives :

* à la méthode ;
* aux données disponibles ;
* au protocole ;
* à la couverture ;
* au contexte ;
* au domaine de mesure ;
* à l’interprétation possible.

Toute limitation qui affecte matériellement l’interprétation d’une mesure **DOIT rester sémantiquement associée à cette mesure lorsqu’elle est interprétée ou transmise pour soutenir une décision**.

Le format exact, l’emplacement et la sérialisation de ces limitations relèvent de l’interopérabilité.

---

## 14. Frontière de mesure

Une mesure NeoMundi décrit une propriété observable dans un périmètre déclaré.

Elle ne constitue pas automatiquement une conclusion générale sur le système observé.

> **Une mesure NeoMundi NE DOIT PAS, à elle seule, être interprétée comme établissant la vérité, la sécurité, une autorité, une permission downstream ou l’admissibilité d’une exécution, sauf si une future métrique explicitement versionnée le définit autrement.**

Cette frontière constitue l’un des principes normatifs fondamentaux du Metric Contract.

---

## 15. Séparation entre mesure et décision

NeoMundi est responsable de la sémantique déclarée de ses mesures.

Le système consommateur reste responsable de la politique ou de l’action opérationnelle qu’il dérive de ces mesures.

Des infrastructures externes **PEUVENT**, par exemple, utiliser une mesure NeoMundi pour :

* journaliser ;
* vérifier ;
* régénérer ;
* rerouter ;
* escalader ;
* demander une supervision humaine ;
* interrompre un workflow ;
* alimenter un processus de conformité ;
* produire ou enrichir un artefact d’evidence.

Cette liste est illustrative et non exhaustive.

Aucune de ces actions ne constitue la signification intrinsèque de la mesure.

---

## 16. Neutralité vis-à-vis de l’infrastructure

Le Metric Contract **NE DOIT PAS imposer un usage downstream comme interprétation unique d’une mesure**.

Une même mesure **PEUT** être consommée :

* par plusieurs infrastructures ;
* dans plusieurs architectures ;
* pour plusieurs finalités.

La logique de politique propre à chaque infrastructure reste extérieure au Metric Contract.

Cette séparation permet à NeoMundi de fournir une couche de mesure commune sans imposer l’architecture de décision qui la consomme.

---

## 17. Valeurs inconnues, nulles et non évaluées

Les états suivants ne sont pas sémantiquement équivalents :

* valeur mesurée ;
* valeur indisponible ;
* `null` ;
* `unknown` ;
* `not_assessed`.

`null` **NE DOIT PAS être interprété comme zéro**.

`unknown` **NE DOIT PAS être interprété comme une catégorie mesurée, faible ou élevée**.

`not_assessed` **NE DOIT PAS être interprété comme une absence de risque ou comme une validation réussie**.

La distinction entre valeurs mesurées, inconnues, indisponibles et non évaluées **DOIT être préservée sémantiquement**.

Les règles exactes d’encodage relèvent de l’interopérabilité.

---

## 18. Mesures partielles

Une observation ou une mesure peut être partielle.

Une observation partielle **NE DOIT PAS être présentée comme complète**.

Une valeur disponible dans une observation partielle **NE DOIT PAS impliquer que toutes les dimensions de mesure ont été évaluées**.

Les systèmes consommateurs **DEVRAIENT** tenir compte de la couverture déclarée lors de l’interprétation de mesures partielles.

---

## 19. Traçabilité

La représentation actuelle peut notamment comporter des identifiants tels que :

* `observation_id`;
* `source_batch_id`;
* `trace_id`;
* un timestamp ;
* des références de version.

Une mesure **DEVRAIT rester attribuable à son observation d’origine**.

La Draft v0.0 ne fixe pas encore la structure obligatoire de traçabilité.

Les exigences concernant :

* hashes ;
* signatures ;
* reçus ;
* identifiants obligatoires ;
* intégrité cryptographique ;
* artefacts d’evidence ;

relèvent du Runtime Interoperability Contract ou de spécifications dédiées.

---

## 20. Confidentialité et données non exposées

Le Metric Contract décrit les données et mesures exposées par l’interface de mesure.

Il n’implique pas un accès aux données internes de calcul d’un système d’IA.

Les exemples synthétiques actuels peuvent ne pas exposer, notamment :

* les prompts bruts ;
* les sorties brutes du modèle ;
* certaines informations provider ;
* des données client ;
* des structures propriétaires.

Cette observation ne constitue pas, dans la Draft v0.0, une interdiction normative universelle.

Les exigences de confidentialité, de sécurité et d’exposition des données devront être définies dans les spécifications appropriées.

---

## 21. Frontière avec le Runtime Interoperability Contract

Le **Metric Contract** définit la sémantique des mesures et signaux.

Le **Runtime Interoperability Contract** définit, ou définira, la manière dont les objets correspondants sont échangés et consommés entre systèmes.

Il pourra notamment spécifier :

* les structures machine-readable ;
* les champs requis ;
* les types ;
* les enums ;
* la sérialisation ;
* la compatibilité ;
* la négociation de version ;
* les contraintes d’intégration.

Le Metric Contract **NE DOIT PAS figer des détails de transport ou d’interopérabilité qui n’ont pas encore été validés**.

L’architecture conceptuelle est donc :

**exécution IA → observation NeoMundi → mesure ou signal NeoMundi → interopérabilité → système consommateur → décision ou action externe**

---

## 22. Versionnement sémantique

Une modification matérielle de la signification d’une mesure ou d’un signal **DOIT être explicitement versionnée**.

Cela concerne notamment les changements affectant substantiellement :

* la propriété mesurée ;
* la définition sémantique ;
* le domaine d’interprétation ;
* les limitations fondamentales ;
* le sens d’une valeur ou d’un signal.

Les observations historiques **DEVRAIENT rester interprétables selon la version sémantique sous laquelle elles ont été produites**.

Les changements de structure ou de sérialisation qui ne modifient pas la signification **PEUVENT** relever exclusivement du versionnement du schéma ou de l’interopérabilité.

---

## 23. Exemple de référence opérationnelle

L’exemple suivant illustre la structure actuelle autour de laquelle la Draft v0.0 est construite :

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

### Statut de cet exemple

**Exemple de référence opérationnelle — ne constitue pas un JSON Schema normatif complet.**

En particulier :

* les valeurs d’exemple ne sont pas normatives ;
* les identifiants d’exemple ne sont pas normatifs ;
* les noms et emplacements exacts des champs ne sont pas figés par le Metric Contract ;
* les enums observés ne sont pas automatiquement normatifs ;
* les exigences exactes de transport restent soumises au Runtime Interoperability Contract.

Dans cet exemple, `synthetic: true` indique explicitement qu’il s’agit d’une observation synthétique.

Elle ne doit pas être présentée comme une observation de production.

---

## 24. Éléments explicitement différés

La Draft v0.0 ne fige pas encore :

* le JSON Schema définitif ;
* la liste définitive des champs obligatoires ;
* les noms définitifs de tous les champs ;
* les enums définitifs ;
* le protocole de transport ;
* la négociation de version ;
* les reçus cryptographiques ;
* les signatures et hashes ;
* le packaging d’evidence ;
* les règles de divulgation du provider ;
* les valeurs exactes des seuils ;
* les frontières exactes des bandes ;
* les règles automatiques de routage ;
* les règles automatiques d’arrêt ;
* la détermination de conformité ;
* la détermination d’admissibilité ;
* l’autorisation d’exécution.

Ces éléments peuvent relever :

* du **Runtime Interoperability Contract** ;
* de spécifications d’evidence ;
* de configurations produit ;
* de la politique du système consommateur ;
* de futures spécifications explicitement versionnées.

---

## 25. Noyau normatif v0.0

La Draft v0.0 concentre volontairement ses exigences normatives sur un nombre limité de frontières durables :

1. Une mesure **DOIT** rester attribuable à son observation.
2. Une observation synthétique **NE DOIT PAS** être présentée comme une observation de production.
3. Une information non déclarée **NE DOIT PAS** être silencieusement déduite comme mesurée ou connue.
4. Une mesure partielle **NE DOIT PAS** être présentée comme complète.
5. La stabilité **NE DOIT PAS** être interprétée comme la vérité factuelle.
6. La cohérence **NE DOIT PAS**, à elle seule, être interprétée comme une validation factuelle.
7. Un signal factuel runtime **NE DOIT PAS** être présenté comme une vérification indépendante de la vérité lorsqu’il ne repose pas sur une validation indépendante.
8. La variabilité sémantique **NE DOIT PAS** être traitée comme une preuve de correction factuelle ni comme une identification de la sortie correcte.
9. Un signal de risque **NE DOIT PAS** devenir automatiquement une décision de politique ou d’exécution.
10. Les limitations matérielles de mesure **DOIVENT** rester sémantiquement associées à l’interprétation.
11. Une mesure NeoMundi **NE DOIT PAS**, à elle seule, établir la vérité, la sécurité, une autorité, une permission downstream ou l’admissibilité d’une exécution.
12. Les états inconnus, nuls ou non évalués **NE DOIVENT PAS** être silencieusement transformés en valeurs mesurées.
13. Toute modification sémantique matérielle **DOIT** être explicitement versionnée.
14. Le Metric Contract **NE DOIT PAS** figer des détails de transport ou d’interopérabilité non validés.
15. Le système consommateur conserve l’autorité sur les décisions et actions qu’il dérive des mesures NeoMundi.

Ce noyau volontairement réduit vise à rendre la Draft v0.0 plus robuste qu’un contrat comportant de nombreuses obligations structurelles encore prématurées.

---

## 26. Principe fondamental

**NeoMundi fournit des mesures et signaux contextualisés portant sur le comportement observable des systèmes d’IA dans des frontières runtime déclarées.**

Ces mesures peuvent être consommées par plusieurs systèmes, infrastructures et usages sans transférer à NeoMundi l’autorité de décision downstream.

> **Measured by NeoMundi.**
> **Used according to the authority of the consuming system.**

---

**NeoMundi Metric Contract — Draft v0.0**
