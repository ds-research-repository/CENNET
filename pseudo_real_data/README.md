## Overview
- This directory provides pseudo real datasets for evaluating causal, explainable AI (causal XAI) methods for tabular data.
- For each dataset, the variables or combinations of variables that determine the target variable are predefined, allowing quantitative evaluation of whether an XAI method can correctly identify them.
- Each dataset directory contains `train.csv` for model training, `valid.csv` for model selection, and `test.csv` for final evaluation.
- These datasets were used in the experiments reported in our paper. To cite this work, see the `How to Cite` section at the end of this README.

## Experimental Procedure
The general experimental procedure is outlined below. An example from the Alarm (SHUNT) dataset is also provided below.
1. Train a binary classification model using `train.csv` and `valid.csv`.
2. Apply an XAI method to the trained model for each sample in `test.csv` and compute an importance score for each variable.
3. Rank the variables or sets of variables in order of highest to lowest causal importance. Details regarding the ranking are provided in Example section. 
4. Refer to the rank of the ground-truth variable or set of the variables pre-defined for each dataset. The ground-truth variables (causal explanatory variables) for each dataset are summarized in the following table.
5. Calculate the average rank over all test samples.

### Example (Alarm(SHUNT))
- For each test sample, calculate the combination importance produced by the XAI method for every combination of two variables among the explanatory variables. For methods that calculate importance for individual variables, use the sum of the two variable importance scores for each combination.
- Sort these combinations by importance in descending order and refer to the rank of the ground-truth variable combination (PULMEMBOLUS, INTUBATION).
- Suppose that, for a given test sample, the two variable combinations are ordered by importance as follows: `(PULMEMBOLUS, PAP), (PULMEMBOLUS, INTUBATION), (PAP, INTUBATION), (VENTLUNG, INTUBATION), ...`.
- In this case, the ground-truth variable combination (PULMEMBOLUS, INTUBATION) is ranked 2nd.
- Repeat this for all test samples. The average rank of the ground-truth combination is the final evaluation metric.


### The target variable and causal explanatory variables for each dataset
| Dataset | Target variable | Causal explanatory variables |
|---|---|---|
| Alarm (SHUNT) | SHUNT | PULMEMBOLUS, INTUBATION |
| Alarm (HISTORY) | HISTORY| LVFAILURE |
| Alarm (CATECHOL) | CATECHOL | ARTCO2, SAO2, TPR | 
| Hailfinder (WindFieldMt) | WindFieldMt | Scenario |
| Hailfinder (ScenRelAMCIN) | ScenRelAMCIN | Scenario |
| Insurance (Othercar) | Othercar | SocioEcon |
| Insurance (Vehicleyear) | Vehicleyear | SocioEcon, RiskAversion |
| Insurance (Antilock) | Antilock | VehicleYear, MakeModel |
| Insurance (Airbag) | Airbag | VehicleYear, MakeModel |
| Carpo (N42) | N42 | N19, N27, N29 | 

## Our Experimental Results
- The table below presents the experimental results reported in our paper.
- The values indicate the mean and standard deviation. Lower is better, and bold represents the best results in each setting.


| Dataset | LIME | S-LIME | SHAP | ACV | CENNET |
|---|---:|---:|---:|---:|---:|
| Alarm (SHUNT) | 6.96 ± 3.50 | **6.93 ± 3.39** | 37.50 ± 10.27 | 22.41 ± 12.49 | 28.31 ± 9.19 |
| Alarm (HISTORY) | **1.00 ± 0.00** | **1.00 ± 0.00** | 1.64 ± 0.85 | 3.26 ± 1.71 | 3.79 ± 0.84 |
| Alarm (CATECHOL) | 103.18 ± 69.98 | 102.12 ± 68.05 | 96.74 ± 96.11 | 118.11 ± 87.88 | **24.75 ± 30.08** |
| Hailfinder (WindFieldMt) | 7.69 ± 4.92 | 7.65 ± 4.94 | 7.54 ± 4.22 | 7.11 ± 4.10 | **2.13 ± 2.63** |
| Hailfinder (ScenRelAMCIN) | 9.06 ± 4.49 | 9.25 ± 4.52 | 7.72 ± 3.76 | 7.96 ± 5.67 | **1.43 ± 0.94** |
| Insurance (Othercar) | 4.32 ± 4.64 | 4.30 ± 4.60 | 3.49 ± 3.23 | 4.86 ± 3.60 | **2.28 ± 2.26** |
| Insurance (Vehicleyear) | 79.21 ± 19.65 | 78.36 ± 18.80 | 70.72 ± 16.17 | 43.74 ± 29.26 | **29.14 ± 5.95** |
| Insurance (Antilock) | 41.22 ± 12.61 | 41.15 ± 12.52 | 15.89 ± 13.96 | 31.86 ± 17.97 | **8.63 ± 6.99** |
| Insurance (Airbag) | 12.69 ± 6.27 | 12.80 ± 6.34 | 4.74 ± 1.71 | 13.35 ± 7.17 | **3.54 ± 4.08** |
| Carpo (N42) | 111.42 ± 22.45 | 111.61 ± 22.28 | 95.36 ± 30.02 | **81.69 ± 42.58** | 86.23 ± 29.11 |


## How to Cite
```
@article{cennet2025,
title = {A Model of Causal Explanation on Neural Networks for Tabular Data},
journal = {Procedia Computer Science},
volume = {264},
pages = {65-79},
year = {2025},
doi = {https://doi.org/10.1016/j.procs.2025.07.119},
url = {https://www.sciencedirect.com/science/article/pii/S1877050925021696},
author = {Takashi Isozaki and Masahiro Yamamoto and Atsushi Noda}
}
```
