<h1 align="center">Models for Sezuire Recognition from EEG Recordings</h1>

# BACKGROUND
Seizures occur when groups of neurons in the brain abnormally increase their activity and behave erratically. This can result in convulsive movement, strange behaviors and/or sensations. Individuals experiencing chronic seizures are considered to have epilepsy and require medical treatment. These treatments include surgery, medication and lifestyle changes (Epilepsy and Seizures, 2023). Producing accurate models to detect epilepsy could help these individuals.

Electroencephalogram (EEG) is one of the procedures used to diagnose epilepsy. Neurons function through the controlled movement of ions such as clacium, sodium, potassium and chloride, which produces electrical currents. The EEG device rests on top of the head and uses small electrodes that can detect electrical signals emitted by groups of neurons (Michel & Brunet, 2019). The data produced is voltage changes over time. It should be noted that EEG results alone are not enough to diagnose epilepsy, and that an EEG can only detect neuronal activity at the surface layers of the brain, not in deeper structures (Epilepsy and Seizures, 2023).

# Language
- Python
  - [NumPy](https://numpy.org/)
  - [pandas](https://pandas.pydata.org/)
  - [Seaborn](https://seaborn.pydata.org/)
  - [Matplotlib](https://matplotlib.org/)
  - [Scikit-learn](https://scikit-learn.org/stable/)
  - [TensorFlow](https://www.tensorflow.org/)

# DATA

## Attribute Information
The original dataset from the reference consists of 5 different folders, each with 100 files, with each file representing a single subject/person. Each file is a recording of brain activity for 23.6 seconds. The corresponding time-series is sampled into 4097 data points. Each data point is the value of the EEG recording at a different point in time. So we have total 500 individuals with each has 4097 data points for 23.5 seconds.

We divided and shuffled every 4097 data points into 23 chunks, each chunk contains 178 data points for 1 second, and each data point is the value of the EEG recording at a different point in time. So now we have 23 x 500 = 11500 pieces of information(row), each information contains 178 data points for 1 second(column), the last column represents the label y {1,2,3,4,5}.

The response variable is y in column 179, the Explanatory variables X1, X2, …, X178

y contains the category of the 178-dimensional input vector. Specifically y in {1, 2, 3, 4, 5}:

5 - eyes open, means when they were recording the EEG signal of the brain the patient had their eyes open

4 - eyes closed, means when they were recording the EEG signal the patient had their eyes closed

3 - Yes they identify where the region of the tumor was in the brain and recording the EEG activity from the healthy brain area

2 - They recorder the EEG from the area where the tumor was located

1 - Recording of seizure activity

All subjects falling in classes 2, 3, 4, and 5 are subjects who did not have epileptic seizure. Only subjects in class 1 have epileptic seizure. Our motivation for creating this version of the data was to simplify access to the data via the creation of a .csv version of it. Although there are 5 classes most authors have done binary classification, namely class 1 (Epileptic seizure) against the rest.

## Acknowledgements
Andrzejak RG, Lehnertz K, Rieke C, Mormann F, David P, Elger CE (2001) Indications of nonlinear deterministic and finite dimensional structures in time series of brain electrical activity: Dependence on recording region and brain state, Phys. Rev. E, 64, 061907

The original dataset can be found at the UCI Machine Learning Repository. The dataset used in this project can be found on Kaggle's platform: https://www.kaggle.com/datasets/harunshimanto/epileptic-seizure-recognition

## Data Processing and Normalization
- This project treats the task as a binary classification problem, where seizure activity is labeled as `1` and all non-seizure EEG recordings are collapsed into a single class labeled as `0`.
- The column `Unnamed` was removed, as it contains non-informative metadata related to the recording session rather than EEG signal content.
- Each 1-second EEG window was independently z-score normalized to reduce inter-sample amplitude variability. EEG signal magnitude can vary substantially due to non-neuronal factors such as muscle activity, subject movement, cardiac artifacts, electrode placement, and electrode contact quality (Michel & Brunet, 2019). As a result, per-window normalization is appropriate and helps ensure that model learning focuses on temporal signal patterns rather than absolute amplitude.

<img width="1000" height="600" alt="raw_vs_normalized_traces" src="https://github.com/user-attachments/assets/0838094e-3750-431c-b2fa-e65d40352063" />

> :warning: **Important Note Regarding This Dataset**
> 
> This dataset consists of preprocessed EEG recordings in which the original subject-level structure has been removed.The original data contained recordings from 500 subjects, each segmented into 23 non-overlapping one-second windows, resulting in a total of 11,500 samples. Because explicit subject identifiers are not available, these windows must be treated as independent samples. As a result, multiple samples in the dataset originate from the same individual, but it is not possible to determine which windows belong to which subject. This limitation should be carefully considered when interpreting model performance. Validation results may be optimistic due to potential correlation between samples, and subject-level seizure detection or clinical generalization cannot be reliably assessed using this dataset. Despite these limitations, the dataset is suitable for model development in EEG-based seizure detection.

# Models
This repository contains two primary modeling approaches applied to the EEG seizure dataset.
## 1. Convolutional Neural Network (CNN) (Primary Model)
- Notebook: `eeg_seizure_recognition.ipynb`
- A supervised learning approach for binary seizure detection using 1D convolutional neural networks.
- Includes data preprocessing, normalization, model training, evaluation metrics (accuracy, recall, AUC), and error analysis.
- This notebook represents the most complete and up-to-date modeling work in the repository.
### CNN Architecture
- **Convolutional Layers:**
  - Two 1-dimensional convolutional layers with 32 and 64 filters, respectively, using ReLU activation.
- **Normalization:**
  - Batch normalization applied after each convolutional block to improve training stability.
- **Pooling:**
  - MaxPooling1D applied after each convolutional block to reduce temporal resolution.
  - GlobalAveragePooling1D used prior to the fully connected layer to reduce overfitting.
- **Fully Connected Layers:**
  - A dense layer with 64 units and ReLU activation, followed by dropout for regularization.
  - A final output layer with a single unit and sigmoid activation for binary seizure classification.

## 2. Unsupervised Clustering (Exploratory Analysis)
- Notebook: `SeizureClustering.ipynb`
- An exploratory analysis investigating whether unsupervised clustering methods can reveal structure in EEG windows.
- This work was conducted earlier in the project and served as an initial exploration of the dataset.
- The clustering approach requires further refinement and should be revisited to incorporate improved preprocessing, feature extraction, and evaluation strategies.
  
# Results

## Accuracy, Recall, and AUC per Epoch
<img width="1200" height="500" alt="cnn_model_metrics" src="https://github.com/user-attachments/assets/d21a9f6a-7acb-4d76-b331-603842aa4c6d" />

## Confusion Matrix on Model Validation Data
The confusion matrix indicates that the model misclassified only 11 out of 460 seizure instances as non-seizure (false negatives), while 8 out of 1,840 non-seizure instances were incorrectly classified as seizures (false positives). This corresponds to a high recall for seizure detection, which is particularly important in clinical applications where minimizing missed seizures is critical.
![cnn_confusion_matrix](https://github.com/user-attachments/assets/3bc9387e-a477-48d3-83f4-79040481024a)

# References

Epilepsy and Seizures. (2023, August 15). National Institute of Neurological Disorders and Stroke. Retrieved October 11, 2023, from https://www.ninds.nih.gov/health-information/disorders/epilepsy-and-seizures

Michel, M., & Brunet, D. (2019). EEG Source Imaging: A Practical Review of the Analysis Steps. Frontiers in Neurology, 10, 325. https://doi.org/10.3389/fneur.2019.00325

# Author

Michael Grybko - GitHub username: grybkom
