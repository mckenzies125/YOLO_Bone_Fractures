# Bone Fracture Detection with YOLO

This project applies deep learning and computer vision to the detection and localization of bone fractures in X-ray images. Using a pretrained YOLO11 object detection model, we developed an end-to-end pipeline for preparing radiograph data, converting fracture annotations into YOLO-compatible bounding boxes, training the model, and evaluating its ability to identify fractured regions.

The final model achieved **84.2% precision** and **48.4% recall** on validation data.

## Project Overview

Bone fractures can be difficult to identify consistently from medical imaging, creating an opportunity for artificial intelligence to support clinical decision-making. The goal of this project was to investigate whether an object detection model could identify and localize fractures in X-ray images.

Rather than treating fracture detection as a simple image-classification problem, the model predicts **bounding boxes around fractured regions**, allowing it to identify where a suspected fracture occurs within an image.

## Data

The project used X-ray images and fracture annotations from the **FracAtlas** dataset.

The original annotations were provided in COCO format and required preprocessing before they could be used to train YOLO. The project therefore included a custom data-preparation workflow to:

- Validate image and annotation data
- Separate images into predefined training and validation sets
- Convert bounding-box coordinates into normalized YOLO format
- Generate corresponding YOLO label files
- Organize images and labels into the directory structure required by Ultralytics YOLO

Only the fracture class was used for object detection.

## Model

A pretrained **YOLO11n** model was fine-tuned on the fracture dataset using the Ultralytics framework.

Baseline training used:

- 100 epochs
- Batch size of 32
- Pretrained YOLO11n weights
- Reproducible random seed
- Ultralytics' training and validation pipeline

Transfer learning allowed the model to build on features learned during pretraining while adapting its object-detection capabilities to fracture localization.

## Model Optimization

### Hyperparameter Experiments

Experiments were conducted with alternative combinations of:

- Learning rate
- Weight decay
- Momentum
- Optimizer configuration

These experiments did not produce a meaningful improvement over the baseline configuration, so the original training configuration was retained.

### Image Augmentation

Data augmentation was also investigated as a strategy for improving fracture detection, particularly recall.

Because the images represented medical data, transformations were deliberately constrained to preserve the clinical plausibility of the radiographs. Augmentation experiments included limited changes to properties such as image brightness and blur.

The augmented model increased sensitivity to fractures but reduced precision substantially. Because of this precision-recall tradeoff, the baseline model was retained as the final model.

## Results

The final model achieved:

| Metric | Validation Performance |
| --- | ---: |
| Precision | **84.2%** |
| Recall | **48.4%** |

The relatively high precision indicates that predicted fracture regions were often correct when the model detected a fracture. However, the lower recall indicates that the model failed to identify a substantial proportion of fractures.

This tradeoff is particularly important in a medical setting, where missed fractures may have meaningful clinical consequences.

## Model Evaluation

Training and validation metrics were examined to evaluate model behavior and determine whether changes to the training process improved performance.

Precision showed substantially stronger performance than recall. Hyperparameter experiments did not meaningfully improve the baseline model, while augmentation increased recall at the expense of considerable precision.

These findings suggest that simply increasing model flexibility or augmenting the existing images was insufficient to address the primary limitation of the model.

## Limitations and Future Work

The primary limitation was the model's relatively low recall.

A larger and more diverse training dataset would likely provide the model with greater exposure to different fracture appearances and anatomical locations. Future work could therefore include:

- Training on additional labeled X-ray images
- Evaluating alternative YOLO architectures and model sizes
- Investigating class- and confidence-threshold optimization
- Expanding the task to distinguish fractures across different anatomical regions
- Evaluating performance on an independent external dataset

The model should be viewed as an exploratory decision-support application rather than a replacement for clinical interpretation.

## Tools & Technologies

- Python
- PyTorch
- Ultralytics YOLO11
- OpenCV
- Albumentations
- PyLabel
- pandas
- NumPy

## Repository Contents

- `final_project_mckenzie_skrastins,_mimmy_mei,_sophia_huang,_charlotte_li.py` — data preprocessing, model training, optimization experiments, and evaluation
- `YOLO_Bone_Fracture_Presentation.pdf` — project presentation and results
- `README.md` — project overview

## Key Takeaways

This project demonstrates an end-to-end deep learning workflow for medical image object detection, including:

- Preparing and validating image data
- Converting COCO annotations to YOLO-compatible labels
- Fine-tuning a pretrained object detection model
- Conducting hyperparameter experiments
- Designing medically appropriate image augmentation
- Evaluating precision-recall tradeoffs
- Interpreting model limitations in the context of a healthcare application

The final YOLO11 model achieved **84.2% precision and 48.4% recall**, demonstrating strong precision while also highlighting the need for improved sensitivity before a system of this kind could be considered for real-world clinical use.

## Academic Context

This project was completed as a final project for **Introduction to Deep Learning (DIDA 340)** at Binghamton University.
