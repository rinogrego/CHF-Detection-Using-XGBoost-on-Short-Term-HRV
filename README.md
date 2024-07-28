# Congestive Heart Failure Detection Using XGBoost on Short-Term Heart Rate Variability

My undergraduate thesis project: Analysis of XGBoost Model Performance to Detect Congestive Heart Failure on Short-term Heart Rate Variability

This repository contains:

- scripts to collect data from [PhysioNet](https://physionet.org)
- collected and cleaned HRV data for short-term and long-term HRV from electrocardiogram (ECG) signals
- exploratory data analysis (long-term HRV only)
- experiment scripts to load, preprocess, train, and validate the model

## What is CHF, ECG, and HRV?

Congestive heart failure (CHF) is a type of heart disease where the heart cannot properly pump the blood to the entire body. This causes some parts of the body to not be able to receive enough nutrients for the cells since the blood is not delivered properly.

Heart rate can be represented by signals, captured using electrocardiogram (ECG) recordings. The recording length may take from few minutes to an entire day (or more). From the captured ECG signals, we can capture various informations regarding heart rate, such as its variation or usually called heart rate variability (HRV). HRV computed from long-term recordings is called long-term HRV and hRV computed from short-term recordings is called short-term HRV. The drop in HRV is correlated with various heart diseases, with CHF is found to be one of them. HRV indices can be extracted from ECG signals, which then can be used to predict the existence of a heart disease.

Normal sample ECG recording <br>
<img src="images/normal sample.PNG" width="50%" height="50%">

CHF sample ECG recording <br>
<img src="images/CHF sample.PNG" width="50%" height="50%">

## Data Cleaning and Preprocessing

- database source: CHFDB, CHF2DB, NSRDB, and NSR2DB from [PhysioNet](https://physionet.org)
- data collected using [wfdb](https://physionet.org/content/wfdb-python/4.1.0/) library
- 5 minutes HRV computed from segmented long-term ECG signals
- ectopic beats are handled using linear interpolation
- signal resampling according to frequency sample information provided by data source
- outliers (below 300 ms or above 2000 ms) are replaced using linear interpolation
- HRV indices are extracted using [hrv-analysis](https://pypi.org/project/hrv-analysis/) library
- train test split ratio: 70:30 with stratify sampling based on the target variable (CHF)

## Experiment

- Model: XGBoost
- Stratified K-Fold Cross Validation with K = 10 and Grid Search method to search the best hyperparameter combination
- Trying 4 different Grid Search objectives: accuracy, sensitiviy, specificity, and AUC score
- Hyperparameter Search Space:

| hyperparameter | Values |
| -- | -- |
| learning_rate | 0.2, 0.3, 0.4 |
| n_estimators | 50, 75, 100, 125, 150, 175, 200 |
| max_depth | 3, 4, 5, 6 |
| min_child_weight | 1, 5, 10 |
| gamma | 1, 2 |
| subsample | 0.6, 0.8, 1.0 |
| colsample_bytree | 0.6, 0.8, 1.0 |

## Results

- Best Grid Search objective: sensitivity
  - Accuracy: 0.966
  - Sensitivity: 0.977
  - Specificity: 0.963
  - AUC score: 0.970
- Most important features across experiments: Age, pNNI_20, LF/HF, NNI_20

Partial results/analysis can be found on my conference paper: [Automated Congestive Heart Failure Detection Using XGBoost on Short-term Heart Rate Variability](https://ieeexplore.ieee.org/document/10381940)