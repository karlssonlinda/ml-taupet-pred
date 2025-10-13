Author: Linda Karlsson, 2025.

# Predict tau-PET models

This repo contains the final models for predicting tau-PET machine learning (ML) composites in https://doi.org/10.1002/alz.14600. 

## Overview
The models are trained on data from BioFINDER-2 data and use three predefined input variable blocks: clinical, plasma, and MRI, as described in the paper. 

**Outcome variables**

  1. Tau-PET Braak I–IV load
  2. Tau-PET Braak I–IV asymmetry (laterality index)
 
For tau load, models were trained on all possible combinations of the three input blocks (seven models in total). Users can select the model corresponding to the input data available. The combined model (clinical + plasma + MRI, tau_load/model7) achieved the best performance, primarily driven by the plasma features. However, because plasma biomarkers are not always available, we also share all alternative models for flexibility (tau_load/model1-model6).
 
For asymmetry, only the MRI-based model (tau_asymmetry/model1) is shared, since MRI variables were the only meaningful predictors, adding other features did not improve performance. Note that the laterality index has only been trained on tau-positive individuals which is the recommended use population during prediction of this composite.

 **Input variables**
 
Input files should be in tabular format (e.g., csv) and include clinical, plasma and/or MRI variables as columns. See an example dataframe with simulated data (df_simulated.csv) and try the models on it using the notebook eval_example.ipynb.

1. Clinical variables: numeric (age [years], sex [0=male, 1=female], education [years], mmse score, 10 word recall score, number of APOE ε2 alleles, number of APOE ε4 alleles).
2. Plasma variables: numeric (p-tau217, p-tau181, p-tau231, NTA, GFAP, NfL).
3. MRI: numeric (FreeSurfer-derived volumes, surface areas, thickness, in total 247 variables)
 
Input variables for each model have been saved as a separate .pkl file. Missing values or columns can be imputed using a trained KNN imputer (n_neighbors = 5) that is included for each model, though this should be applied cautiously, especially when a large proportion of data is missing. 
 
The model output is a continuous numeric variable representing the predicted tau-PET Braak I–IV load or laterality index. The output is saved as an additional column in the original tabular data frame and saved as a .csv file.

**Directory navigation**

tau_load (model1: clinical, model 2: plasma, model 3: MRI, model4: clinical+plasma, model5: clinical+MRI, model6: plasma+MRI, model7: clinical+plasma+MRI.

tau_asymmetry: (model1: MRI)

## Typical workflow

1.	**Data preparation**: Assemble tabular input data (clinical, plasma, and/or MRI), including FreeSurfer parcellations for MRI.
2.	**QC and curation**: Harmonize variable names with the provided input list. Handle missing data via the paired KNN imputer if necessary.
3.	**Preprocessing**: No manual scaling required, z-score standardization (fitted on training data) is integrated within the model pipeline.
4.	**Prediction**: Load the desired model (.pkl) and apply it to new participants to generate predictions.
5.	**Evaluation and visualization**: We have included an example Jupyter notebook (eval_example.ipynb) that can be used to demonstrate prediction and inference with simulated data (including model performance with MAE and R² metrics and visualization of predicted vs. observed values through Matplotlib scatterplots).

## Dependencies

  - python=3.9
  - pandas=1.4.4
  - scikit-learn=1.1.2
  - numpy=1.23.3
  - matplotlib=3.5.3

