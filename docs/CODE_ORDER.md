# Code order and dependency chain

## Minimal manuscript reproduction chain

1. `00_brca_stage_bp_screening_pilot.py`
   - Input: BRCA GE matrix, GO BP GMT, stage/clinical files.
   - Output: observation-readiness table, stage discriminability table, selected BP terms, BP network/pilot outputs.

2. `01_bp_module_reconstruction.py`
   - Input: outputs from step 00.
   - Output: module assignment, module summary, early-vs-late module overlap/rewiring summaries.

3. `02_patient_bp_state_profile.py`
   - Input: BRCA GE matrix, GO BP/Hallmark GMT, module outputs.
   - Output: patient module scores, patient centroid similarity/distance metrics, module centroids, master table, profile figures.

4. `03_random_controls.py`
   - Input: outputs from steps 00–02.
   - Output: random BP-module nulls, stage-label shuffle, patient scramble, empirical p values, random-control figures.

5. `04_downstream_validation.py`
   - Input: outputs from step 03 plus phenotype/clinical matrix.
   - Output: bootstrap, PAM50, survival, supplementary clinical endpoint scans, integrated evidence table.

## Optional analysis

`optional_tme_conditioned_module_analysis.py` is exploratory and can stay outside the main manuscript reproduction chain unless used in a supplement or future paper.
