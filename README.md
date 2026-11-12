<div align="center">

# MarineLite: Marine Organism Detection

<p>GhostConv and SimSPPF integrated into DU-MobileYOLO · URPC2020 · five ablation variants</p>

<a href="https://marine-object-detection.onrender.com/"><img src="https://img.shields.io/badge/Live_demo-Render-17634B?style=for-the-badge&amp;logo=render&amp;logoColor=white" alt="Live demo on Render" /></a>
<a href="https://huggingface.co/spaces/Wizzas/Marine-Object-Detection"><img src="https://img.shields.io/badge/Live_demo-Hugging_Face-FFD21E?style=for-the-badge&amp;logo=huggingface&amp;logoColor=black" alt="Live demo on Hugging Face Spaces" /></a>

<br />

<img src="https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&amp;logo=pytorch&amp;logoColor=white" alt="PyTorch" />
<img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=flat-square&amp;logo=opencv&amp;logoColor=white" alt="OpenCV" />
<img src="https://img.shields.io/badge/mAP%400.5-75.36%25-171717?style=flat-square" alt="mAP@0.5 75.36%" />
<img src="https://img.shields.io/badge/Parameters-4.37M-171717?style=flat-square" alt="4.37M parameters" />
<img src="https://img.shields.io/badge/Latency-19.66_ms-171717?style=flat-square" alt="19.66 ms latency" />

</div>

## Overview

**Integration of GhostConv and SimSPPF into a Deformable-Upsampling Lightweight Detector for Marine Organism Detection**

This project adapts DU-MobileYOLO, a lightweight detector for marine organisms, and evaluates the result on the URPC2020 dataset. It reduces computational redundancy in the detector while keeping its Deformable Upsampling mechanism unchanged.

| Final model | Dataset | Result against the retrained baseline |
| --- | --- | --- |
| DU-MobileYOLO + GhostConv + SimSPPF | URPC2020, 800 test images, 4 classes | mAP@0.5 74.23% → 75.36%, parameters 4.70M → 4.37M, latency 21.06 ms → 19.66 ms |

| | |
| --- | --- |
| **Author and maintainer** | [Wilbert Andrew Yonathan (WAYTECHG)](https://github.com/WAYTECHG) |
| **Institution** | Xiamen University Malaysia |
| **Academic context** | AIT304, Advanced Issues of Artificial Intelligence (Computer Vision) |

## Live demo

| Host | Link |
| --- | --- |
| Render | **[marine-object-detection.onrender.com](https://marine-object-detection.onrender.com/)** |
| Hugging Face Spaces | **[Wizzas/Marine-Object-Detection](https://huggingface.co/spaces/Wizzas/Marine-Object-Detection)** |

## Results on the URPC2020 test set

### Final model against the baseline

| Model | Params (M) | GFLOPs | Latency (ms) | mAP@0.5 (%) | mAP@0.5:0.95 (%) | Precision (%) | Recall (%) |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| DU-MobileYOLO baseline (retrained) | 4.70 | 12.1 | 21.06 | 74.23 | 41.01 | 80.95 | 66.34 |
| **GhostConv + SimSPPF (final)** | **4.37** | **11.5** | **19.66** | **75.36** | 40.71 | 79.16 | **67.18** |
| Difference | -0.33 | -0.6 | -1.40 | +1.13 | -0.30 | -1.79 | +0.84 |

The final model improves mAP@0.5, recall, parameter count, GFLOPs and latency against the retrained baseline. Precision and mAP@0.5:0.95 decrease slightly, which is a small trade-off under stricter localization and precision-based evaluation.

### Ablation study

| Model | Params (M) | GFLOPs | Latency (ms) | mAP@0.5 (%) | mAP@0.5:0.95 (%) | Precision (%) | Recall (%) |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| DU-MobileYOLO + SPPF | 4.60 | 12.0 | 20.04 | 74.04 | 40.86 | 75.78 | 68.70 |
| DU-MobileYOLO + SimSPPF | 4.60 | 12.0 | 19.96 | 75.23 | 40.77 | 78.13 | 68.89 |
| DU-MobileYOLO + GhostConv | 4.47 | 11.6 | 20.20 | 74.62 | 40.69 | 79.29 | 67.90 |
| DU-MobileYOLO + GhostConv + SPPF | 4.37 | 11.5 | 20.17 | 75.32 | 40.67 | 78.85 | 68.36 |
| **DU-MobileYOLO + GhostConv + SimSPPF** | **4.37** | **11.5** | **19.66** | **75.36** | 40.71 | 79.16 | 67.18 |

The final GhostConv + SimSPPF model has the highest mAP@0.5 of the tested variants, with the lowest parameter count and GFLOPs. The GhostConv + SPPF configuration is an ablation variant only, not the final method.

## Method

The model changes selected computationally redundant parts of DU-MobileYOLO:

1. **SimSPPF** replaces the original parallel spatial pyramid pooling structure with a simplified, sequential SPPF-style module using ReLU activation.
2. **GhostConv** replaces selected standard 3×3 convolution layers inside the Multi-Concat blocks to reduce parameter cost.
3. **Deformable Upsampling (DU)** is kept unchanged to preserve spatial alignment during feature fusion.

[![Proposed DU-MobileYOLO architecture with GhostConv Multi-Concat blocks, SimSPPF and Deformable Upsampling](assets/proposed.png)](assets/proposed.png)

*Figure: the DU-MobileYOLO + GhostConv + SimSPPF architecture, showing the backbone, feature fusion network and detection heads for small, medium and large objects. Click the image for full resolution.*

## My contributions

| Area | Contribution |
| --- | --- |
| Model adaptation | Integrated GhostConv into selected 3×3 convolution layers in the Multi-Concat blocks and replaced the original pooling structure with SimSPPF. |
| Preserved architecture | Kept the existing Deformable Upsampling mechanism; it is not claimed as a new component of this project. |
| Baseline comparisons | Retrained and evaluated DU-MobileYOLO and YOLOv7-tiny on the project's URPC2020 split. |
| Ablation study | Evaluated GhostConv, SPPF, SimSPPF and their combinations to measure the effect of each change. |
| Evaluation | Compared accuracy, precision, recall, parameter count, GFLOPs and measured latency, and documented both improvements and trade-offs. |
| Experiment workflow | Organized variant-specific configurations, an experiment runner and supporting scripts, with documented training, testing and detection commands. |
| Interactive demo | Deployed a marine detection demo on Render and Hugging Face Spaces. |

The contribution is the adaptation, integration and experimental evaluation of established components. This project does not claim to have invented DU-MobileYOLO, Ghost modules or SimSPPF.

## Dataset

The project uses the [URPC2020 underwater object detection dataset](https://www.kaggle.com/datasets/lywang777/urpc2020). The link is also recorded in `DatasetLink.txt`.

**Classes:** `0` holothurian, `1` echinus, `2` scallop, `3` starfish.

| Split | Images | Labeled images | Background images |
| --- | ---: | ---: | ---: |
| Train | 5,543 | 5,455 | 88 |
| Validation | 1,200 | 1,153 | 47 |
| Test | 800 | 775 | 25 |

Training and evaluation use an input size of **640 × 640**.

## Getting started

### Environment

```powershell
python -m venv marine_env
marine_env\Scripts\activate
pip install -r requirements.txt
```

Run all commands from the project folder.

### Model variants

| Variant | Description | YAML file |
| --- | --- | --- |
| `baseline` | Retrained DU-MobileYOLO baseline | `DU_MobileYOLO.yaml` |
| `proposed_ghost` | GhostConv ablation | `proposed_ghost.yaml` |
| `proposed_sppf` | SPPF ablation | `proposed_sppf.yaml` |
| `proposed_simsppf` | SimSPPF ablation | `proposed_simsppf.yaml` |
| `proposed_ghost_sppf` | GhostConv + SPPF ablation | `proposed_ghost_sppf.yaml` |
| `proposed_ghost_simsppf` | Final GhostConv + SimSPPF model | `proposed_ghost_simsppf.yaml` |
| `yolov7_tiny` | Retrained YOLOv7-tiny comparison model | `yolov7-tiny.yaml` |

List the supported variants with:

```powershell
python run_experiment.py --list
```

**Implementation note.** The default import in `models/yolo.py` should stay as `from models.common import *`, which is correct for the baseline and YOLOv7-tiny. For the proposed and ablation variants, use `run_experiment.py`: it switches to the right model implementation for each variant and restores the import afterwards.

### Training

All models are trained from scratch. In PowerShell, write `--weights=` and not `--weights ""`. Replace `<variant>` with a name from the table above:

```powershell
python run_experiment.py train --variant <variant> --data data/urpc2020.yaml --epochs 300 --batch-size 8 --img-size 640 --device 0 --workers 2 --weights=
```

For example, the final model:

```powershell
python run_experiment.py train --variant proposed_ghost_simsppf --data data/urpc2020.yaml --epochs 300 --batch-size 8 --img-size 640 --device 0 --workers 2 --weights=
```

YOLOv7-tiny is trained through `train.py` directly:

```powershell
python train.py --weights= --cfg cfg/training/yolov7-tiny.yaml --data data/urpc2020.yaml --epochs 300 --batch-size 8 --img-size 640 640 --device 0 --workers 2 --project runs/train --name yolov7_tiny
```

### Testing

Testing runs on the test split by setting `--task test`:

```powershell
python run_experiment.py test --variant <variant> --weights runs/train/<variant>/weights/best.pt --data data/urpc2020.yaml --batch-size 1 --img-size 640 --device 0 --task test --name <variant>_test
```

YOLOv7-tiny:

```powershell
python test.py --weights runs/train/yolov7_tiny/weights/best.pt --data data/urpc2020.yaml --img-size 640 --batch-size 1 --task test --device 0 --project runs/test --name yolov7_tiny_test
```

### Detection

One image with the final model:

```powershell
python run_experiment.py detect --variant proposed_ghost_simsppf --weights runs/train/proposed_ghost_simsppf/weights/best.pt --source URPC2020/URPC2020/test/images/000008.jpg --img-size 640 --device 0 --conf-thres 0.25 --iou-thres 0.45
```

To run on every test image, set `--source URPC2020/URPC2020/test/images`.

### Reproducing the results

1. Download URPC2020 using the link in `DatasetLink.txt`.
2. Place the dataset according to the paths in `data/urpc2020.yaml`, or edit the YAML to match your machine.
3. Install the dependencies from `requirements.txt`.
4. Train the model variant you want.
5. Evaluate its `best.pt` checkpoint with `--task test`.

## Repository structure

Dataset images, training outputs and model weights are not included.

```text
├── cfg/training/            # Model YAML files for the baseline, YOLOv7-tiny and each variant
├── data/                    # urpc2020.yaml and hyperparameters
├── models/                  # common.py and one common_*.py per variant, plus MobileViT blocks
├── utils/
├── scripts/
├── train.py
├── test.py
├── detect.py
├── export.py
├── run_experiment.py        # Runs train, test and detect for a named variant
├── experiment_workflow.ipynb
├── requirements.txt
├── DatasetLink.txt
└── README.md
```

Excluded from the repository: `runs/`, `URPC2020/`, `datasets/`, `*.pt`, `*.pth`, `__pycache__/`, `*.pyc`, `marine_env/` and `.venv/`.

## Upstream work and acknowledgements

This project builds on:

- **[DU-MobileYOLO](https://github.com/ZERO-SPACE-X/DU-MobileYOLO):** the base marine organism detector and its Deformable Upsampling mechanism, distributed under GPL-3.0.
- **[YOLOv7](https://github.com/WongKinYiu/yolov7):** the detector implementation and the YOLOv7-tiny comparison model.
- **[GhostNet: More Features From Cheap Operations](https://openaccess.thecvf.com/content_CVPR_2020/html/Han_GhostNet_More_Features_From_Cheap_Operations_CVPR_2020_paper.html)** (Han et al., CVPR 2020): the Ghost module concept behind GhostConv.
- **[YOLOv6 SimSPPF implementation](https://github.com/meituan/YOLOv6/blob/main/yolov6/layers/common.py):** the reference for simplified spatial pyramid pooling with ReLU activation.
- **[URPC2020](https://www.kaggle.com/datasets/lywang777/urpc2020):** the underwater object detection dataset.

Credit for these architectures, concepts, code and data remains with their original authors.

## Attribution and licensing

**Wilbert Andrew Yonathan**, AI Engineering student, Xiamen University Malaysia · [GitHub: WAYTECHG](https://github.com/WAYTECHG)

Suggested attribution:

> Wilbert Andrew Yonathan. *Integration of GhostConv and SimSPPF into a Deformable-Upsampling Lightweight Detector for Marine Organism Detection*. Xiamen University Malaysia, AIT304 Computer Vision project.

When referring to this project's adaptations, results, documentation or demo, please credit the author and link to this repository, and credit DU-MobileYOLO and the other upstream work separately.

This attribution statement does not replace the repository's software license or the licenses of upstream components. Keep the relevant copyright and license notices when reusing code, and state any changes you make.
