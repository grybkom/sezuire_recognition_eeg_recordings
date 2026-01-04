<h1 align="center">Models for Sezuire Recognition from EEG Recordings</h1>

# BACKGROUND
Seizures occur when groups of neurons in the brain abnormally increase their activity and behave erratically. This can result in convulsive movement, strange behaviors and/or sensations. Individuals experiencing chronic seizures are considered to have epilepsy and require medical treatment. These treatments include surgery, medication and lifestyle changes (Epilepsy and Seizures, 2023). Producing accurate models to detect epilepsy could help these individuals.

Electroencephalogram (EEG) is one of the procedures used to diagnose epilepsy. Neurons function through the controlled movement of ions such as clacium, sodium, potassium and chloride, which produces electrical currents. The EEG device rests on top of the head and uses small electrodes that can detect electrical signals emitted by groups of neurons (Rayi, 2022). The data produced is voltage changes over time. It should be noted that EEG results alone are not enough to diagnose epilepsy, and that an EEG can only detect neuronal activity at the surface layers of the brain, not in deeper structures (Epilepsy and Seizures, 2023).

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

## Important Note Regarding This Dataset

Since subject identifiers are not available, samples need to be treated as independent windows.

The data has been processed so 500 subjects × 23 windows resulting in 11500 samles. Therefore, there are multiple entries from the same subject and it is not possilbe to determine which entries are from the same subject. This needs to be kept in mind when deriving any clinical implcations from this work and sub-level sezuire detection cannot be accopmished.  

## Acknowledgements

Andrzejak RG, Lehnertz K, Rieke C, Mormann F, David P, Elger CE (2001) Indications of nonlinear deterministic and finite dimensional structures in time series of brain electrical activity: Dependence on recording region and brain state, Phys. Rev. E, 64, 061907

The original dataset can be found at the UCI Machine Learning Repository. The dataset used in this project can be found on Kaggle's platform: https://www.kaggle.com/datasets/harunshimanto/epileptic-seizure-recognition
