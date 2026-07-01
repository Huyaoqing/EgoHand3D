# EgoHand3D

EgoHand3D is an application-layer toolkit for egocentric hand reconstruction,
result export, visualization, evaluation, and training workflow management. It
wraps a WiLoR-based hand reconstruction runtime with practical command-line
tools for first-person images and videos.

![EgoHand3D overview](docs/assets/egohand3d-overview.png)

## What It Does

- Detect hands in image folders or videos.
- Reconstruct hand meshes and export OBJ files.
- Export 21 projected 2D hand keypoints.
- Export 21 3D hand joints with camera metadata.
- Export MANO pose, shape, and camera parameters.
- Visualize predicted joints and video overlays.
- Evaluate HOI4D-style 2D keypoint predictions.
- Build manifests, result reports, and merged export indexes.
- Validate runtime dependencies and model asset paths.

## Repository Layout

```text
egohand3d/                 application-layer inference, IO, and visualization helpers
tools/                     reporting, video, evaluation, validation, and merge tools
scripts/                   common inference and training shell commands
docs/                      GitHub Pages project website
detect_and_reconstruct.py  image reconstruction and mesh export entry point
export_2d_joints.py        2D joint export entry point
export_3d_joints.py        3D joint export entry point
export_mano_params.py      MANO parameter export entry point
egohand3d_cli.py           unified CLI wrapper
train.py                   training workflow entry point
```

## External Assets

This repository does not publish model weights, MANO data, datasets, generated
outputs, or software-copyright registration documents by default.

Expected runtime assets:

```text
wilor/ or an installed WiLoR-compatible Python package
pretrained_models/wilor_final.ckpt
pretrained_models/model_config.yaml
pretrained_models/detector.pt
mano_data/MANO_RIGHT.pkl
mano_data/MANO_LEFT.pkl
```

You can provide them as directories, symlinks, or command-line paths:

```bash
ln -s /path/to/WiLoR/wilor wilor
ln -s /path/to/pretrained_models pretrained_models
ln -s /path/to/mano_data mano_data
```

Before publishing this repository publicly, verify whether the vendored
`wilor/` code may be redistributed under its upstream license. If not, replace
it with installation instructions or a Git submodule that points to the original
project.

## Environment

Use the project environment if it already exists:

```bash
source scripts/activate.sh
```

Or create an environment from the portable dependency files:

```bash
conda env create -f environment.yml
conda activate egohand3d
pip install -r requirements.txt
```

Run a lightweight setup check:

```bash
python egohand3d_cli.py validate --image_dir /path/to/images
```

## Quick Start

Mesh reconstruction and overlay:

```bash
python egohand3d_cli.py reconstruct \
  --img_folder /path/to/images \
  --out_folder outputs/demo_mesh
```

Export 2D joints:

```bash
python egohand3d_cli.py export-2d \
  --img_folder /path/to/images \
  --out_folder outputs/2d_joints
```

Export 3D joints:

```bash
python egohand3d_cli.py export-3d \
  --img_folder /path/to/images \
  --out_folder outputs/3d_joints
```

Export MANO parameters:

```bash
python egohand3d_cli.py export-mano \
  --img_folder /path/to/images \
  --out_folder outputs/mano_params
```

Build a processing manifest:

```bash
python egohand3d_cli.py manifest \
  --image_dir /path/to/images \
  --pred_dir outputs/2d_joints \
  --out outputs/manifest.json
```

Merge exported results:

```bash
python egohand3d_cli.py merge \
  --joints2d_dir outputs/2d_joints \
  --joints3d_dir outputs/3d_joints \
  --mano_dir outputs/mano_params \
  --mesh_dir outputs/demo_mesh \
  --out outputs/merged_exports.json
```

## Unified CLI

```text
reconstruct   export OBJ meshes and overlay renderings
export-2d     export projected 2D hand joints
export-3d     export 3D hand joints and camera metadata
export-mano   export MANO pose, shape, and camera parameters
video         detect and visualize hands in a video
evaluate      evaluate 2D joints against HOI4D-style labels
report        build a report for an output directory
manifest      build an image/video processing manifest
merge         merge exported 2D/3D/MANO/mesh files into one index
visualize     draw JSON joints on source images
validate      validate dependencies and required runtime files
```

## Training

Single-node 4 GPU HOI4D clean:

```bash
bash scripts/train_hoi4d_clean_4gpu.sh
```

Multi-node HOI4D clean + EgoDex:

```bash
bash scripts/train_hoi4d_clean_egodex_2x4gpu_volc.sh
```

Manual smoke run:

```bash
python train.py \
  exp_name=smoke_hoi4d_clean_1gpu \
  data=hoi4d_clean \
  experiment=wilor_vit_refinenet \
  trainer=gpu \
  trainer.devices=1 \
  TRAIN.BATCH_SIZE=2 \
  GENERAL.NUM_WORKERS=2 \
  GENERAL.TOTAL_STEPS=20
```

## GitHub Pages

The project website is in `docs/`. After pushing to GitHub, enable Pages from
`Settings -> Pages -> Deploy from a branch -> main /docs`.

You can also open it locally:

```bash
xdg-open docs/index.html
```

## Software Copyright Materials

The local software-copyright registration materials are intentionally ignored by
Git in `.gitignore`. They include application drafts, Word files, official
templates, and private registration notes. Keep them local or upload them only
to a private repository if you explicitly intend to share them.

The public repository can still describe the claim boundary:

- self-developed application-layer Python source: `3214` lines at the time of
  registration preparation;
- third-party model code, pretrained weights, MANO data, datasets, and example
  photos are not claimed as original source code.

See [GITHUB_PUBLISHING.md](GITHUB_PUBLISHING.md) and
[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) before making the repository
public.
