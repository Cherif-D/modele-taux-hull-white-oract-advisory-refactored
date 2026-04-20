# Hull-White 1F Interest Rate Model - Internship Project

Projet developpe dans le cadre d'un stage en modelisation des taux chez ORACT Advisory.
L'objectif est de produire une chaine complete, claire et reproductible autour du modele de Hull-White 1 facteur: de la courbe de taux a la simulation Monte Carlo, puis a la validation des resultats.

## Project Highlights

- Construction d'une courbe zero-coupon a partir de donnees de marche.
- Calibration du modele Hull-White 1F sur volatilites de swaptions.
- Calcul de la derive temporelle `theta(t)`.
- Simulation Monte Carlo des trajectoires de taux courts.
- Validation de la courbe reproduite avec suivi des erreurs par maturite.
- Generation automatique de tableaux et figures pour analyse quantitative.

## Repository Structure

```text
.
├── data/                      # Donnees d'entree (zc rates + swaptions)
├── notebooks/                 # Notebook d'analyse calibration
├── src/                       # Modules metier (courbe, calibration, simulation, validation)
├── tests/                     # Tests unitaires
├── run_all.py                 # Pipeline principale
├── config.yaml                # Configuration unique
└── requirements.txt           # Dependances Python
```

## Methodology

1. Chargement de la courbe zero-coupon de marche.
2. Construction QuantLib de la courbe et diagnostics de coherence.
3. Calibration des parametres `(a, sigma)` du modele Hull-White.
4. Generation de `theta(t)` sur grille temporelle.
5. Simulation de trajectoires de taux courts.
6. Repricing moyen des zero-coupons et comparaison au marche.

## Quick Start

```bash
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
# .venv\Scripts\Activate.ps1  # Windows PowerShell

python -m pip install --upgrade pip
pip install -r requirements.txt

python run_all.py
pytest -q
```

## Main Outputs

Apres execution, les livrables sont disponibles dans `outputs/`:

- `outputs/tables/calibration_parameters.csv`
- `outputs/tables/validation_summary.csv`
- `outputs/figures/curve_target_vs_reproduced.png`
- `outputs/figures/curve_zoom_long_end_gt_10y.png`
- `outputs/reports/final_report.md`

## Core Stack

- Python 3.10+
- QuantLib
- NumPy / Pandas / SciPy
- Matplotlib
- PyTest

## Context

Stage M1 Finance - modelisation quantitative des taux d'interet.
