# Data

This folder is where the pipeline expects the ADNI data. **It is intentionally empty in the repository.** ADNI data cannot be redistributed under the ADNI Data Use Agreement, and `.gitignore` excludes everything here except this file.

## What you need

| File | Source |
|---|---|
| `ADNIMERGE.csv` | ADNI study data, LONI Image and Data Archive (IDA) |

`ADNIMERGE.csv` has one row per subject visit, with demographics, diagnosis (`DX`), cognitive assessments, FreeSurfer MRI volumes, CSF biomarkers, and PET measures. The pipeline uses only columns from this table.

## How to get it

1. Apply for access at <https://adni.loni.usc.edu/data-samples/access-data/>. You will agree to the ADNI Data Use Agreement as part of the application.
2. After approval, log in to <https://ida.loni.usc.edu>, open ADNI, and go to the study data downloads.
3. Download the ADNIMERGE table as a CSV.
4. Save it here as `data/ADNIMERGE.csv`.

The paper used the legacy ADNIMERGE release (ADNI1 through ADNI3, 16,420 rows). Newer ADNI4 data is distributed through the restructured ADNIMERGE2 package, which does not include the FreeSurfer volumes this pipeline needs.

## Rules

- Do not commit any file from this folder.
- Do not upload ADNI data to a public location, including public Kaggle datasets.
- Outputs the pipeline writes (`pipeline/outputs`, `pipeline/checkpoints`) can contain subject-level information and are excluded from git for the same reason.
