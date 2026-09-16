# MLR — Machine Learning Models for Ligand-Controlled Selectivity in NHC–Pd Catalysis

This repository contains the data and Jupyter notebooks behind the multivariate linear
regression (MLR) and random forest (RF) models that predict the experimental
selectivity (ΔΔG‡) of NHC-ligated Pd catalysts from DFT-derived descriptors.

The models are trained on experimental ΔΔG‡ values for a growing set of NHC-type
ligands (5-IMes, 5-IPr, 5-IPr\*, 5-IPent, 5-Np#, and related) combined with ~44–61
computed descriptors per ligand: interaction free energies (ωB97XD-D), Pd–C bond
lengths and angles, buried volumes (%Vbur at 3.0/3.5/4.0 Å), NPA and Mulliken charges,
Pd d-orbital occupations and energies, NBO donor–acceptor interaction energies, and
ligand-level descriptors (B1_Sp2, B5_Sp2, E_Pd(LP), frontier orbital energies).

## Repository structure — one notebook per manuscript figure

| Notebook | Figure | Content |
|---|---|---|
| `Figure 3A.ipynb` | 3A | Random Forest feature-importance ranking over all 44 descriptors (16-ligand training set) |
| `Figure 3B.ipynb` | 3B | Two-parameter MLR parity plot and MLR vs RF comparison (16-ligand training set) |
| `Figure 4A&B.ipynb` | 4A, 4B | Random Forest feature-importance ranking over all 61 descriptors (4A) and correlation heatmap of the top six descriptors (4B), 21-ligand training set |
| `Figure 4C&D.ipynb` | 4C, 4D | Four-parameter MLR parity plot (4C) and MLR vs RFR performance comparison (4D), 21-ligand training set |
| `Figure 5B.ipynb` | 5B | Predicted ΔΔG‡ for 20 designed ligands (L22–L41) with the four-parameter model, benchmarked against two experimentally validated ligands |
| `Figure S5.ipynb` | S5 | Exhaustive one- and two-parameter descriptor screening (11-ligand training set) |
| `Figure S6.ipynb` | S6 | Original two-parameter MLR model on the 11-ligand training set |
| `Figure 5B.xlsx` | 5B (data) | Raw and scaled descriptors and predicted ΔΔG‡ for the 20 designed ligands |

## Training data

| File | Ligands | Descriptors |
|---|---|---|
| `Training Set_11.csv` | 11 | 47 |
| `Training Set_16.csv` | 16 | 47 |
| `Training Set_21.csv` | 21 | 64 |

Each CSV is indexed by ligand name and contains the experimental target (`ΔΔG_Exp`),
experimental syn/anti selectivity (`Exp_syn`, `Exp_anti`), and the computed descriptors.
Absolute ωB97XD-D energies are included alongside the per-ligand relative values used
for modelling.

## Key results

- **Two-parameter model (11/16-ligand sets):** ΔΔG‡ from BL(C28–Pd) and %Vbur(3.0 Å);
  R² = 0.99 / RMSE = 0.004 kcal/mol (11 ligands), R² = 0.95 / RMSE = 0.024 kcal/mol
  (16 ligands).
- **Four-parameter model (21-ligand set):**

  ΔΔG‡ = 0.38·[B1_Sp2] + 0.32·[%Vbur(3.0 Å)] − 0.37·[B5_Sp2] − 0.31·[E_Pd(LP)] + 0.19

  R² = 0.90, RMSE = 0.041 kcal/mol, leave-one-out Q² = 0.81 (n = 21).
- **Descriptor ranking:** Random Forest analysis consistently identifies steric
  descriptors (%Vbur, B5_Sp2) and the Pd lone-pair energy as the most important
  features for selectivity.
- **Model validation:** of the 20 designed ligands predicted with the four-parameter
  model, two were tested experimentally — L24 (predicted +1.58, found +1.31 kcal/mol)
  and L30 (predicted −0.55, found −0.49 kcal/mol).

## Requirements

- Python 3.11+
- pandas, numpy, scikit-learn, matplotlib, seaborn
- Jupyter / JupyterLab

## Usage

Run each notebook from the repository root — all notebooks read the
`Training Set_*.csv` files by relative path. Each notebook reproduces one (or two)
manuscript figures exactly as submitted, including all embedded outputs.

## Notes

- Random Forest metrics (R² ≈ 0.97–0.98) are in-sample values computed on the full
  training set; the leave-one-out Q² values from the linear models are the
  cross-validated performance figures.
- `Figure 5B.xlsx` contains both raw descriptor values and values scaled against the
  21-ligand training set (columns with the `_S` suffix), as used by the model.
