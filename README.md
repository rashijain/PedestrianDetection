# Pedestrian Detection with YOLOv8

This repository contains the model training architecture and evaluation metrics for a high-resolution pedestrian detection system built with YOLOv8. Designed for autonomous driving perception and urban surveillance contexts, the model is optimized to detect pedestrians in dense, variable street environments.

### ⚠️ Repository Context: Experimental Artifact
**Please note:** The Jupyter Notebook provided (`PedestrianDetection_YOLO_GPU_Nov25.ipynb`) is uploaded as a **raw training artifact**. 

It is an unedited experimental scratchpad used for rapid hyperparameter tuning and model convergence testing. As such, it contains unfiltered procedural code, localized data mounting commands, and extensive training logs. It is provided here strictly as transparent proof-of-work for the model's training pipeline and the final validation metrics, rather than as an example of refactored, production-ready software.

## Dataset & Preprocessing Pipeline
*(Note: The preprocessing pipeline was executed in a separate, localized environment prior to this training run.)*

The model utilizes high-resolution urban street scenes from Cityscapes dataset. A custom data preprocessing pipeline was built to translate the raw annotation data into a model-ready YOLO format:

* **Annotation Extraction & Normalization:** Parsed complex JSON annotation trees to extract visible bounding box (`bboxVis`) coordinates. Absolute pixel values (X, Y, Width, Height) were mathematically converted into normalized YOLO format relative to the original image dimensions.
* **Semantic Label Mapping:** Pedestrian objects were mapped to the primary positive class. Regions labeled as "ignore" (e.g., highly dense crowds, reflections, or ambiguous shapes) were systematically filtered out to prevent noisy gradients during backpropagation.
* **Geographic Stratification:** To ensure robust evaluation and prevent spatial data leakage, the train/validation split was strictly separated by geographic location. Images from specific cities were isolated purely for validation, while the remaining cities were utilized for the training corpus.

## Technical Overview
* **Architecture:** YOLOv8 Large (`yolov8l.pt`)
* **Input Resolution:** 1024x1024 (multi-scale training enabled to preserve the spatial resolution and feature maps of small, distant pedestrians)
* **Optimization:** AdamW optimizer with a base learning rate of 0.001
* **Augmentation:** Aggressive augmentation strategy (including Mosaic and Mixup) to force the model to learn partial object features and generalize against urban overfitting. 

## Results
The validation metrics for this model run are preserved in the execution outputs of the `PedestrianDetection_YOLO_GPU_Nov25.ipynb` notebook. The metrics for pedestrian detection are as follows:

* **mAP@50:** 65.5%
* **mAP@50-95:** 39.6%
* **IoU:** 66.9%
