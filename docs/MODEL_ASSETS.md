# Model Assets Strategy

This repository is the GitHub-safe code version of the demo.

## Why Large Assets Are Excluded

The local nutrition runtime includes files that are too large for a normal GitHub repository workflow.

Examples from the current local runtime:

- `phase1_encoder_nb_e3.pt`: about 1.4 GB
- `phase2_retrieval_e3/faiss_new.index`: about 233 MB
- large `.npy` feature arrays above 100 MB

Because of that, this repository should only store:

- application code
- integration code
- setup documentation
- small config files

## Recommended Split

Use two deliverables:

1. GitHub code repository
2. Separate runtime asset package for internal sharing

## What Goes In GitHub

- React frontend
- Express adapter
- Python server source code if lightweight
- setup docs
- environment variable examples

## What Stays Out Of GitHub

- model checkpoints
- local Hugging Face vendor snapshots
- FAISS indexes
- frozen archives
- generated training or retrieval arrays

## Team Handoff Recommendation

For teammates, publish a second package that includes:

- the runtime folder
- a requirements file
- startup commands
- folder placement instructions

The app repo should reference that package in documentation instead of storing the binary assets directly.
