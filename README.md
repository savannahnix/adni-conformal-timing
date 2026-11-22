# Conformal Calibration of Predicted MCI-to-AD Conversion Timing

Code for the paper:

> Savannah Nix, Xinyi Zhang, Xinjie Lan, for the Alzheimer's Disease Neuroimaging Initiative. *Beyond Concordance: A Conformal Calibration Study of Predicted Alzheimer's Disease Conversion Timing.* Submitted to *Computer Methods and Programs in Biomedicine*.

The pipeline trains four survival models (Cox proportional hazards, Weibull accelerated failure time, gradient-boosted survival analysis, and DeepSurv) to predict time from mild cognitive impairment (MCI) to Alzheimer's disease (AD) in ADNI. It wraps each model in conformal prediction intervals and measures where each observed conversion falls inside its interval (the within-interval percentile, ideal 0.5). It compares fixed-width split conformal intervals with two adaptive constructions, Conformalized Survival Distributions (CSD) and CiPOT, and includes the paper's sensitivity analyses.

**No ADNI data is included in this repository.** You need your own ADNI access to run it (see [Data](#data)).

## Repository structure

```
adni-conformal-timing/
├── pipeline/
│   ├── conformal_timing_pipeline.ipynb   # Full pipeline; produces every number, table, and figure in the paper
│   ├── preprocessing.py                  # Survival labels, ComBat harmonization, imputation, cohort assembly
│   ├── modeling.py                       # Cox PH, Weibull AFT, GBSA, DeepSurv training and tuning
│   ├── postprocessing.py                 # Kaplan-Meier curves and survival-curve plots
│   ├── concordance.py                    # IPCW time-dependent concordance (Antolini C-td)
│   ├── config.py                         # Shared constants
│   ├── checkpoints/                      # Saved models (created at run time, not committed)
│   ├── figures/                          # Generated figures (not committed)
│   └── outputs/                          # Generated tables and paper_numbers.json (not committed)
├── data/
│   └── README.md                         # How to obtain ADNIMERGE.csv and where to put it
├── requirements.txt
├── LICENSE
└── README.md
```

## Data

This study uses data from the [Alzheimer's Disease Neuroimaging Initiative (ADNI)](https://adni.loni.usc.edu). ADNI data are available to qualified researchers who apply for access and agree to the ADNI Data Use Agreement, which does not allow redistribution. For that reason this repository contains code only.

To run the pipeline:

1. Apply for ADNI data access at <https://adni.loni.usc.edu/data-samples/access-data/>.
2. Once approved, log in to the LONI Image and Data Archive (IDA) and download the **ADNIMERGE** table (`ADNIMERGE.csv`) from the ADNI study data.
3. Place the file at `data/ADNIMERGE.csv`, or set `DATA_PATH` in Section 1.2 of the notebook to its location.

`data/README.md` has more detail. The paper used the legacy ADNIMERGE release covering ADNI1, ADNI-GO, ADNI2, and ADNI3 (16,420 subject-visit rows). Results from a different release may differ slightly.

## Setup

Python 3.10 or later is recommended.

```bash
git clone https://github.com/savannahnix/adni-conformal-timing.git
cd adni-conformal-timing
pip install -r requirements.txt
```

## Running the pipeline

Open `pipeline/conformal_timing_pipeline.ipynb` and run all cells. Never make a dataset containing ADNI data public.

Flags at the top of the notebook control run time:

| Flag | Default | Effect |
|---|---|---|
| `RETRAIN` | `True` | Retrains all four models. Set `False` to reload saved checkpoints. |
| `FIT_SET` | `'mci_train'` | Subjects used to fit ComBat, z-score constants, and MICE (training data only). |
| `RUN_MODEL_COMPARISON_BOOTSTRAP` | `False` | 100-resample pairwise model comparison. Adds about 7 hours. |
| `RUN_KFOLD_BOOTSTRAP` | `True` | Five-fold landmark analysis with bootstrap CIs. |
| `RUN_INTERVAL_SHAPE_KFOLD` | `True` | Five-fold comparison of fixed width, CSD, and CiPOT (Table 6). |

A full run takes about 2 to 3 hours on CPU without the model-comparison bootstrap. A random seed of 42 is used throughout, and two full reruns reproduced every reported value.

### Where each result comes from

| Paper item | Notebook section |
|---|---|
| Table 1 (baseline characteristics) | 4.4 |
| Table 2 (C-td) and pairwise bootstrap | 7, 14.1 |
| Table 3 (landmark analysis) | 12.5, 12.6 |
| Table 4 (sensitivity analyses) | 10.10, 13.1, 13.2 |
| Table 5 (fixed width, re-centered, CSD, CiPOT) | 10.8, 10.10 |
| Table 6 (five-fold validation) | 13.6 |
| Figure 4 and Supplementary Fig. S1 | 10.8 |
| Figure 5 and Supplementary Fig. S2 | 10.9 |
| Figures 2, 3, and 6 | 14.2 |
| All reported numbers | 14.1 and 14.1b (`outputs/paper_numbers.json`) |

## Data protection

`.gitignore` excludes the `data/`, `checkpoints/`, `outputs/`, and `figures/` folders and all `.csv`, `.npz`, `.pkl`, and `.zip` files. Model checkpoints and cached per-subject arrays are derived from ADNI data and must not be committed or shared. Before pushing, run `git status` and confirm that no data files are staged.

## Citation

If you use this code, please cite the paper (full reference to be added on publication).

## Acknowledgements

Data collection and sharing for this project was funded by the Alzheimer's Disease Neuroimaging Initiative (ADNI). See the paper for the full ADNI acknowledgement. A complete listing of ADNI investigators can be found at <http://adni.loni.usc.edu/wp-content/uploads/how_to_apply/ADNI_Acknowledgement_List.pdf>.

## License

MIT. See [LICENSE](LICENSE).
