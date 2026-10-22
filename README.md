# Alzheimer’s Disease Detection Using Deep Learning
A research project exploring binary classification of Alzheimer’s disease (AD) and cognitively normal (CN) subjects from brain MRI images using transfer learning with a pretrained ResNet18 model.

> **Research disclaimer:** This project is for academic and educational research only. It is not a medical device and must not be used to diagnose Alzheimer’s disease or make clinical decisions.

## Project Overview
Alzheimer’s disease can affect a person's brain memory, thinking ablity, and daily activities. This project investigates whether a deep-learning model can distinguish MRI scans associated with Alzheimer’s disease from scans associated with cognitively normal subjects.

The MRI data and corresponding clinical information were obtained from the Alzheimer’s Disease Neuroimaging Initiative (ADNI). Clinical records were used to identify the two classes:
- **CN (Cognitively Normal):** label `0`
- **AD (Alzheimer’s Disease):** label `1`

The project report describes 173 subjects with usable MRI data and 5,190 extracted MRI slices: 4,110 CN slices and 1,080 AD slices.

## Objectives
- Match MRI data with clinical records and assign AD/CN labels.
- Convert and preprocess MRI scans for model training.
- Address class imbalance using data augmentation on the training data.
- Train and compare different ResNet18 transfer-learning experiments.
- Evaluate model predictions using subject-level metrics.
- Explore Grad-CAM visualizations to inspect image regions influencing model predictions.

## Methodology
1. **Data preparation:** Clinical records were filtered for AD and CN labels and matched with available MRI scans.
2. **Conversion and preprocessing:** MRI files were converted to NIfTI using `dcm2niix`, normalized, and processed into 30 middle slices per subject. Slices were resized to 224 × 224 pixels and saved as images.
3. **Data splitting:** Subjects were divided into training, validation, and test sets using a 70% / 15% / 15% split. The project notebooks use subject IDs for the split.
4. **Class imbalance handling:** Augmentation was applied to increase the representation of AD images in the training data.
5. **Model training:** A pretrained ResNet18 model was used for binary classification. Experiments compared the original and augmented datasets, frozen versus partially unfrozen layers, learning rates, weight decay, and decision thresholds.
6. **Evaluation:** Predictions for slices from the same subject were averaged to produce a subject-level prediction. Accuracy, recall, precision, F1-score, AUC, and confusion matrices were used for evaluation.
7. **Explainability:** Grad-CAM is intended to visualize image regions that influence the selected model’s predictions. The Grad-CAM notebook is not currently included in this repository and will be added after the implementation is recovered or recreated and checked.

## Best Experiment Results
The table below summarizes the best reported experiment from each model group in the supplied experiment-results document. Values are reported as recorded in that document.

| Model group | Selected experiment | Main setup | Accuracy | Recall | Precision | F1-score | AUC |
|---|---|---|---:|---:|---:|---:|---:|
| Model 1 | Experiment 1 | Original dataset; frozen ResNet18 backbone; learning rate `1e-4` | 84.62% | 40.00% | 66.67% | 50.00% | 91.43% |
| Model 2 | Experiment 1 | Augmented dataset; frozen ResNet18 backbone; learning rate `1e-4` | **88.46%** | **80.00%** | 66.67% | **72.73%** | **92.38%** |
| Model 3 | Experiment 4 | Augmented dataset; frozen backbone; learning rate `1e-4`; weight decay `1e-5` | 88.46% | 80.00% | 66.67% | 72.73% | 87.62% |

Among these reported experiments, **Model 2 Experiment 1** has the highest reported AUC and ties Model 3 Experiment 4 on accuracy, recall, precision, and F1-score. It is therefore the current candidate for the overall best model and for the planned Grad-CAM analysis. These are results from this project’s test split and should not be interpreted as evidence of clinical performance.

## Repository Structure
```text
alzheimer-mri-classification/
├── README.md
├── .gitignore
├── codes/
│   ├── preprocessing/
│   ├── model_1/
│   ├── model_2/
│   ├── model_3/
│   ├── threshold_tuning/
│   └── grad_cam/   
└── docs/
    ├── project_report.pdf   
    └── model_experiment_results.pdf
```
Only the selected best experiment notebook for each model group is intended to be published, while the experiment-results document records the wider comparison. The final filenames may differ from the example names above.

## Software and Libraries
The project notebooks use Python and include the following libraries/tools:
- PyTorch and Torchvision
- NumPy and Pandas
- scikit-learn
- Matplotlib
- OpenCV and Pillow
- NiBabel
- `dcm2niix` for DICOM-to-NIfTI conversion
- Google Colab / Jupyter Notebook for running notebooks

Exact package versions and a `requirements.txt` will be added after the final selected notebooks are checked.

## Dataset and Reproducibility Notes
- The MRI dataset is **not included** in this repository. Access it through ADNI and follow the dataset’s registration, data-use, and citation requirements.
- The current notebooks contain Google Drive paths specific to the original working environment. Update the dataset paths before running them in another environment.
- Training a model requires the prepared image folders and may benefit from a CUDA-enabled GPU.
- Model weights and datasets are not included unless explicitly added later.
- Grad-CAM implementation and visualizations will be documented after the code has been verified.

## Limitations
- The dataset used in the project is relatively small and imbalanced.
- The approach uses 2D slices and may lose some 3D spatial information from MRI volumes.
- The task is limited to AD versus CN; other stages such as mild cognitive impairment are not included.
- Only ResNet18 was evaluated in this project.
- Test-set results alone do not establish generalization to other populations or clinical settings.

## Acknowledgement
This project was completed as an academic project in the Department of Computer Science and Information Technology, Cotton University. The MRI data source was the Alzheimer’s Disease Neuroimaging Initiative (ADNI).

## Documentation
The project report and detailed experiment-results document will be placed in the `docs/` directory after they are reviewed for public sharing. Personal identifiers, enrollment details, signatures, and other private information should be removed from any report copy published publicly.
