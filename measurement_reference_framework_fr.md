# Cadre de référence de la mesure NeoMundi

**Version :** 0.1
**Statut :** Draft
**Mainteneur :** NeoMundi
**Périmètre :** Cadre de référence pour la mesure, à l’exécution, du comportement observable des systèmes d’IA

---

## 1. Objet

Le Cadre de référence de la mesure NeoMundi définit les principes selon lesquels une mesure NeoMundi peut être produite, reproduite, transportée entre différentes infrastructures, interprétée indépendamment et comparée dans le temps.

Il complète le Contrat métrique NeoMundi.

Le **Contrat métrique** définit ce que signifient les mesures et signaux NeoMundi.

Le **Cadre de référence de la mesure** définit les conditions dans lesquelles ces mesures conservent un sens interprétable à travers des observations répétées, différents systèmes d’IA, différentes infrastructures techniques et différents usages en aval.

Ce cadre ne définit pas :

* une décision de politique ;
* une décision d’autorisation ;
* une action d’exécution ;
* un verdict de conformité ;
* une certification de sûreté ;
* une affirmation de vérité factuelle.

NeoMundi fournit la mesure.

L’infrastructure qui consomme cette mesure reste responsable de son interprétation, de sa politique et de ses actions.

---

## 2. Objet de la mesure

L’objet principal de la mesure est le **comportement observable d’un système d’IA à l’exécution**.

NeoMundi ne suppose pas disposer d’un accès direct :

* aux poids du modèle ;
* au raisonnement interne ;
* aux données d’entraînement ;
* aux états cachés ;
* à l’architecture propriétaire ;
* à l’infrastructure interne du fournisseur.

La mesure porte donc sur ce qui peut être observé lors de l’exécution du système dans des conditions définies.

Selon le protocole, les propriétés observables peuvent notamment inclure :

* la variation des réponses ;
* la cohérence ;
* la récurrence ;
* des signaux factuels ;
* la latence ;
* la disponibilité ;
* l’évolution comportementale dans le temps ;
* les changements entre observations répétées ;
* les changements entre versions, fournisseurs ou environnements d’exécution.

Une mesure décrit une observation de comportement.

Elle n’explique pas, à elle seule, la cause interne de ce comportement.

---

## 3. Principe de référence

Une mesure NeoMundi n’est pas définie par la décision qu’elle produit.

Elle est définie par les conditions dans lesquelles son sens peut rester suffisamment stable à travers :

* des observations répétées ;
* différents moments d’exécution ;
* différents systèmes d’IA ;
* différents fournisseurs ;
* différentes infrastructures ;
* différents consommateurs en aval ;
* différentes interprétations indépendantes.

Le cadre de référence sépare donc quatre couches :

**Observation → Mesure → Interprétation → Action**

NeoMundi intervient principalement dans les couches d’observation et de mesure.

L’interprétation et l’action restent externes, sauf lorsqu’elles sont explicitement mises en œuvre par un autre système.

Cette séparation fait partie de la frontière de mesure.

---

## 4. Invariance sémantique

Une métrique doit conserver le même sens défini indépendamment de l’infrastructure qui la consomme.

Par exemple, si un signal NeoMundi représente un degré mesuré de cohérence comportementale, sa définition sémantique ne doit pas changer parce qu’une infrastructure l’utilise pour :

* la supervision ;
* l’audit ;
* l’escalade vers un humain ;
* l’analyse assurantielle ;
* la gouvernance ;
* la cybersécurité ;
* la comparaison de modèles ;
* le contrôle opérationnel.

Différents systèmes peuvent associer des conséquences différentes à une même mesure.

La définition de la mesure, elle, doit rester inchangée.

Ce principe peut être exprimé ainsi :

**même primitive de mesure → contrat sémantique stable → interprétations multiples → actions multiples**

La portabilité sémantique est donc distincte de la portabilité décisionnelle.

NeoMundi n’exige pas que des systèmes en aval prennent la même décision à partir de la même mesure.

---

## 5. Reproductibilité

Un cadre de mesure doit rendre la reproduction possible lorsque les systèmes sous-jacents et les conditions d’exécution le permettent.

Une observation NeoMundi reproductible doit donc préserver, lorsque cela est techniquement disponible et pertinent :

* l’identifiant du protocole ;
* la version du protocole ;
* la définition de la métrique ;
* la version du schéma de mesure ;
* l’identifiant de l’observation ;
* l’horodatage de l’exécution ;
* le fournisseur d’IA ;
* l’identifiant du modèle ;
* la version du modèle lorsqu’elle est disponible ;
* la référence de l’entrée ou du prompt ;
* les paramètres d’exécution lorsqu’ils sont disponibles ;
* le nombre de répétitions ;
* la méthode de scoring ;
* la version du logiciel ou du launcher ;
* le résultat de la mesure ;
* les limites connues.

La reproductibilité ne signifie pas qu’un système d’IA doit produire exactement la même réponse.

Pour des systèmes stochastiques ou évolutifs, la non-identité des sorties peut précisément constituer l’objet de la mesure.

L’exigence porte sur le fait que **la procédure de mesure** puisse être reproduite et que ses conditions puissent être inspectées.

---

## 6. Répétabilité et observation longitudinale

Le comportement d’un système d’IA à l’exécution peut varier même lorsque l’entrée semble inchangée.

Pour cette raison, une exécution unique ne doit pas être automatiquement considérée comme représentative de l’état comportemental du système.

Les protocoles de mesure NeoMundi peuvent donc utiliser des observations répétées.

La répétition de la mesure permet notamment de distinguer :

* une variation isolée ;
* un comportement persistant ;
* des motifs récurrents ;
* des changements de régime ;
* une dérive longitudinale ;
* des anomalies temporaires.

L’objectif n’est pas d’imposer un comportement déterministe.

L’objectif est de rendre la variation comportementale observable.

Lorsqu’une comparaison longitudinale est réalisée, la continuité du protocole et la traçabilité des versions sont nécessaires afin de distinguer une évolution du processus de mesure d’une évolution du système observé.

---

## 7. Portabilité entre infrastructures

Une couche de mesure devient plus utile lorsque sa sortie peut être consommée sans imposer à l’infrastructure en aval le modèle de politique du fournisseur de la mesure.

NeoMundi cherche donc à exposer ses mesures au moyen de contrats explicites et portables.

Une infrastructure en aval peut consommer le même signal de mesure pour des finalités différentes.

Par exemple :

```text
Système d’IA
    ↓
Observation NeoMundi
    ↓
Signal de mesure NeoMundi
    ↓
Infrastructure externe
    ↓
Interprétation / politique
    ↓
Décision / exécution / preuve
```

L’infrastructure externe reste en contrôle des couches finales.

Cela permet à une même primitive de mesure de participer à des systèmes hétérogènes sans exiger que ces systèmes deviennent des systèmes NeoMundi.

---

## 8. Interprétation indépendante

La mesure et l’interprétation doivent rester distinguables.

Deux infrastructures indépendantes peuvent recevoir la même mesure NeoMundi et parvenir à des conclusions différentes.

Cela ne constitue pas nécessairement un échec de la mesure.

Cela peut refléter :

* des niveaux de tolérance au risque différents ;
* des politiques différentes ;
* des environnements réglementaires différents ;
* des contextes opérationnels différents ;
* des seuils de décision différents ;
* des objectifs métier différents.

NeoMundi distingue donc :

**ce qui a été observé**

de

**ce que cette observation signifie pour un acteur donné**

et de

**ce que cet acteur décide ensuite de faire**.

Cette séparation empêche une métrique d’exécution de devenir silencieusement une autorité de décision.

---

## 9. La mesure n’est pas la vérité

Un signal de mesure n’est pas équivalent à une vérité factuelle.

Un système peut paraître stable tout en étant faux.

Un système peut être cohérent en interne tout en répétant la même information erronée.

Inversement, la variation entre plusieurs réponses ne signifie pas nécessairement qu’il y a défaillance.

Pour cette raison, NeoMundi évite de traiter une métrique isolée comme un verdict universel.

Lorsqu’une évaluation factuelle est incluse dans un protocole, la méthode d’évaluation factuelle doit elle-même être identifiable et soumise à ses propres limites.

Les signaux de mesure doivent donc être interprétés comme des éléments de preuve concernant un comportement observable, et non comme des affirmations autosuffisantes de vérité, de sûreté ou de conformité.

---

## 10. Mesures indépendantes multiples

Les systèmes d’IA exposent plusieurs dimensions comportementales.

Aucune métrique unique à l’exécution ne doit être automatiquement considérée comme représentative de l’ensemble de l’état comportemental d’un système.

Lorsque cela est pertinent, NeoMundi peut combiner ou exposer plusieurs mesures indépendantes tout en préservant leur signification propre.

Cela permet aux systèmes en aval de raisonner à partir de plusieurs signaux plutôt qu’à partir d’un score composite unique.

Le cadre privilégie :

* des signaux explicites ;
* des relations documentées ;
* une incertitude visible ;
* des transformations traçables ;

plutôt qu’une agrégation opaque.

Lorsqu’un indicateur composite est utilisé, il doit rester traçable jusqu’aux mesures qui le composent.

---

## 11. Traçabilité

Une mesure doit être associée à une provenance suffisante pour permettre son inspection ultérieure.

Une observation NeoMundi peut donc inclure ou référencer :

* le moment où l’observation a eu lieu ;
* le système observé ;
* le protocole utilisé ;
* les définitions métriques utilisées ;
* la version du logiciel ;
* les entrées utilisées ;
* les limites connues ;
* la manière dont le signal résultant a été produit.

La traçabilité permet de réexaminer les mesures après l’exécution.

Elle permet également de comparer ultérieurement des observations produites dans des conditions différentes.

---

## 12. Versionnage

Les fournisseurs d’IA évoluent.

Les modèles évoluent.

Les API évoluent.

Les logiciels de mesure évoluent.

Les métriques peuvent également évoluer.

Le versionnage fait donc partie du cadre de référence.

Une mesure doit, lorsque cela est possible, permettre de distinguer les changements concernant :

* le système d’IA observé ;
* le protocole d’observation ;
* la méthode de scoring ;
* la définition de la métrique ;
* le schéma de sortie ;
* le logiciel de mesure.

Une observation historique doit rester interprétable selon le contrat et la version du protocole sous lesquels elle a été produite.

Une nouvelle version ne doit pas redéfinir silencieusement les mesures passées.

---

## 13. Falsifiabilité et réplication externe

Un cadre de mesure doit pouvoir être remis en question de manière indépendante.

Lorsque cela est possible, NeoMundi cherche à rendre disponibles les éléments nécessaires pour permettre à des tiers de :

* reproduire un protocole ;
* inspecter une définition métrique ;
* comparer des observations indépendantes ;
* identifier les désaccords ;
* tester d’autres interprétations ;
* signaler les échecs ou les limites.

Le désaccord externe n’est pas exclu du cadre.

Il fait partie du processus par lequel une discipline de mesure peut être testée et améliorée.

Une affirmation de mesure qui ne peut, en principe, ni être remise en question ni reproduite ne doit pas être considérée comme une preuve métrologique forte.

---

## 14. Limites connues

La mesure comportementale à l’exécution comporte des limites intrinsèques.

Parmi elles :

* les systèmes côté fournisseur peuvent évoluer sans préavis ;
* les versions des modèles ne sont pas toujours totalement exposées ;
* les systèmes stochastiques peuvent produire des résultats différents dans des conditions nominalement équivalentes ;
* les conditions réseau peuvent affecter les mesures de latence ;
* les évaluateurs externes peuvent introduire leur propre incertitude ;
* les prompts et jeux de données peuvent contenir des biais ou des ambiguïtés ;
* le comportement observable ne permet pas d’inférer toutes les propriétés internes d’un système ;
* les mesures restent dépendantes du périmètre du protocole utilisé.

Ces limites doivent être documentées plutôt que masquées.

Une mesure est pertinente à l’intérieur de sa frontière déclarée.

Elle ne doit pas être généralisée au-delà de cette frontière sans éléments de preuve supplémentaires.

---

## 15. Accumulation des preuves

La solidité d’un cadre de mesure ne provient pas uniquement de sa spécification.

Elle provient également de son usage répété.

Les preuves peuvent s’accumuler à travers :

* des expériences répétées ;
* des observations longitudinales ;
* des mesures multi-fournisseurs ;
* des réplications indépendantes ;
* des intégrations externes ;
* des infrastructures aval hétérogènes ;
* des désaccords documentés ;
* des échecs documentés ;
* des révisions de protocoles.

Le fait que différentes infrastructures puissent consommer le même objet de mesure sans nécessiter de modification de sa définition sémantique constitue un élément de preuve en faveur de sa portabilité.

Cela ne constitue pas, à lui seul, une preuve de validité universelle.

---

## 16. Architecture de référence

La frontière de référence NeoMundi peut être représentée ainsi :

```text
┌───────────────────────────────┐
│        Système d’IA           │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│     Exécution observable      │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│       Mesure NeoMundi         │
│                               │
│ observation                   │
│ métriques                     │
│ signaux                       │
│ provenance                    │
└───────────────┬───────────────┘
                │
         frontière de mesure
                │
                ▼
┌───────────────────────────────┐
│    Infrastructure en aval     │
│                               │
│ interprétation                │
│ politique                     │
│ autorisation                  │
│ décision                      │
│ exécution                     │
│ reporting                     │
└───────────────────────────────┘
```

Cette frontière est intentionnelle.

NeoMundi fournit le signal.

L’infrastructure qui le consomme détermine ce que ce signal signifie dans son propre contexte.

---

## 17. Relation avec le Contrat métrique

Ce cadre et le Contrat métrique NeoMundi remplissent des fonctions différentes.

### Contrat métrique 

Il définit :

* la sémantique des métriques ;
* les définitions des signaux ;
* les champs attendus ;
* les limites d’interprétation ;
* les limites connues.

### Cadre de référence de la mesure

Il définit :

* l’objet de la mesure ;
* les frontières de mesure ;
* les principes de reproductibilité ;
* les principes de portabilité ;
* l’indépendance de l’interprétation ;
* les exigences de traçabilité ;
* les principes de versionnage ;
* les principes de falsifiabilité et de réplication.

Ensemble, ils constituent la base sémantique et méthodologique de la couche de mesure runtime NeoMundi.

---

## 18. Énoncé de référence

L’approche de mesure NeoMundi peut être résumée ainsi :

> Une mesure NeoMundi est une observation du comportement d’un système d’IA à l’exécution, produite selon un protocole explicite et un contrat sémantique. Son sens doit rester traçable à travers les répétitions, le temps, les systèmes et les infrastructures qui la consomment. La mesure informe l’interprétation et l’action en aval, mais ne constitue pas, à elle seule, une politique, une autorisation, une exécution, une certification ou une vérité.

---

## 19. Statut

Ce document est un **Draft v0.1**.

Il est destiné à évoluer à partir :

* des résultats expérimentaux ;
* des réplications externes ;
* des pilotes d’interopérabilité ;
* des revues indépendantes ;
* des retours d’implémentation ;
* de l’évolution du Contrat métrique NeoMundi.

Toute modification de ce cadre doit être versionnée et documentée.

---

**NeoMundi**
*Mesure indépendante du comportement des systèmes d’IA à l’exécution.*
