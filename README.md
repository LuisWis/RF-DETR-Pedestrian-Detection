# RF-DETR Small Pedestrian Detection

## Project Overview

This project fine-tunes RF-DETR Small for single-class pedestrian object detection using a chronologically partitioned video-frame dataset.

## Dataset

- Source: Previously annotated pedestrian video frames
- Total original images: 1,279
- Training images: 867
- Validation images: 185
- Testing images: 187
- Excluded temporal buffer images: 40
- Object class: person
- Annotation format: YOLO bounding boxes

All frames originate from one continuous video. Consequently, the evaluation assesses performance on later frames from the same scene, rather than an independent camera environment.

The dataset archive is not included in this repository. Obtain the original annotated dataset separately and follow the notebook's dataset preparation procedure.

## Model

- Architecture: RF-DETR Small
- Framework: PyTorch and RF-DETR
- Initial weights: Official pretrained RF-DETR Small checkpoint
- Input resolution: 512 x 512
- Fine-tuning epochs: 20
- Physical batch size: 4
- Gradient accumulation steps: 4
- Baseline learning rate: 0.0001
- Optimization experiment learning rate: 0.00005

## Google Colab Setup

1. Open the accompanying `.ipynb` notebook in Google Colab.
2. Enable an NVIDIA GPU runtime.
3. Install the required dependencies using the notebook setup cells.
4. Mount Google Drive and prepare the annotated pedestrian dataset.
5. Run the dataset verification and chronological splitting cells.
6. Run RF-DETR training using the baseline hyperparameters.
7. Run validation analysis and evaluate the trained checkpoint.
8. Optionally reproduce the second learning-rate experiment.

The notebook uses a Google Drive project directory. Adjust its paths to match your own Drive before running the code.

For compatible PyTorch and CUDA installation instructions, consult the official PyTorch documentation.

## Evaluation

The selected baseline model was evaluated on 187 chronologically reserved test images with 535 ground-truth pedestrian annotations.

At confidence threshold 0.30 and matching IoU threshold 0.50:

- True positives: 328
- False positives: 160
- False negatives: 207
- Precision: 67.21%
- Recall: 61.31%
- F1 score: 64.13%
- Mean matched IoU: 75.46%

COCO evaluation:

- mAP@0.50: 65.87%
- mAP@0.75: 28.19%
- mAP@0.50:0.95: 32.77%

Matched-object classification accuracy was 100% among 328 matched detections. This is a single-class task and is not overall detection accuracy.

## Learning Rate Comparison

The baseline achieved a best validation EMA mAP@0.50:0.95 of 37.29%, compared with 36.98% for the learning rate 0.00005 experiment.

The baseline was retained using the validation metric as the selection criterion.

## Repository Contents

- Notebook: Complete data preparation, training, and evaluation workflow
- `configs/`: Hyperparameter configurations
- `results/`: Evaluation and comparison tables
- `figures/`: Generated experimental visualizations
- `requirements.txt`: Recorded dependency versions

## Limitations

The dataset originates from a single camera recording, and the training and testing images share scene characteristics. Small and distant pedestrians were more difficult to detect. Independent-camera validation and additional datasets are recommended for future work.
