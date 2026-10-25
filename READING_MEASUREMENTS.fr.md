# Comprendre les mesures
[English](READING_MEASUREMENTS.md) · [Français](READING_MEASUREMENTS.fr.md)

## Lire un résultat
Lire ensemble la valeur, son état, sa couverture, ses versions et les conditions
qui l'ont produite. `null` ne signifie pas zéro. Un nom identique ne suffit pas
à établir que deux résultats sont calculés de la même manière.

## Que veut dire G ?
Les anciens documents décrivent G comme un indicateur lié à la stabilité
runtime. Ils ne constituent pas, à eux seuls, une spécification actuelle de
sa construction. Les liens ci-dessous sont conservés comme références historiques.

`G`, `g_score`, `g_final` et `stability_score` doivent être distingués.
Leur correspondance, les entrées agrégées et la normalisation doivent être
documentées pour une **version précise du moteur**. Ne pas déduire une formule
générale du seul nom d'un champ ou d'un ancien document.

Le [tableau d'interprétation de l'API](https://github.com/neomundi-io/neomundi-runtime-measurement/blob/main/docs/MEASUREMENT_INTERPRETATION_TABLE.md)
distingue vocabulaire normatif, expérimental et illustratif.
Ce guide n'élève pas ces descriptions au statut de validation scientifique.

## Ce qu'une fiche de calcul doit préciser
| Question | Information nécessaire |
|---|---|
| Que mesure-t-on ? | Objet, unité, plage et sens de variation |
| Qu'est-ce qui entre dans le calcul ? | Signaux élémentaires et origine locale, déclarée ou évaluée |
| Qu'agrège-t-on ? | Composants inclus, exclus, dépendances et normalisation |
| Que fait-on lorsqu'une entrée manque ? | Règles de disponibilité, calcul partiel, refus et propagation de null |
| Quelle formule s'applique ? | Expression versionnée, coefficients applicables, bornage et configuration |
| Comment le vérifier ? | Exemples de référence recalculables et sorties attendues |
| Avec quelles limites ? | Protocole, domaine testé, incertitudes et éléments non établis |

Expliquer le sens et les agrégations est nécessaire pour une lecture utile.
La publication de formules exactes doit être rapprochée du code réellement
déployé, de sa version et des droits de diffusion. Cette page n'annonce pas
qu'une formule complète et reproductible est déjà publiée.

## Où lire aujourd'hui
- [Contrat sémantique FR](metric-contract-v0.0.fr.md) · [EN](metric-contract-v0.0.en.md) — Draft v0.0.
- [Cadre de référence FR](measurement_reference_framework_fr.md) · [EN](measurement_reference_framework_en.md).
- [Interprétation et consommation FR](signal_interpretation_and_consumption_rules.fr.md) · [EN](signal_interpretation_and_consumption_rules.en.md).
- [Qualification et méthodes](https://github.com/neomundi-io/neomundi-metrology-validation).
- [Ancienne télémétrie](https://github.com/neomundi-io/runtime-telemetry-signals) — référence historique, pas spécification de production actuelle.
- [API](https://github.com/neomundi-io/neomundi-runtime-measurement) · [Station](https://github.com/neomundi-io/neomundi-station-public).

Une mesure n'accorde pas de permission d'exécution. L'interprétation et la
décision restent celles du système consommateur.
