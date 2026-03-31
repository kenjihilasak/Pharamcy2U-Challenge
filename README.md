# Hackathon Track 1 Repository

This repository contains the work for the Pharmacy2U refill-risk challenge.

## Project Layout

```text
.
|-- data/
|   |-- raw/
|   |-- processed/
|   `-- outputs/
|-- notebooks/
|   |-- preprocesing.ipynb
|   `-- model_classification.ipynb
|-- docs/
|   `-- preprocesing_model_classification.md
|-- references/
|   |-- Pharmacy2U_Leeds_2Day_Hackathon_Prescriptions_Brief_to_send.pdf
|   `-- hackathon_track_brief.md
|-- .gitignore
`-- README.md
```

## Data Folders

- `data/raw/`: original source files downloaded from CMS or other external sources.
- `data/processed/`: cleaned feature tables created by `preprocesing.ipynb`.
- `data/outputs/`: model metrics, predictions, calibration tables, and other generated results.

## Notebooks

- `notebooks/preprocesing.ipynb`: builds leakage-safe regression and classification datasets from the raw PDE file.
- `notebooks/model_classification.ipynb`: trains and evaluates the late-refill classification models.

Both notebooks resolve the project root automatically, so they work whether you run them from the repository root or from inside `notebooks/`.

## References

- `references/Pharmacy2U_Leeds_2Day_Hackathon_Prescriptions_Brief_to_send.pdf`
- `references/hackathon_track_brief.md`

## Notes

- Raw source data under `data/raw/` is ignored by git.
- The old `DATA-AI-Hackathon-Track-1/` folder is no longer part of the active project layout.
