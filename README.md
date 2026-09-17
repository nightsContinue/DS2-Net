# DS<sup>2</sup>-Net

Dense Spatial Modulation &amp; Sparse Strip Propagation Network
 **空间稠密调制与条带稀疏传播协同的交通标志检测方法**

**DS2Net** is a traffic-sign detector designed for complex road scenes, where small signs, long imaging distances, occlusion, and cluttered backgrounds make reliable detection difficult. The paper has been accepted; formal publication details will be added after they are available.

<p align="center">
  <img src="截图/overall-architecture-DETR-like.png" alt="Overview of the DS2Net architecture" width="100%">
</p>

## Highlights

- **Spatially Adaptive Feature Modulation (SAFM):** enhances sign responses and suppresses distracting background patterns in shallow, high-resolution features with position-dependent modulation.
- **Directional Strip Attention Module (DSAM):** aggregates feature information along horizontal and vertical strips to strengthen edges, contours, and regular geometric structures.
- **Dense modulation followed by sparse propagation:** places SAFM in the shallow backbone and DSAM in the feature-fusion stage, so spatial detail is improved before directional structure is refined.
- **Strong small-object performance:** the method reaches an AP@[0.50:0.95] of **0.710** on TT100K and **0.558** on CCTSDB2021.

## Method

DS2Net uses a two-stage feature enhancement path:

1. **Dense spatial modulation.** SAFM divides projected shallow features into a modulation branch and a skip branch. Multi-scale context produces spatially varying gains for the modulation branch, while the skip branch preserves fine local detail.
2. **Sparse directional propagation.** DSAM applies horizontal and vertical strip aggregation with two receptive-field scales (K = 7 and K = 11). This efficiently emphasizes the directional structures that commonly characterize traffic signs.

The resulting detector retains a high-resolution prediction branch and applies DSAM to fused multi-scale features before detection.

## Results

All results use the COCO evaluation protocol with `maxDets = 100`. `AP_s` denotes AP@[0.50:0.95] for small objects.

### CCTSDB2021

| Method | AP@[0.50:0.95] | AP@0.50 | AP@0.75 | AP_s |
| :-- | --: | --: | --: | --: |
| YOLOv11 | 0.519 | 0.815 | 0.609 | 0.485 |
| YOLO-TS | 0.540 | **0.851** | 0.615 | 0.512 |
| RT-DETR | 0.552 | 0.841 | 0.662 | 0.533 |
| **DS2Net** | **0.558** | 0.837 | **0.676** | **0.545** |

### TT100K

| Method | AP@[0.50:0.95] | AP@0.50 | AP@0.75 | AP_s |
| :-- | --: | --: | --: | --: |
| YOLOv9-C | 0.653 | 0.852 | 0.771 | 0.510 |
| YOLOv11 | 0.650 | 0.850 | 0.770 | 0.435 |
| RT-DETR | 0.657 | 0.855 | 0.769 | 0.516 |
| **DS2Net** | **0.710** | **0.912** | **0.841** | **0.571** |

<p align="center">
  <img src="检测性能对比_紧凑版.png" alt="TT100K detection-performance comparison" width="82%">
</p>

## Experimental Setting

| Item | Setting |
| :-- | :-- |
| Framework | PyTorch 2.0.1 |
| Acceleration | CUDA 11.8, cuDNN, automatic mixed precision |
| Hardware | One NVIDIA GeForce RTX 4070 Super GPU (12 GB) |
| Input resolution | 640 × 640 |
| Training schedule | 200 epochs; 3-epoch linear warm-up; linear learning-rate decay |
| Optimizer settings | Initial learning rate 1e-2; final learning-rate factor 1e-2; momentum 0.9; weight decay 5e-4 |
| Evaluation | COCO AP/AR; `maxDets = 100` |

### Dataset Protocols

- **CCTSDB2021:** 17,856 images in total, using 16,356 images for training and 1,500 images for independent testing.
- **TT100K:** 45 classes with at least 100 instances are retained. The resulting 9,738 images are split into 7,790 training images and 1,948 validation images. This is the paper's custom split; comparisons should use the same split and evaluation protocol.

## Repository Status

This version contains the paper materials, architecture diagram, and experimental visualizations. Training/evaluation code, configuration files, pretrained checkpoints, and an inference demo are **not included** in the current repository version.

When code is released, this section should be updated with:

```text
├── configs/       # model and dataset configurations
├── datasets/      # data preparation instructions
├── models/        # DS2Net implementation
├── tools/         # training, evaluation, and inference scripts
├── weights/       # pretrained checkpoints or download links
└── README.md
```

## Citation

The paper has been accepted. Please replace this section with the official journal citation, DOI, and BibTeX entry once the publication metadata is available.

```bibtex
@article{yang_ds2net,
  title   = {DS2Net: Dense Spatial Modulation and Sparse Strip Propagation Network for Traffic Sign Detection},
  author  = {Yang, Gaoming and Lu, Kaixuan},
  note    = {Accepted; bibliographic details to be added}
}
```

## License

No license file is currently included. Please contact the authors before reusing code, figures, or other project materials.

## Contact

- **Kaixuan Lu** — kxlu@aust.edu.cn
