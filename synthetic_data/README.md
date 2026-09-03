## Overview
- This repository provides three synthetic datasets for evaluating causal, explainable AI (causal XAI) methods for tabular data.
- For each dataset, the variables or combinations of variables that determine the target variable are predefined, allowing quantitative evaluation of whether an XAI method can correctly identify them.
- The following three datasets are provided:
  - `NonLinear`: Data with nonlinear and additive relationships
  - `NonAdditive`: Nonlinear and non-additive data in which the combination of important variables changes depending on the conditions
  - `Category`: Non-additive data in which the target variable is determined by a combination of categorical variables
- Each dataset consists of 10 explanatory variables and one target variable.
- Each dataset directory also contains `train.csv` for model training, `valid.csv` for model selection, and `test.csv` for final evaluation.
- These datasets were used in the experiments reported in our paper. To cite this work, see the `How to Cite` section at the end of this README.


## Directory Structure
- The three datasets are stored in the `data` directory as follows:
```
data/
├── NonLinear/        # Data for the Non-Linear and Additive experiment
│   ├── train.csv
│   └── valid.csv
│   └── test.csv
├── NonAdditive/      # Data for the Non-Linear and Non-Additive experiment
│   ├── train.csv
│   └── valid.csv
│   └── test.csv
└── Category/         # Data for the Category experiment
    ├── train.csv
    ├── valid.csv
    └── test.csv
```

## Experimental Procedure
The general experimental procedure is outlined below. Detailed procedures are provided in the section for each dataset.
1. Train a binary classification model using `train.csv` and `valid.csv`.
2. Apply an XAI method to the trained model for each sample in `test.csv` and compute an importance score for each variable.
3. Rank the variables or sets of variables in order of highest to lowest causal importance. Refer to the section for each dataset for details on the rankings.
4. Refer to the rank of the ground-truth variable or set of the variables pre-defined for each dataset. Dataset-specific details are provided in the corresponding sections.
5. Calculate the average rank over all test samples.

### Non-Linear Dataset
- In this dataset, the target variable is generated from a nonlinear additive function of multiple explanatory variables.
- X1–X4 are used to generate the target variable, whereas X5–X10 are not.
- Evaluation method
  - For each test sample, rank the explanatory variables in descending order of importance based on the scores produced by the XAI method.
  - Calculate the mean rank of the ground-truth variables (X1, X2, X3, and X4) for that sample. Average this value over all test samples to obtain the final evaluation metric.
  - A lower mean rank indicates that the XAI method ranks the ground-truth variables more accurately.
- Example
  - Suppose that, for a given test sample, X1-X10 are ordered by importance as follows: ```X2, X3, X6, X7, X1, X4, X10, X9, X8, X5```.
  - In this case, the ranks of X1-X4 are X1: 5th, X2: 1st, X3: 2nd, and X4: 6th.
  - Therefore, (5 + 1 + 2 + 6) / 4 = 3.5 is the average rank for this test sample.
  - Repeat this calculation for all test samples. The overall average is the final evaluation metric.

### Non-Additive Dataset
- In this dataset, the target variable is determined by non-linear and non-additive combinations of multiple explanatory variables.
- The combination of variables that affects the target variable changes according to the value of X1, as shown below. X8-X10 are not used to generate the target variable.
  - If X1 > 7: X1 and X2
  - If 4 < X1 ≤ 7: X1 and X3
  - If 0 < X1 ≤ 4: X1 and X4
  - If -4 < X1 ≤ 0: X1 and X5
  - If -7 < X1 ≤ -4: X1 and X6
  - If X1 ≤ -7: X1 and X7
- Therefore, there is no single ground-truth variable set shared by all samples. Instead, the ground-truth variable pair differs for each sample according to the value of X1.
- Evaluation method
  - For each test sample, calculate the combination importance produced by the XAI method for every pair of the 10 explanatory variables. For methods that calculate importance for individual variables, use the sum of the two variable importance scores for each pair.
  - There are 45 possible pairs of two variables.
  - Sort these 45 variable pairs by importance in descending order and determine the rank of the ground-truth pair for that sample.
  - Repeat this for all test samples and use the average rank of the ground-truth pair as the final evaluation metric.
  - A lower average rank for the ground-truth pair indicates that the XAI method identifies the combination of variables affecting the target variable more accurately.
- Example
  - Suppose that X1 has a value of 5 for a given test sample.
  - Because 4 < X1 ≤ 7, the ground-truth variable pair is (X1, X3).
  - Suppose the variable-pair importance ranking produced by the XAI method is `(X1, X2), (X2, X4), (X1, X3), (X3, X5), ...` from highest to lowest.
  - In this case, the ground-truth pair (X1, X3) is ranked 3rd.
  - Repeat this for all test samples. The average rank of the ground-truth pair is the final evaluation metric.

### Category Dataset
- This dataset consists of 10 categorical variables, and the target variable is determined non-additively by the combination of X1, X2, and X3.
- X4-X10 are not used to generate the target variable.
- Evaluation method
  - For each test sample, calculate the combination importance produced by the XAI method for every combination of three variables among the 10 explanatory variables. For methods that calculate importance for individual variables, use the sum of the three variable importance scores for each combination.
  - There are 120 possible combinations of three variables.
  - Sort these 120 combinations by importance in descending order and determine the rank of the ground-truth variable combination (X1, X2, X3).
  - Repeat this for all test samples and use the average rank of (X1, X2, X3) as the final evaluation metric.
  - A lower average rank for the ground-truth combination indicates that the XAI method identifies the important combination of variables more accurately.
- Example
  - Suppose that, for a given test sample, the three-variable combinations are ordered by importance as follows: `(X1, X2, X4), (X2, X3, X5), (X1, X2, X3), (X1, X3, X6), ...`.
  - In this case, the ground-truth variable combination (X1, X2, X3) is ranked 3rd.
  - Repeat this for all test samples. The average rank of the ground-truth combination is the final evaluation metric.




## Our Experimental Results
- The table below presents the experimental results reported in our paper.
- The values indicate the mean and standard deviation. Lower is better. Lower is better, and bold represents the best results in each setting.

|  | Non-Linear | Non-Additive | Category |
|---|---|---|---|
| LIME | 3.31 ± 0.71 | 18.76 ± 13.17 | 33.69 ± 32.56 |
| S-LIME | **2.99 ± 0.52**  |13.72 ± 12.24  |33.62 ± 32.69 |
| SHAP | 3.11 ± 0.58 |  6.74 ± 9.98 | 9.57 ± 7.83 |
| ACV | 4.49 ± 1.77 | 14.63 ± 12.78 |15.53 ± 19.62  |
| CENNET | 3.17 ± 0.55 | **3.12 ± 3.44** | **4.62 ± 4.49** |



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
