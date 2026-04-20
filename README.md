# Modèle de Taux Hull-White 1 Facteur - Projet de Stage

Projet Python de finance quantitative réalisé en stage chez ORACT Advisory, consacré à la construction de courbe de taux, la calibration du modèle de Hull-White 1 facteur et la simulation Monte Carlo des taux courts.

L'objectif du dépôt est de proposer un projet propre, reproductible et défendable, adapté à un contexte de stage en modélisation quantitative des taux d'intérêt.

## Sommaire

1. [Vue d'ensemble](#vue-densemble)
2. [Ce que fait le projet](#ce-que-fait-le-projet)
3. [Cadre quantitatif](#cadre-quantitatif)
4. [Méthodologie retenue](#méthodologie-retenue)
5. [Architecture du dépôt](#architecture-du-dépôt)
6. [Installation et exécution](#installation-et-exécution)
7. [Sorties générées](#sorties-générées)
8. [Compétences mises en évidence](#compétences-mises-en-évidence)
9. [Limites du projet](#limites-du-projet)
10. [Extensions naturelles](#extensions-naturelles)

## Vue d'ensemble

Le pipeline implémente une chaîne complète autour du modèle Hull-White 1F:

1. lecture de la courbe zéro-coupon de marché;
2. construction QuantLib de la courbe d'actualisation;
3. calibration des paramètres `(a, sigma)` sur volatilités de swaptions;
4. construction de la fonction de dérive `theta(t)`;
5. simulation Monte Carlo des trajectoires de taux courts;
6. comparaison de la courbe simulée moyenne avec la courbe cible;
7. génération automatique de tableaux, figures et rapport final.

Le dépôt a été pensé comme un livrable de stage: lisible, testable et focalisé sur la logique quantitative.

## Ce que fait le projet

Le projet permet de:

- charger des données de courbe et de volatilités swaptions;
- construire la courbe ZC avec conventions de marché cohérentes;
- diagnostiquer l'alignement de courbe par nœuds et par buckets;
- calibrer Hull-White 1F via minimisation d'erreur de prix;
- calculer `theta(t)` sur grille régulière;
- simuler les taux courts sous dynamique Hull-White;
- reproduire les prix/taux zéro-coupon moyens depuis la simulation;
- mesurer les erreurs absolues et relatives selon la maturité;
- produire automatiquement des sorties `CSV`, `PNG`, `Markdown`;
- valider les briques principales via tests unitaires.

## Cadre quantitatif

Le modèle suit la dynamique short-rate Hull-White 1 facteur:

$$
dr_t = (\theta(t) - a r_t)\,dt + \sigma\,dW_t
$$

avec:

- `a` : vitesse de retour à la moyenne;
- `sigma` : volatilité instantanée;
- `theta(t)` : dérive déterministe assurant la cohérence avec la courbe initiale.

La validation principale consiste à vérifier que la moyenne Monte Carlo reproduit correctement la structure par terme observée en entrée, y compris sur le long terme.

## Méthodologie retenue

### 1. Courbe de taux

La courbe zéro-coupon est construite depuis `data/zc_rates.csv` via QuantLib, avec interpolation cohérente et extrapolation contrôlée.

### 2. Calibration

Les paramètres `(a, sigma)` sont calibrés sur `data/swaption_vols.csv` afin de minimiser l'écart entre prix modèle et prix implicites de marché.

### 3. Theta et simulation

Après calibration, `theta(t)` est évaluée sur une grille annuelle fine puis utilisée pour simuler les trajectoires de taux courts sur horizon long.

### 4. Validation

Les courbes simulées sont comparées à la courbe cible pour produire des métriques globales et par buckets de maturité.

## Architecture du dépôt

```text
project_root/
├── README.md
├── requirements.txt
├── run_all.py
├── config.yaml
├── .gitignore
├── data/
│   ├── zc_rates.csv
│   └── swaption_vols.csv
├── src/
│   ├── __init__.py
│   ├── curve_builder.py
│   ├── curve_diagnostics.py
│   ├── calibration.py
│   ├── hw_model.py
│   ├── hw_theta.py
│   ├── simulation.py
│   ├── validation.py
│   ├── plots.py
│   ├── reporting.py
│   └── utils.py
└── tests/
    ├── test_curve_builder.py
    ├── test_calibration.py
    ├── test_simulation.py
    ├── test_theta.py
    └── test_validation.py
```

Le dépôt public est volontairement épuré: les documents de travail (docs, reports, notebooks) restent en local.

## Installation et exécution

### 1. Environnement

Sous Windows:

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

Sous Linux/macOS:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### 2. Pipeline complète

```bash
python run_all.py
```

### 3. Tests

```bash
pytest -q
```

## Sorties générées

Après exécution, les livrables sont produits dans `outputs/`:

- `outputs/tables/calibration_parameters.csv`
- `outputs/tables/curve_alignment_before_after.csv`
- `outputs/tables/validation_summary.csv`
- `outputs/figures/curve_target_vs_reproduced.png`
- `outputs/figures/curve_zoom_long_end_gt_10y.png`
- `outputs/reports/final_report.md`

## Compétences mises en évidence

Ce projet met en évidence:

- modélisation de taux d'intérêt en temps continu;
- calibration de modèle sur instruments de taux;
- simulation Monte Carlo quantitative;
- utilisation de QuantLib en Python;
- validation d'un modèle par indicateurs d'erreur;
- structuration d'un pipeline de recherche reproductible;
- industrialisation minimale d'un livrable de stage (tests + reporting).

## Limites du projet

Le projet reste volontairement ciblé:

- modèle 1 facteur (pas de multifactoriel);
- calibration sur jeu de données donné;
- pas de calibration dynamique rolling;
- pas de contraintes réglementaires de production;
- pas d'interface applicative.

## Extensions naturelles

Les extensions les plus pertinentes seraient:

- calibration rolling sur différentes dates de marché;
- comparaison Hull-White vs G2++;
- ajout d'instruments de validation (caps/floors, swaptions exotiques);
- étude de sensibilité plus poussée sur `(a, sigma)` et sur la courbe d'entrée;
- packaging applicatif (API ou dashboard).

## En une phrase

Ce dépôt présente une implémentation stage-ready du modèle Hull-White 1 facteur, avec une chaîne complète de courbe, calibration, simulation et validation, pensée pour être lisible, robuste et présentable sur GitHub.
