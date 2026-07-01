# Third-Party Notices and Claim Boundary

This project contains application-layer code for first-person hand
reconstruction workflows. It integrates with external model code, model assets,
datasets, and scientific Python libraries.

## Self-Developed Scope

The software-copyright preparation material counts the following application
and tooling source files as self-developed project code:

- `detect_and_reconstruct.py`
- `export_2d_joints.py`
- `export_3d_joints.py`
- `export_mano_params.py`
- `train.py`
- `egohand3d_cli.py`
- `egohand3d/*.py`
- `tools/*.py`

At the time of registration-material preparation, these Python files contain
`3214` lines.

## Excluded From Original-Source Claim

The following are not claimed as original self-developed source code:

- `wilor/`: WiLoR-based model, dataset, loss, renderer, and utility code.
- `pretrained_models/`: pretrained checkpoint and detector assets.
- `mano_data/`: MANO model data.
- datasets and WebDataset tar files.
- example photos and videos.
- generated outputs, logs, reports, and caches.
- software-copyright registration drafts and official templates.

## Public-Release Caution

Before making this repository public, verify whether the local `wilor/` source
may be redistributed under its upstream license. If redistribution is not
permitted or the license is unclear, do one of the following:

1. remove `wilor/` from the public repository and document how users should
   install it from the upstream project;
2. use a Git submodule that points to the upstream repository;
3. keep this repository private.

Pretrained weights, MANO data, datasets, and example images should not be
uploaded unless their licenses explicitly permit redistribution.

## Dependency Categories

Runtime and development dependencies include, but are not limited to:

- Python
- PyTorch
- PyTorch Lightning
- Hydra
- OpenCV
- NumPy
- Ultralytics
- Trimesh
- Pyrender
- WebDataset

Each dependency remains governed by its own license.

## Suggested Citation and Attribution

If this project is published for research or engineering reuse, include
attribution for the upstream hand reconstruction model and any datasets used for
training or evaluation. Keep upstream citation text in the README or a dedicated
`CITATION.cff` file once the exact upstream references are confirmed.
