# Automatic Image Colorization using SVR + MRF

## 1. Project Overview

This project focuses on automatic image colorization using Machine Learning and image processing techniques. It takes a grayscale image and predicts suitable colors to generate a colorized image.

The project uses Support Vector Regression (SVR) to predict color information and a Markov Random Field (MRF) approach to improve color consistency between neighboring image regions.

## 2. Objectives

- Convert grayscale images into colorized images.
- Predict color information using Support Vector Regression (SVR).
- Improve spatial consistency using MRF-based smoothing.
- Compare the results with a baseline approach.
- Improve model performance through feature selection and parameter tuning.
- Demonstrate the colorization process step by step.

## 3. Technologies and Methods

- **Python** – Main programming language
- **Google Colab / Jupyter Notebook** – Development environment
- **NumPy** – Numerical computations and array processing
- **Scikit-learn** – Machine Learning and SVR
- **SLIC Superpixels** – Divide an image into meaningful regions
- **YUV Color Space** – Represent brightness and color information separately
- **RBF Kernel SVR** – Predict the U and V color components
- **MRF and ICM** – Improve consistency between neighboring regions

## 4. Dataset

The project uses the **BSDS500 dataset**, which contains natural images suitable for image-processing experiments.

The dataset was organized into training, validation, and test sets:

- Training: 200 images
- Validation: 100 images
- Test: 200 images

The training and validation sets were used during development. The test set was reserved for final evaluation and should be evaluated separately before reporting final test performance.

## 5. Project Workflow

1. Load an input image.
2. Prepare grayscale and color representations.
3. Convert the image to the YUV color space.
4. Divide the image into regions using SLIC superpixels.
5. Extract features from the image regions.
6. Train separate SVR models to predict the U and V color components.
7. Apply MRF-based processing using Iterated Conditional Modes (ICM) to improve spatial color consistency.
8. Convert the predicted color information back to RGB.
9. Display the original image, intermediate processing stages, and final colorized output.

## 6. Version History and Improvements

### Initial Version

The initial version used a feature set containing **236 features** for each image region. These included raw image patches, frequency information, statistical measurements, percentiles, histogram information, gradients, texture features, multi-scale features, and spatial coordinates.

Separate RBF-kernel SVR models were trained to predict the U and V color components.

**Initial results:**

| Metric | Result |
|---|---:|
| U-component MAE | 0.043727 |
| V-component MAE | 0.053654 |
| Average MAE | 0.048691 |
| Baseline average MAE | 0.038742 |

The initial model performed worse than the baseline. Visual inspection also showed issues such as green tinting, color bleeding, blocky superpixel boundaries, and low saturation.

### Improved Version

To improve performance, we reduced the feature set from 236 features to **20 selected features**.

The main improvements were:

- Used Ridge-based feature selection on a sample of 5,000 training examples to identify a smaller set of useful features.
- Removed the raw image patch, FFT, and histogram feature groups from the selected feature set.
- Retained selected statistical, percentile, gradient, texture, multi-scale, and coordinate features.
- Used separate RBF-kernel SVR models for the U and V color components.
- Tuned the SVR parameters to `C = 1`, `gamma = 0.001`, and `epsilon = 0.01`.

**Improved results:**

| Metric | Result |
|---|---:|
| U-component MAE | 0.031842 |
| V-component MAE | 0.038721 |
| Average MAE | 0.035281 |
| Baseline average MAE | 0.038742 |
| Improvement over baseline | Approximately 8.93% |

The improved model reduced the average MAE compared with both the initial version and the baseline.

**Note:** Lower MAE indicates lower average prediction error. The reported improvement is based on the recorded U and V validation errors; final test-set evaluation is still needed before making claims about generalization to unseen test images.

## 7. Repository Structure

```text
ML-miniproject/
├── Automatic_Images_Colorization.ipynb
├── README.md
└── artifacts/
    ├── final_model_package.zip
    └── version1_feature_data.npz
```

### Files and Folders

- **`Automatic_Images_Colorization.ipynb`** — Main notebook containing the implementation, experiments, and evaluation.
- **`artifacts/final_model_package.zip`** — Package containing the final trained models and associated supporting files.
- **`artifacts/version1_feature_data.npz`** — Saved feature arrays used by the initial feature-data workflow.
- **`README.md`** — Project documentation.

## 8. How to Get Started

1. Clone or download this repository.
2. Open `Automatic_Images_Colorization.ipynb` in Google Colab or a compatible Jupyter environment.
3. Install any required Python libraries that are not already available.
4. Make the required dataset and supporting files available in the execution environment.
5. Check the file paths in the notebook and update them to match your environment.
6. Run the notebook cells in the required order.

**Important:** The notebook currently uses Colab-specific paths. For example, the initial feature-data workflow expects `/content/version1_feature_data.npz`, while the file in this repository is stored under `artifacts/`. Update the path or copy the file to the expected location before running that cell.

## 9. Expected Output

The notebook demonstrates the image-colorization pipeline, including grayscale conversion, SLIC superpixel segmentation, SVR-based color prediction, MRF-based refinement, and the final colorized image.

The notebook also records model evaluation metrics and comparisons with the baseline.

## 10. Current Status and Limitations

The repository contains the main notebook, the saved feature data, and the final model package.

The improved model achieved a lower average validation MAE than the baseline. However, further evaluation is needed on the reserved test set.

Visual limitations may include color bleeding, inaccurate color predictions, blocky region boundaries, and insufficient saturation.

## 11. Future Improvements

- Evaluate the improved model on the reserved test set.
- Reduce color bleeding and unwanted color artifacts.
- Improve color consistency across image regions.
- Explore additional feature-selection and SVR-tuning strategies.
- Improve the reproducibility of the training and testing workflow.
- Investigate alternative colorization approaches for better visual quality.
