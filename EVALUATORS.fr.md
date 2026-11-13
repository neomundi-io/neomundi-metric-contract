# Comprendre les évaluateurs NeoMundi

[English](EVALUATORS.md) · [Français](EVALUATORS.fr.md)

**Statut :** guide explicatif, 13 novembre 2026. **Ce n'est ni** un inventaire validé des évaluateurs actuellement actifs en production, **ni** un nouveau schéma de mesure, **ni** une attestation de calibration scientifique.

## Rôle des évaluateurs

Un évaluateur contribue à produire un signal à partir d'une observation et de conditions d'exécution données. **Le signal**, **l'évaluateur qui le produit** et **l'agrégat** éventuellement dérivé sont trois objets distincts. Une valeur numérique n'est pas automatiquement une probabilité, un verdict de vérité ou un intervalle de confiance calibré.

Le Metric Contract définit le sens des signaux ; le service de mesure publie valeurs et provenance ; [Metrology Validation](https://github.com/neomundi-io/neomundi-metrology-validation) documente les expériences. Consulter le [tableau d'interprétation API](https://github.com/neomundi-io/neomundi-runtime-measurement/blob/main/docs/MEASUREMENT_INTERPRETATION_TABLE.md) et la réponse versionnée réellement reçue pour les champs précis.

## Grandes familles de signaux

Ces catégories sont **descriptives** : elles ne prouvent ni l'activation permanente d'un évaluateur ni une correspondance universelle entre versions d'API.

| Famille | Indication possible | Ce qu'elle ne démontre pas |
|---|---|---|
| Cohérence / pertinence sémantique | Relation entre la question et la réponse selon une méthode déclarée (historiquement, similarité d'embeddings) | Exactitude factuelle, utilité universelle ou accord humain |
| Risque factuel / hallucination | Estimation par un juge des risques liés aux affirmations factuelles d'une réponse ; des versions historiques utilisent un LLM-juge | Vérification indépendante du vrai sans source de référence ni protocole adapté |
| Risque sémantique / modération | Catégorie ou condition de risque signalée par un classifieur sur le contenu effectivement évalué | Garantie globale de sécurité, probabilité calibrée ou couverture du contenu non examiné |
| Ambiguïté / instabilité sémantique | Caractérisation d'une ambiguïté selon le protocole déclaré | Variabilité réellement mesurée entre répétitions si celles-ci n'ont pas eu lieu |
| Stabilité / agrégats | Valeur dérivée de signaux déclarés sous une version et configuration spécifiques | Grandeur autonome, indépendance des composantes ou invariance entre versions |

Ne pas confondre `G`, `g_score`, `g_final` et `stability_score`. Leur relation et leur calcul exigent une référence propre à la version ; des équations publiques plus anciennes concernent des implémentations historiques.

## Informations nécessaires pour lire une mesure

1. **Objet observé :** entrée, réponse et périmètre effectivement examinés.
2. **État de la mesure :** `measured`, `not_measured`, `degraded`, incomplet ou non déclaré, selon le contrat applicable. Une mesure absente n'est ni zéro ni une valeur favorable.
3. **Couverture :** contrôles tentés, réussis et indisponibles. Une décision `ALLOW` ne prouve pas à elle seule qu'un contrôle a eu lieu.
4. **Identité et configuration :** version de mesure **et** évaluateur/modèle réellement servi, avec les empreintes de configuration effectivement disponibles. Un modèle configuré ou demandé n'est pas forcément celui ayant répondu. Ne jamais inventer les empreintes absentes.
5. **Validité :** date et périmètre de calibration, corpus de référence, comparateurs, erreurs, répétabilité et réplication indépendante. À défaut, indiquer **non établi**.
6. **Comparabilité :** vérifier la compatibilité des protocoles, versions, couvertures, évaluateurs et conditions. Les mêmes noms de champs ou versions ne suffisent pas.

## Exemples de lecture prudente

- `coherence_score` présent : *signal lié à la pertinence sous la méthode utilisée* — **pas** « réponse vraie ».
- Faible risque factuel : *peu de problèmes détectés par ce juge dans ces conditions* — **pas** « aucune hallucination ».
- `status: not_measured`, `value: null` : *aucune valeur numérique disponible* — **ni** zéro **ni** succès.
- Test fumée réussi : *chaîne technique fonctionnelle sur les cas testés* — **pas** validation générale.

## Qualification et confidentialité

La calibration et la validation s'apprécient **par signal, version, domaine et expérience**. Un test limité ne qualifie pas l'ensemble des évaluateurs. Consulter le [programme de qualification expérimentale](https://github.com/neomundi-io/neomundi-metrology-validation) et sa [politique de divulgation](https://github.com/neomundi-io/neomundi-metrology-validation/blob/main/PUBLIC_DISCLOSURE_POLICY.fr.md).

Expliquer la sémantique et les limites ne nécessite pas de divulguer les prompts internes, le routage effectif, les clés ou les jeux de données protégés. Inversement, la confidentialité ne doit pas masquer les résultats négatifs ni laisser croire à une reproductibilité non démontrée. Les artefacts figés historiques ne décrivent pas automatiquement la production actuelle.

## Références

- [Comprendre les mesures](READING_MEASUREMENTS.fr.md)
- [Metric Contract v0.0 — Draft](metric-contract-v0.0.fr.md)
- [Tableau d'interprétation API](https://github.com/neomundi-io/neomundi-runtime-measurement/blob/main/docs/MEASUREMENT_INTERPRETATION_TABLE.md)
- [Metrology Validation](https://github.com/neomundi-io/neomundi-metrology-validation)
- [Limites historiques du SDK](https://github.com/neomundi-io/neomundi-python-sdk/blob/main/docs/LIMITES.md)

**Frontière :** les mesures contribuent à l'interprétation technique et à la supervision humaine. Elles ne certifient ni vérité, ni sécurité, ni conformité et ne donnent aucune autorisation d'exécution.
