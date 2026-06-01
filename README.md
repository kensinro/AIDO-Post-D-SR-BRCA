# AIDO-Post-D-SR-BRCA code

Code package for the TCGA-BRCA Post-D / BP-state reconstruction manuscript:

**Observation-ready and task-discriminative biological-process observables enable non-random patient-level functional-state reconstruction in breast cancer**

This repository organizes the Python scripts used to generate the BRCA patient-level biological-process state reconstruction, random-control validation, bootstrap/PAM50/survival analyses, and manuscript figures/tables.

## Manuscript analysis axis

1. Project GO Biological Process terms onto the BRCA RNA-seq matrix.
2. Apply observation-readiness filtering using a minimum matched-gene cutoff of 10.
3. Screen Stage I/II versus Stage III/IV discriminability using `D = -log10(p)`.
4. Reorganize nominally stage-discriminative BP terms into BP-state modules/components.
5. Project modules onto individual patients and calculate late-centroid similarity.
6. Validate using random BP modules, stage-label shuffling, patient scrambling, bootstrap resampling, PAM50 subtype analysis, and survival endpoints.

## Script order

Run the scripts in this order. The first two scripts are upstream reconstruction steps; scripts 02–04 are the core manuscript pipeline.

| Step | Script | Purpose |
|---|---|---|
| 00 | `scripts/00_brca_stage_bp_screening_pilot.py` | Stage I/II vs III/IV BP screening, readiness filtering, and BP network/pilot outputs. |
| 01 | `scripts/01_bp_module_reconstruction.py` | Reconstruct BP-state modules/components from stage-associated BP outputs. |
| 02 | `scripts/02_patient_bp_state_profile.py` | Build patient-level BP-state profiles, centroids, similarity/distance metrics, PCA/heatmaps. |
| 03 | `scripts/03_random_controls.py` | Random BP-module baseline, stage-label shuffle, and patient-scramble controls. |
| 04 | `scripts/04_downstream_validation.py` | Bootstrap stability, PAM50/subtype tests, survival endpoints, and integrated evidence table. |
| optional | `scripts/optional_tme_conditioned_module_analysis.py` | Exploratory TME-conditioned module analysis; not required for the main BRCA-SR manuscript core. |

The `original_scripts/` folder preserves the original working files exactly as provided.

## Expected local data layout

The scripts currently use local Windows paths. Update the path block near the top of each script before running, or adapt them to a shared `config.json`.

Expected inputs include:

```text
D:/AIDO-Data/UCSC_XENA/Breast Cancer (BRCA)/GE.tsv
D:/AIDO-Data/UCSC_XENA/Breast Cancer (BRCA)/Phenotype.tsv
D:/AIDO-Data/UCSC_XENA/Breast Cancer (BRCA)/TCGA.BRCA.sampleMap_BRCA_clinicalMatrix
D:/AIDO-Data/GSEA/c5.go.bp.v2026.1.Hs.symbols.gmt
D:/AIDO-Data/GSEA/h.all.v2026.1.Hs.symbols.gmt
D:/AIDO-Temp/
```

A template is provided in `config_paths_template.json`.

## Installation

```bash
python -m venv .venv
.venv/Scripts/activate  # Windows PowerShell/CMD users may need the matching activation command
pip install -r requirements.txt
```

## Reproducibility notes

The random-control script fixes the random seed as `20260531`. The BRCA random BP-module baseline uses 500 random module iterations, the stage-label shuffle uses 1000 iterations, and patient scrambling uses 500 iterations in the current working version.

Main manuscript-linked outputs include:

- Figure 1: workflow figure prepared outside this code package.
- Figure 2: random-control validation outputs from `03_random_controls.py`.
- Figure 3: late-centroid similarity and bootstrap stability outputs from `04_downstream_validation.py`.
- Figure 4: PAM50/subtype outputs from `04_downstream_validation.py`.
- Figure 5: OS/DSS survival outputs from `04_downstream_validation.py`.

## Recommended GitHub upload

```bash
git init
git add README.md requirements.txt .gitignore config_paths_template.json scripts docs original_scripts
git commit -m "Add BRCA Post-D BP-state reconstruction code"
git branch -M main
git remote add origin https://github.com/<USER>/<REPO>.git
git push -u origin main
```

Do not upload TCGA raw data, UCSC Xena downloaded files, or large generated output folders unless the target repository is designed for data deposition. Keep raw data paths local and provide data-access instructions in the manuscript/code availability statement.
