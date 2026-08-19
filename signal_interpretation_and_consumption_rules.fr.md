# NeoMundi — Règles d’interprétation et de consommation des signaux

**Contrat d’interprétation des mesures runtime et règles de consommation des signaux NeoMundi**

> **Statut :** Contrat expérimental / pré-freeze
> **Périmètre :** Sorties de mesure runtime, sémantique des signaux, frontières de consommation, interopérabilité et règles d’implémentation
> **Principe fondamental :** **NeoMundi mesure. Le système consommateur interprète, gouverne et agit.**

---

## 1. Objet

Le présent document définit l’interface observable entre la couche de mesure runtime NeoMundi et les systèmes externes qui consomment les signaux NeoMundi.

Son objectif est de rendre les mesures NeoMundi :

* explicites ;
* consommables par machine ;
* versionnées ;
* interprétables ;
* reproductibles ;
* interopérables ;
* utilisables sans avoir à reconstruire ou déduire la logique interne de NeoMundi.

Ce contrat définit **ce que signifient les sorties NeoMundi**.

Il établit également la frontière entre :

1. **Mesure** — ce que NeoMundi observe et émet ;
2. **Interprétation** — ce qui peut légitimement être déduit de ces mesures ;
3. **Politique du consommateur** — ce qu’un système externe choisit de faire de ces mesures ;
4. **Exécution** — l’action effectivement appliquée au système d’IA.

Architecture cible :

```text
Génération IA
     │
     ▼
Mesure NeoMundi
     │
     ▼
Signaux runtime
     │
     ▼
Interprétation / politique du consommateur
     │
     ▼
CONTINUE / VERIFY / STOP / REGENERATE / REROUTE / ABSTAIN / autre action
```

NeoMundi constitue donc une **couche de mesure runtime**, et non un moteur universel de politique ou de décision.

---

## 2. Terminologie normative

Les termes **DOIT**, **NE DOIT PAS**, **DEVRAIT**, **NE DEVRAIT PAS** et **PEUT** sont utilisés dans leur sens normatif.

Trois catégories d’information sont distinguées dans ce document.

### NORMATIF

Fait partie du Metric Contract.

Un consommateur peut implémenter son système sur cette base.

### EMPIRIQUE

Résultat observé dans des expériences NeoMundi ou dans des études externes.

Un résultat empirique **NE DOIT PAS** être automatiquement transformé en règle de protocole.

### INTERNE / NON CONTRACTUEL

Éléments de l’implémentation interne de NeoMundi qui ne sont pas nécessaires à la consommation correcte de la mesure.

---

## 3. Frontière fondamentale

Un signal NeoMundi est un **signal de mesure**, et non en lui-même un verdict de vérité, de sécurité, de légalité ou d’acceptabilité métier.

En particulier :

```text
stabilité élevée ≠ exactitude factuelle
```

```text
FLAG ≠ erreur prouvée
```

```text
ALLOW ≠ vérité prouvée
```

L’infrastructure consommatrice reste responsable de déterminer les conséquences opérationnelles associées à une mesure NeoMundi.

---

## 4. Modèle des signaux runtime

La famille de signaux runtime NeoMundi actuellement documentée comprend notamment les objets suivants :

| Signal                | Type          | Signification                          | Statut                   |
| --------------------- | ------------- | -------------------------------------- | ------------------------ |
| `G` / `g_score`       | numérique     | Signal de stabilité générative runtime | Défini conceptuellement  |
| `stability_score`     | float `[0,1]` | Stabilité globale de la génération     | Défini                   |
| `delta_g` / `∆G`      | numérique     | Variation de la stabilité runtime      | Défini conceptuellement  |
| `delta_series`        | tableau       | Évolution de ∆G pendant la génération  | Défini                   |
| `delta_variation`     | numérique     | Amplitude globale de variation de ∆G   | Défini                   |
| `delta_profile`       | enum          | Forme de la trajectoire ∆G             | Défini                   |
| `decision`            | enum          | Classification runtime                 | Défini                   |
| `hallucination_score` | numérique     | Signal associé au risque factuel       | Signal exposé            |
| `coherence_score`     | numérique     | Signal de cohérence sémantique         | Signal exposé            |
| `regime`              | enum / état   | Contexte runtime synthétique           | Défini conceptuellement  |
| `total_tokens`        | entier        | Nombre de tokens générés               | Défini                   |
| `g_final`             | numérique     | Valeur finale associée à G             | Relation restant à figer |

La présence d’un champ dans ce tableau **n’implique pas** que sa méthode de calcul interne soit publique.

---

## 5. G / G-score

### 5.1 Signification

`G` représente une propriété runtime liée à la stabilité, à la régularité ou à la cohérence du processus génératif.

Il **NE DOIT PAS** être interprété comme un score direct de vérité factuelle.

Une valeur élevée de G peut coexister avec une réponse incorrecte.

Ce phénomène est désigné comme :

> **stabilité trompeuse**

Exemple :

```text
G élevé
+
réponse factuellement incorrecte
=
génération stable mais incorrecte
```

La conclusion opérationnelle correcte est donc :

```text
G mesure une stabilité runtime.
G n’établit pas la vérité factuelle.
```

---

## 6. Relation entre G et factualité

Aucun consommateur **NE DOIT** inférer :

```text
G ↑  => exactitude factuelle ↑
```

Les travaux expérimentaux disponibles montrent que des valeurs élevées ou proches de G peuvent coexister avec des niveaux d’exactitude factuelle très différents.

La validation factuelle **DOIT** donc rester logiquement distincte de la mesure de stabilité runtime.

Lorsqu’une tâche exige une validation factuelle, une couche de validation externe **PEUT** être utilisée.

---

## 7. `stability_score`

`stability_score` représente la stabilité globale de la génération.

Plage actuellement documentée :

```text
0.0 <= stability_score <= 1.0
```

Direction d’interprétation :

```text
valeur élevée -> stabilité runtime mesurée plus élevée
valeur faible  -> stabilité runtime mesurée plus faible
```

Il **NE DOIT PAS** être interprété comme :

* une confiance factuelle ;
* une probabilité que la réponse soit correcte ;
* une probabilité de sécurité ;
* une probabilité de conformité.

---

## 8. Delta G — ∆G

`∆G` représente l’évolution du signal de stabilité runtime au cours de la génération.

Conceptuellement :

```text
∆G > 0
```

indique une augmentation de la variable de stabilité mesurée sur l’intervalle considéré.

```text
∆G < 0
```

indique une dégradation de cette variable.

La transformation interne exacte produisant ∆G **DOIT** être associée à l’implémentation et à la version du normaliseur correspondantes.

Les consommateurs **NE DOIVENT PAS** reconstruire leur propre ∆G à partir de champs différents sauf si le contrat définit explicitement cette transformation.

---

## 9. `delta_series`

`delta_series` contient l’évolution temporelle de ∆G.

Chaque observation est associée à une position dans la génération, généralement une position token et une valeur numérique de ∆G.

Représentation conceptuelle :

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

Cet exemple est uniquement illustratif.

La sérialisation faisant autorité **DOIT** être définie par le schéma ENF versionné.

---

## 10. `delta_profile`

Les profils ∆G actuellement documentés sont :

```text
DROP
FLAT
V_SHAPE
```

### DROP

Dégradation du signal de stabilité sans récupération ultérieure suffisante.

Interprétation opérationnelle :

```text
dégradation runtime / rupture / tension
```

### FLAT

Trajectoire globalement stable, sans dégradation ou récupération significative observée.

Interprétation :

```text
trajectoire ∆G stable ou approximativement stable
```

### V_SHAPE

Dégradation suivie d’une récupération partielle.

Interprétation :

```text
dégradation temporaire suivie d’une récupération
```

Ces profils décrivent **la forme du signal**.

Ils **NE DOIVENT PAS** être automatiquement interprétés comme des jugements factuels.

---

## 11. ALLOW et FLAG

La classification runtime finale actuellement documentée est binaire :

```text
ALLOW
FLAG
```

### ALLOW

`ALLOW` signifie que la mesure runtime NeoMundi n’a pas classé la génération comme nécessitant une alerte runtime dans la configuration de mesure applicable.

Cela **NE SIGNIFIE PAS** :

```text
vrai
sûr
approuvé
conforme
factuellement vérifié
```

### FLAG

`FLAG` signifie que la mesure runtime NeoMundi a identifié une génération nécessitant une attention particulière selon la configuration de mesure applicable.

`FLAG` **PEUT** être consommé comme déclencheur de :

* vérification supplémentaire ;
* escalade ;
* revue humaine ;
* régénération ;
* reroutage ;
* arrêt anticipé ;
* journalisation ;
* production d’un objet de preuve.

Il **NE DOIT PAS** être automatiquement interprété comme la preuve que la sortie est incorrecte.

---

## 12. Relation entre DROP et FLAG

Les travaux expérimentaux de réplication NeoMundi ont observé une relation très forte entre `DROP` et `FLAG`, allant jusqu’à une correspondance parfaite dans plusieurs campagnes testées.

Cette relation est actuellement classée :

```text
EMPIRIQUE
```

sauf si une version spécifique du Metric Contract la promeut explicitement au statut :

```text
NORMATIF
```

Un consommateur **NE DOIT** donc pas supposer de lui-même :

```text
DROP ⇔ FLAG
```

comme invariant universel sur la seule base d’observations expérimentales.

---

## 13. Régime runtime

Un `regime` runtime peut fournir une description synthétique de l’état du système.

Un état actuellement documenté est :

```text
STABLE
```

`STABLE` **DOIT** être compris comme un contexte runtime.

Il **NE DOIT PAS**, à lui seul, établir :

* l’exactitude factuelle ;
* l’absence d’hallucination ;
* l’absence de FLAG ;
* l’absence de DROP ;
* l’autorisation de supprimer une vérification.

Un consommateur **DEVRAIT** interpréter le régime conjointement avec les autres signaux runtime.

---

## 14. Interprétation multi-signaux

NeoMundi **DEVRAIT** être consommé comme un système de mesure multi-signaux.

La combinaison conceptuelle minimale est :

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
signaux de validation pertinents pour la tâche
```

Aucun signal unique **NE DEVRAIT** être considéré comme une caractérisation complète du risque génératif.

### G élevé + échec factuel

```text
Interprétation :
stabilité trompeuse

Action consommateur possible :
validation factuelle externe
```

### FLAG + DROP

```text
Interprétation :
zone prioritaire de dégradation runtime

Actions possibles :
vérification / escalade / arrêt / régénération
```

### STABLE seul

```text
Interprétation :
information insuffisante pour conclure opérationnellement

Action :
consulter les autres signaux
```

### ALLOW + échec factuel

```text
Interprétation :
la mesure runtime n’a pas détecté l’échec factuel

Action :
maintenir une validation factuelle lorsque la tâche l’exige
```

---

## 15. Signaux factuels et sémantiques

NeoMundi peut exposer des signaux complémentaires, notamment :

```text
hallucination_score
coherence_score
```

Ces signaux représentent des dimensions distinctes de la stabilité générative.

Conceptuellement :

```text
stabilité
factualité
cohérence sémantique
```

sont des axes séparés.

Ils **NE DOIVENT PAS** être silencieusement fusionnés dans une notion unique de « qualité ».

La plage, la normalisation, le modèle, la calibration et la version exacte de chaque score auxiliaire **DOIVENT** être définis avant toute utilisation normative.

---

## 16. Seuils

### 16.1 Aucun seuil ne doit être inféré

Les consommateurs **NE DOIVENT PAS** déduire des seuils officiels NeoMundi à partir d’études exploratoires publiées.

Par exemple :

```text
∆G < 0
```

a été utilisé expérimentalement comme critère de détection du premier événement de dégradation dans une étude d’early stopping.

Ce critère était méthodologique et exploratoire.

Il ne constitue **pas automatiquement un seuil officiel d’arrêt NeoMundi**.

De même :

```text
∆G < -0.05
∆G < -0.10
∆G < -0.15
```

ont été explorés expérimentalement.

Ils **NE DOIVENT PAS** être considérés comme normatifs sauf s’ils sont explicitement intégrés dans une version du contrat.

---

## 17. Seuils dépendant du provider, du modèle, de la tâche ou du contexte

Aucun consommateur **NE DEVRAIT** supposer qu’un seuil numérique est universellement transportable entre :

* providers ;
* modèles ;
* versions de modèle ;
* catégories de prompts ;
* familles de tâches ;
* configurations de génération ;
* versions du normaliseur.

Les recherches montrent notamment que la composition des prompts et les conditions provider/modèle peuvent influencer significativement les distributions observées.

Tout seuil contextualisé **DOIT** au minimum identifier :

```text
metric_version
normalizer_version
provider/model scope
task/context scope
threshold value
threshold semantics
```

En l’absence de ces éléments, le seuil **DOIT** être considéré comme non autoritatif.

---

## 18. Autorité du consommateur et chemins computationnels

Cette section est normative.

NeoMundi produit la mesure.

Le système consommateur détient la politique d’exécution.

```text
NeoMundi
   ↓
mesure
   ↓
consommateur C
   ↓
décision de politique
   ↓
système B
```

Un signal NeoMundi **PEUT** conduire C à sélectionner un chemin computationnel différent pour B.

Par exemple :

```text
continuer normalement
effectuer une vérification supplémentaire
réduire la vérification
arrêter la génération
régénérer
rerouter
demander une revue humaine
s’abstenir
```

Cependant, cette action résulte de **la politique définie par C**, et non d’une autorité universelle intrinsèque au signal NeoMundi.

---

## 19. Réduction computationnelle et Token ROI

Les mesures NeoMundi **PEUVENT** être utilisées pour étudier ou implémenter une réduction computationnelle.

Par exemple :

```text
dégradation runtime détectée
        ↓
politique d’arrêt du consommateur
        ↓
génération interrompue
        ↓
tokens évités
```

Une étude exploratoire NeoMundi a défini :

```text
E-token = TotalTokens - TokenDetection
```

où `TokenDetection` correspond à la position du premier token répondant au critère expérimental de dégradation.

Cette métrique estime le nombre théorique de tokens qui auraient pu être évités avec une politique d’arrêt anticipé.

La recherche démontre donc un **potentiel d’actionnabilité**.

Elle n’établit pas une règle universelle d’arrêt.

---

## 20. Réduction de la vérification

Une mesure NeoMundi **PEUT** soutenir une politique consommateur réduisant la vérification ou sélectionnant un chemin computationnel plus court.

Cependant :

> **Aucune règle générale NeoMundi actuelle ne stipule qu’un G élevé, un état STABLE, un état ALLOW ou tout autre signal unique autorise automatiquement une réduction de la vérification.**

Pour une expérience visant à mesurer un Token ROI associé à une réduction de vérification, la règle correspondante **DOIT** donc être définie explicitement dans le profil de politique consommateur.

Exemple :

```text
mesure NeoMundi
+
conditions de politique C satisfaites
+
signaux requis présents et valides
=
chemin de vérification réduit autorisé
```

À défaut de politique explicite :

```text
hypothèse par défaut = aucune réduction automatique
```

---

## 21. CONTINUE, STOP et ABSTAIN

`CONTINUE`, `STOP` et `ABSTAIN` **DEVRAIENT** être considérés comme des **états d’action du consommateur**, sauf si un futur schéma NeoMundi les expose explicitement comme sorties de mesure.

### CONTINUE

Le consommateur poursuit le chemin computationnel actuel.

### STOP

Le consommateur termine ou interrompt le chemin computationnel.

### ABSTAIN

Le consommateur refuse de prendre une décision opérationnelle sur la base des mesures disponibles et transfère le contrôle vers un autre chemin.

Exemples :

```text
ABSTAIN -> demander davantage de preuves
ABSTAIN -> politique de repli
ABSTAIN -> revue humaine
ABSTAIN -> comportement sûr par défaut
```

Ces états **NE DOIVENT PAS** être confondus avec :

```text
ALLOW
FLAG
STABLE
DROP
```

car les deux catégories appartiennent à des couches distinctes :

```text
ALLOW / FLAG / STABLE / DROP
        =
états de mesure / d’interprétation

CONTINUE / STOP / ABSTAIN
        =
états d’action du consommateur
```

---

## 22. Résolution des conflits

Des signaux apparemment contradictoires sont possibles dans un système de mesure multidimensionnel.

Exemple :

```text
G = élevé
decision = ALLOW
signal factuel = faible
```

Cela ne constitue pas nécessairement une contradiction interne.

Cela peut indiquer :

```text
génération stable
+
faiblesse factuelle
```

NeoMundi ne réduit donc pas systématiquement les signaux à un verdict scalaire unique.

Principe général :

```text
chaque signal conserve sa dimension sémantique
```

plutôt que :

```text
tous les signaux sont fusionnés en une signification unique
```

Les règles de priorité propres au consommateur **DOIVENT** être explicites.

Elles **NE DOIVENT PAS** être reconstruites à partir de simples corrélations observées dans des jeux de données expérimentaux.

---

## 23. Règles de vérification

NeoMundi distingue mesure et validation.

Une vérification **DEVRAIT** être déclenchée selon les exigences de l’application consommatrice.

Exemples de situations pouvant justifier une vérification :

* `FLAG` ;
* `DROP` ;
* dégradation significative de ∆G ;
* désaccord entre plusieurs signaux runtime ;
* G élevé accompagné d’un doute factuel externe ;
* données périmées ou incomplètes ;
* contexte applicatif à haut risque ;
* conditions définies par la politique du consommateur.

Aucun signal de stabilité runtime NeoMundi ne prouve, à lui seul, la véracité factuelle.

---

## 24. Signaux manquants

Un consommateur **DOIT** distinguer :

```text
signal absent
```

de :

```text
signal observé avec une valeur neutre ou stable
```

Une donnée manquante **NE DOIT PAS** être silencieusement convertie en :

```text
0
ALLOW
STABLE
sûr
vérifié
```

La politique de fallback faisant autorité pour chaque champ requis **DOIT** être définie dans le schéma ou le profil consommateur.

Tant qu’elle n’est pas explicitement versionnée, l’absence d’une mesure nécessaire **DEVRAIT** être interprétée comme :

```text
mesure indisponible
```

et non comme un résultat positif.

---

## 25. Payloads malformés

Un payload malformé **NE DOIT PAS** être interprété comme une mesure NeoMundi valide.

Le consommateur **DEVRAIT** :

```text
refuser l’interprétation de la mesure
journaliser l’échec de validation
conserver la provenance
appliquer sa politique de repli
```

Un payload malformé **NE DOIT PAS** produire silencieusement `ALLOW`.

---

## 26. Payloads partiels

Les environnements streaming peuvent exposer un état partiel avant la fin de la génération.

Un consommateur **DOIT** distinguer :

```text
observation runtime partielle
```

de :

```text
mesure finale
```

Les champs correspondant à une agrégation finale ou à une classification finale **NE DOIVENT PAS** être considérés comme valides avant l’événement de fin défini par le schéma.

---

## 27. Signaux périmés

Les mesures runtime sont liées temporellement et causalement à une génération.

Un consommateur **NE DOIT PAS** appliquer un signal à une génération ou à un tour différent de celui dont il provient.

Chaque signal consommé **DEVRAIT** disposer d’une provenance suffisante pour identifier :

```text
requête
génération
tour
timestamp
version de mesure
```

Un signal qui ne peut pas être relié causalement à la décision en cours **DEVRAIT** être considéré comme indisponible.

---

## 28. Causalité entre tours

Règle conceptuelle par défaut :

```text
measurement(t) s’applique à la génération / au tour qui l’a produit
```

Un signal du tour `t-1` **NE DOIT PAS** automatiquement gouverner le tour `t`.

Toute réutilisation inter-tour nécessite une politique longitudinale explicite du consommateur.

Le protocole faisant autorité **DEVRAIT** permettre de relier chaque signal à des identifiants tels que :

```text
conversation_id
turn_id
generation_id
measurement_id
timestamp
```

Les noms de champs exacts relèvent du schéma d’interopérabilité officiel.

---

## 29. Durée de validité d’un signal

NeoMundi ne définit pas nécessairement un TTL temporel universel indépendant de la causalité.

La règle privilégiée est une validité fondée sur l’identité :

```text
le signal est valide pour la génération qu’il mesure
```

plutôt que :

```text
le signal est valide pendant N secondes arbitraires
```

Toute persistance plus longue ou utilisation longitudinale **DOIT** être définie par l’application consommatrice.

---

## 30. Versioning

Chaque mesure NeoMundi destinée à un usage de production **DEVRAIT** exposer suffisamment d’informations de version pour déterminer son contrat d’interprétation.

Le modèle de versioning **DEVRAIT** distinguer :

```text
schema_version
metric_version
normalizer_version
```

Ces versions ont des rôles différents.

### `schema_version`

Structure et sérialisation du payload.

### `metric_version`

Définition et sémantique de la métrique.

### `normalizer_version`

Transformation ou calibration appliquée à la mesure brute ou intermédiaire.

Une modification de l’une **NE DOIT PAS** être silencieusement présentée comme une modification d’une autre.

---

## 31. Compatibilité

Les consommateurs **DEVRAIENT** utiliser des règles de compatibilité explicites.

Conceptuellement :

```text
même version majeure du schéma
+
sémantique métrique compatible
+
normaliseur reconnu
=
mesure consommable
```

Un consommateur **NE DOIT PAS** traiter silencieusement une version inconnue comme sémantiquement identique à une version connue.

Une version incompatible ou inconnue **DEVRAIT** déclencher le fallback prévu.

---

## 32. G, `g_score`, `g_final`, `stability_score` et `delta_g`

Ces identifiants **NE DOIVENT PAS** être considérés comme synonymes sauf définition explicite dans la version métrique correspondante.

Hiérarchie conceptuelle actuelle :

```text
G / g_score
     │
     ├── mesure associée à la stabilité runtime
     │
     └── évolution au cours de la génération
              │
              ▼
             ∆G
```

`stability_score` représente un score de stabilité global au niveau de la génération.

La relation mathématique exacte entre :

```text
G
g_score
g_final
stability_score
delta_g
```

**n’est pas entièrement définie par les rapports exploratoires externes actuellement disponibles.**

Par conséquent :

> Les consommateurs **NE DOIVENT PAS inventer** de conversions, d’équivalences, de coefficients ou de formules de normalisation entre ces champs.

La relation faisant autorité **DOIT** être fournie par le contrat d’implémentation de la métrique NeoMundi versionnée.

---

## 33. Formules internes

Le Metric Contract distingue :

```text
sémantique nécessaire au consommateur
```

de :

```text
calcul interne
```

Un consommateur n’a pas nécessairement besoin d’accéder à l’ensemble des formules propriétaires internes pour utiliser correctement la mesure.

Lorsqu’un mécanisme reste propriétaire, NeoMundi **PEUT** exposer à la place :

* la sémantique de sortie ;
* le domaine et la plage ;
* l’identifiant de calibration ou de version ;
* le sens d’interprétation ;
* les limites et incertitudes ;
* les invariants nécessaires à l’interopérabilité.

Un consommateur **NE DEVRAIT PAS** tenter de reconstruire des coefficients internes manquants à partir des sorties empiriques.

---

## 34. Normalisation

La normalisation fait partie de la définition d’une métrique.

Lorsqu’une métrique est normalisée, son contrat **DEVRAIT** spécifier :

```text
domaine d’entrée
domaine de sortie
normalizer_version
direction d’interprétation
comportement aux bornes
comportement en cas d’entrée invalide
```

Les consommateurs **NE DOIVENT PAS** renormaliser indépendamment les valeurs NeoMundi tout en continuant à les présenter comme des signaux NeoMundi canoniques, sauf autorisation explicite du contrat.

---

## 35. Payload ENF canonique

Le payload ENF faisant autorité **DOIT** être défini par un JSON Schema lisible par machine et stocké dans le repository.

Emplacement recommandé :

```text
/schema/enf-runtime.schema.json
```

Le présent document décrit la sémantique.

Le JSON Schema définit la sérialisation.

Le Metric Contract définit la signification.

Le contrat d’interopérabilité définit l’échange.

Ces couches **DEVRAIENT** rester distinctes.

---

## 36. Exemple conceptuel de payload

L’exemple suivant est **illustratif et non autoritatif** tant qu’il n’est pas aligné avec le JSON Schema versionné.

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

Les consommateurs **DOIVENT** utiliser le JSON Schema comme autorité de sérialisation, et non cet exemple.

---

## 37. Exemples de scénarios consommateur

### Scénario A — Mesure runtime stable

```text
stability_score = élevé
delta_profile = FLAT
decision = ALLOW
```

Signification NeoMundi :

```text
la génération apparaît runtime-stable selon la mesure applicable
```

Non impliqué :

```text
factuellement vrai
sûr
approuvé
vérification inutile
```

Comportement possible de C :

```text
CONTINUE
```

si la politique C l’autorise explicitement.

### Scénario B — Dégradation runtime

```text
delta_profile = DROP
decision = FLAG
```

Signification NeoMundi :

```text
dégradation runtime détectée
```

Comportements possibles de C :

```text
VERIFY
STOP
REGENERATE
REROUTE
HUMAN_REVIEW
```

selon la politique du consommateur.

### Scénario C — Stabilité trompeuse

```text
G = élevé
état runtime = stable
validation factuelle externe = échec
```

Interprétation :

```text
stable mais incorrect
```

Comportement possible :

```text
activer la couche factuelle
demander une vérification
ne pas utiliser la stabilité comme preuve factuelle
```

### Scénario D — Mesure manquante

```text
signal NeoMundi requis indisponible
```

Interprétation :

```text
mesure indisponible
```

et non :

```text
ALLOW
```

Comportement possible :

```text
ABSTAIN
fallback
vérification complète
```

selon la politique du consommateur.

---

## 38. Profil de politique consommateur

Pour rendre les expériences reproductibles, les systèmes consommant NeoMundi **DEVRAIENT** publier un profil de politique distinct.

Exemple :

```text
C Policy Profile v1.0
```

contenant :

```text
signaux d’entrée
signaux obligatoires
signaux optionnels
règles de vérification
règles de réduction de vérification
règles d’arrêt anticipé
règles de fallback
règles de priorité
mapping vers les actions
```

Cela évite de confondre la politique du consommateur avec la mesure elle-même.

---

## 39. Politiques expérimentales

Une expérience **PEUT** définir une politique telle que :

```text
si condition X :
    STOP
```

ou :

```text
si condition Y :
    réduire la vérification
```

à condition que cette règle soit clairement identifiée comme :

```text
POLITIQUE CONSOMMATEUR SPÉCIFIQUE À L’EXPÉRIENCE
```

et non comme :

```text
sémantique universelle de la mesure NeoMundi
```

Cette distinction est essentielle pour la reproductibilité.

---

## 40. Expériences Token ROI

Pour mesurer le Token ROI, trois objets **DEVRAIENT** rester distincts :

```text
mesure
politique
économie
```

Exemple :

```text
NeoMundi détecte un événement au token T
        ↓
la politique C choisit STOP
        ↓
la génération s’arrête à T
        ↓
calcul du Token ROI
```

Le benchmark évalue donc :

> **la valeur de la consommation de la mesure NeoMundi selon une politique donnée**

et non :

> **une propriété selon laquelle NeoMundi arrêterait universellement les générations.**

---

## 41. Résultats de recherche et règles de protocole

Les recherches NeoMundi peuvent mettre en évidence des relations statistiquement ou opérationnellement fortes.

Par exemple :

* corrélation entre DROP et FLAG ;
* `stability_score` plus faible dans certains cas FLAG ;
* différences selon les catégories de prompts ;
* détection précoce d’une dégradation ;
* économies potentielles de tokens.

Ces observations peuvent éclairer de futures versions du protocole.

Elles ne deviennent pas automatiquement des règles contractuelles.

Cycle attendu :

```text
observation
    ↓
réplication
    ↓
interprétation
    ↓
décision contractuelle
    ↓
règle normative versionnée
```

---

## 42. Base de recherche

Le présent document est notamment informé par des travaux exploratoires externes.

### Pape Malick DIOP

**Audit exploratoire des signaux runtime NeoMundi — Analyse de G, ∆G, FLAG, régime et exactitude benchmark**

Contributions principales :

* séparation entre stabilité runtime et exactitude factuelle ;
* stabilité trompeuse ;
* lecture multi-signaux ;
* interprétation de G, ∆G, FLAG, DROP et régime.

### Fatima Ezzahrae GOUARAB

**Actionability of the NeoMundi ControlTower Runtime Signal — Evaluation of a Governance Policy Based on Early Stopping of Generation**

Contributions principales :

* actionnabilité runtime ;
* détection précoce ;
* E-token ;
* politique expérimentale d’early stopping ;
* distinction entre seuil expérimental et seuil officiel.

### Fatima Ezzahrae GOUARAB

**Stabilité et reproductibilité du signal runtime NeoMundi ControlTower — Analyse comparative multi-providers, multi-corpus et multi-configurations**

Contributions principales :

* réplication multi-provider ;
* influence de la nature des tâches et des prompts ;
* reproductibilité de certaines relations entre signaux ;
* forte correspondance empirique entre DROP et FLAG dans les configurations testées.

Ces études constituent des **éléments de preuve et d’interprétation**.

Elles ne remplacent pas le Metric Contract versionné.

---

## 43. Ce que ce document ne prétend pas

Ce document ne prétend pas que :

```text
G mesure la vérité
FLAG prouve une hallucination
ALLOW prouve l’exactitude
DROP équivaut universellement à FLAG
STABLE autorise la suppression d’une vérification
∆G < 0 est le seuil universel d’arrêt NeoMundi
```

Il n’exige pas non plus la publication des mécanismes internes propriétaires lorsqu’ils ne sont pas nécessaires à l’interopérabilité.

---

## 44. Freeze d’implémentation

Une implémentation consommatrice ne peut être considérée comme **figée par rapport à NeoMundi** que lorsque sont fixés :

```text
schema_version
metric_version
normalizer_version
consumer_policy_version
```

Le registre de freeze **DEVRAIT** donc identifier :

```text
NeoMundi Metric Contract : X
ENF Schema : Y
Normalizer : Z
Consumer Policy C : N
```

Cela permet à NeoMundi d’évoluer ultérieurement sans modifier silencieusement un benchmark déjà en cours.

---

## 45. Artifacts nécessaires à un freeze complet

Un package d’implémentation complet **DEVRAIT** contenir :

```text
README.md
METRIC_CONTRACT.md
schema/enf-runtime.schema.json
schema/examples/
consumer/C_POLICY_PROFILE.md
VERSIONING.md
CHANGELOG.md
```

Fixtures recommandées :

```text
allow-flat.json
flag-drop.json
stable-factual-failure.json
partial-payload.json
missing-signal.json
malformed-payload.json
unknown-version.json
```

Chaque fixture **DEVRAIT** indiquer le comportement consommateur attendu.

---

## 46. Structure recommandée du repository

```text
neomundi-metric-contract/
│
├── README.md
├── metric-contract-v0.0.en.md
├── metric-contract-v0.0.fr.md
│
├── signal_interpretation_and_consumption_rules.en.md
├── signal_interpretation_and_consumption_rules.fr.md
│
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
└── docs/
    └── SIGNAL_DICTIONARY.md
```

---

## 47. Principe canonique

L’architecture NeoMundi peut se résumer ainsi :

```text
Nous mesurons.
Vous interprétez.
Vous gouvernez.
Vous agissez.
```

Ou, techniquement :

```text
Mesure ≠ Politique ≠ Exécution
```

NeoMundi fournit la couche indépendante de mesure runtime.

L’infrastructure environnante conserve le contrôle de l’interprétation, de la politique et de l’action.

---

## 48. État actuel du contrat

Les éléments suivants sont suffisamment documentés pour soutenir un travail d’implémentation :

* stabilité runtime comme dimension de mesure distincte ;
* `stability_score` ;
* `ALLOW / FLAG` ;
* `DROP / FLAT / V_SHAPE` ;
* trajectoire ∆G ;
* `delta_series` ;
* `delta_variation` ;
* signaux complémentaires factuels et de cohérence ;
* séparation entre mesure et action du consommateur ;
* possibilité d’un early stopping ou d’autres chemins computationnels côté consommateur.

Les éléments suivants nécessitent encore un **freeze normatif explicite** avant de pouvoir être considérés comme canoniques :

* relation exacte entre `G`, `g_score`, `g_final`, `stability_score` et `delta_g` ;
* définition de la normalisation lorsque celle-ci doit être connue du consommateur ;
* JSON Schema ENF faisant autorité ;
* ensemble exhaustif des enums ;
* seuils numériques officiels, s’il en existe ;
* matrice exacte de compatibilité ;
* distinction exhaustive champs obligatoires / optionnels ;
* comportement canonique face aux payloads malformés, partiels ou périmés ;
* éventuel profil NeoMundi autorisant explicitement une réduction de vérification.

Tant que ces éléments ne sont pas versionnés, les consommateurs **NE DOIVENT PAS les déduire**.

---

## 49. Philosophie du contrat

NeoMundi conserve volontairement une séparation entre :

```text
ce qui a été mesuré
```

et :

```text
ce qui doit être fait
```

Cette séparation permet à une même primitive de mesure d’être consommée par différentes infrastructures sans leur imposer un modèle unique de gouvernance.

**Une couche de mesure.
Plusieurs applications.
Plusieurs politiques.
Plusieurs infrastructures.
Sans modifier la mesure elle-même.**

---

**NeoMundi Recherche**
**Mesure runtime des systèmes d’intelligence artificielle**
