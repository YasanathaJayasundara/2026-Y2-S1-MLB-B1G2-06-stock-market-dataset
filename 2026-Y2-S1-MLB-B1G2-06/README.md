# Stock Market Direction Prediction

**Module:** IT2011 - Artificial Intelligence and Machine Learning  
**Institution:** Sri Lanka Institute of Information Technology (SLIIT)  
**Group ID:** `2026-Y2-S1-MLB-B1G2-06`  
**Project status:** Completed and evaluated

This project applies six supervised machine-learning algorithms to predict the direction of the next closing price for international stock-market indices.

> **Important:** This is an academic machine-learning project. Its predictions must not be interpreted as financial or investment advice.

## Project Objective

The task is formulated as binary classification:

| Target | Meaning |
|---:|---|
| `0` | The next closing price decreased or remained unchanged |
| `1` | The next closing price increased |

For every market index, the target was created by comparing its next recorded closing price with its current closing price.

## Dataset Summary

| Property | Value |
|---|---|
| Original dataset size | 112,457 rows and 8 source columns |
| Market coverage | 14 international market indices |
| Source attributes | Date, Open, High, Low, Close, Adjusted Close, Volume and Index |
| Rows with missing required values | 2,219 (approximately 1.97%) |
| Exact duplicate rows | 0 |
| Invalid OHLC records removed | 1 |
| Final cleaned dataset | 110,223 rows and 10 columns, including engineered fields |
| Class 0 records | 51,715 |
| Class 1 records | 58,508 |

The final chronological partitions contain:

| Partition | Records | Purpose |
|---|---:|---|
| Training | 77,150 | Fit preprocessing steps and models |
| Validation | 16,534 | Compare baseline and tuned configurations |
| Testing | 16,539 | Perform one final unbiased evaluation |

## Group Members and Contributions

| Student ID | Student Name | Preprocessing Contribution | Model |
|---|---|---|---|
| IT25102978 | Jayasundara J.M.Y.V. | Missing-data handling | Logistic Regression |
| IT25102165 | Halangoda R.W.W.M.M.C. | Categorical encoding | Support Vector Machine |
| IT25200808 | Shavindi T.D.P. | Outlier handling | Decision Tree |
| IT25101173 | Randil B.R.K. | Feature scaling | K Nearest Neighbors |
| IT25200151 | Wanniarchchi W.K.A.N. | Feature engineering | Random Forest |
| IT25102034 | Heshan I.A.M. | Feature selection | Gradient Boosting |

## Integrated Preprocessing Pipeline

The combined preprocessing notebook performs the following operations:

1. Validates the required columns and converts `Date` to a consistent date type.
2. Removes incomplete and invalid financial records and checks for duplicates.
3. Constructs the next-day direction target separately within each market index.
4. Creates calendar features: year, month, day and day of week.
5. Creates chronological 70% training, 15% validation and 15% testing partitions within each index.
6. Learns IQR outlier limits from the training data and applies them to all partitions.
7. One-hot encodes the market-index category and aligns the resulting columns.
8. Fits `StandardScaler` only on the training data and transforms validation and test data using the same scaler.
9. Fits `SelectKBest` only on the training data and retains the five highest-scoring predictors.

Fitting the outlier limits, scaler and feature selector only on training data prevents future information from leaking into model development.

### Final Selected Features

| Feature | Meaning |
|---|---|
| `Volume_Available` | Indicates whether a positive volume value was reported |
| `Day` | Standardized calendar day of the month |
| `Index_399001.SZ` | One-hot indicator for the Shenzhen Component Index |
| `Index_IXIC` | One-hot indicator for the NASDAQ Composite Index |
| `Index_TWII` | One-hot indicator for the Taiwan Weighted Index |

## Models

Six classifiers were trained and evaluated on the same processed data:

- Logistic Regression
- Support Vector Machine (linear SVM)
- Decision Tree
- K Nearest Neighbors (KNN)
- Random Forest
- Gradient Boosting

Each experiment included baseline training, five-fold stratified cross-validation, hyperparameter tuning with `GridSearchCV`, validation comparison, final refitting and testing on the untouched test partition.

## Final Test Results

| Model | Accuracy | Precision | Recall | F1 Score | ROC AUC | Predicted Both Classes |
|---|---:|---:|---:|---:|---:|:---:|
| **KNN** | 50.70% | 54.78% | 55.31% | 55.04% | **0.5086** | Yes |
| Decision Tree | 54.56% | 54.56% | 100.00% | 70.60% | 0.5069 | No |
| Random Forest | 54.54% | 54.56% | 99.91% | 70.58% | 0.5051 | Yes |
| Gradient Boosting | 54.56% | 54.56% | 100.00% | 70.60% | 0.5042 | No |
| Logistic Regression | 54.56% | 54.56% | 100.00% | 70.60% | 0.4950 | No |
| SVM | 54.56% | 54.56% | 100.00% | 70.60% | 0.4907 | No |

The test-set majority-class baseline was 54.56%. Logistic Regression, SVM, Decision Tree and Gradient Boosting obtained approximately this accuracy by predicting every test observation as Class 1. Random Forest predicted only 13 observations as Class 0.

### Final Model Selection

- **Primary model - KNN:** Selected because it predicted both target classes in substantial quantities and achieved the highest test ROC AUC and balanced accuracy.
- **Secondary model - Decision Tree:** Retained as an interpretable comparison model and achieved the second-highest test ROC AUC.

KNN achieved 50.70% accuracy and a ROC AUC of 0.5086. Therefore, the project does **not** claim reliable next-day stock-direction prediction. The results indicate that the selected five-feature representation contains limited predictive information.

## Repository Structure

```text
2026-Y2-S1-MLB-B1G2-06/
|-- README.md
|-- requirements.txt
|-- data/
|   |-- raw/
|   |   `-- Market.csv
|   |-- processed/
|   |   |-- train_selected.csv
|   |   |-- validation_selected.csv
|   |   `-- test_selected.csv
|   `-- external/
|-- notebooks/
|   |-- preprocessing/
|   |   |-- IT25102978_Missing_Data_Handling.ipynb
|   |   |-- IT25102165_Categorical_Encoding.ipynb
|   |   |-- IT25200808_outlier-hanling.ipynb
|   |   |-- IT25101173_FeatureScaling.ipynb
|   |   |-- IT25200151_Feature engineering .ipynb
|   |   `-- IT25102034 Feature selection.ipynb
|   |-- group pipeline/
|   |   `-- MLB_2026-Y2-S1-MLB-B1G2-06 group_pipeline.ipynb
|   |-- models/
|   |   |-- IT25102978_Logistic_Regression.ipynb
|   |   |-- IT25102165_SVM.ipynb
|   |   |-- IT25200808_Decision_Tree.ipynb
|   |   |-- IT25101173_KNN.ipynb
|   |   |-- IT25200151_Random_Forest.ipynb
|   |   `-- IT25102034_Gradient_Boosting.ipynb
|   `-- final comparison/
|       `-- Final Model Comparison.ipynb
|-- results/
|   |-- preprocessing/
|   |   |-- eda visualizations/
|   |   |-- logs/
|   |   `-- outputs/
|   |-- individual models/
|   |   |-- IT25102978_Logistic_Regression_Results/
|   |   |-- IT25102165_SVM_Results/
|   |   |-- IT25200808_Decision_Tree_Results/
|   |   |-- IT25101173_KNN_Results/
|   |   |-- IT25200151_Random_Forest_Results/
|   |   `-- IT25102034_Gradient_Boosting_Results/
|   `-- final comparison/
|       |-- tables/
|       |-- summaries/
|       `-- plots/
|-- documentation/
|   |-- 2026-Y2-S1-MLB-B1G2-06.pdf
|   `-- CONTRIBUTIONS.md
`-- scripts/
```

## How to Run the Project

The notebooks can be executed in Google Colab or in a local Jupyter environment.

### 1. Install the Dependencies

```bash
pip install -r requirements.txt
```

### 2. Run the Group Preprocessing Pipeline

Open:

```text
notebooks/group pipeline/MLB_2026-Y2-S1-MLB-B1G2-06 group_pipeline.ipynb
```

Run all cells from top to bottom. The notebook automatically reads `data/raw/Market.csv` or `Market.csv`. In Colab, it requests the file if it cannot locate it.

The pipeline generates the selected datasets required for model training:

```text
train_selected.csv
validation_selected.csv
test_selected.csv
```

It also creates `AIML_Group_Pipeline_Results.zip`, containing the preprocessing outputs, logs and EDA visualizations.

### 3. Run the Six Model Notebooks

Open each notebook under `notebooks/models/` and run all cells. In Colab, upload these three files when requested:

```text
train_selected.csv
validation_selected.csv
test_selected.csv
```

Each model notebook creates a result ZIP:

```text
IT25102978_Logistic_Regression_Results.zip
IT25102165_SVM_Results.zip
IT25200808_Decision_Tree_Results.zip
IT25101173_KNN_Results.zip
IT25200151_Random_Forest_Results.zip
IT25102034_Gradient_Boosting_Results.zip
```

### 4. Compare All Six Models

Open:

```text
notebooks/final comparison/Final Model Comparison.ipynb
```

Run all cells and upload the six model-result ZIP files when requested. The notebook generates:

```text
AIML_Final_Model_Comparison_Results.zip
```

The final comparison includes consolidated metric tables, cross-validation results, prediction samples, confusion matrices, model-comparison plots and the final-selection summary.

## Saved Outputs

| Location | Contents |
|---|---|
| `results/preprocessing/outputs/` | Cleaned, engineered, processed and selected datasets |
| `results/preprocessing/logs/` | Missing-value summaries, split details, IQR limits, scaler parameters and feature scores |
| `results/preprocessing/eda visualizations/` | Seven preprocessing and EDA charts |
| `results/individual models/` | Saved models, predictions, metrics, tuning results, reports and plots for all six classifiers |
| `results/final comparison/tables/` | Final test, cross-validation, design and prediction-sample tables |
| `results/final comparison/summaries/` | Final model-selection and project summaries |
| `results/final comparison/plots/` | Six final comparison figures |

The complete project report is available at [`documentation/2026-Y2-S1-MLB-B1G2-06.pdf`](documentation/2026-Y2-S1-MLB-B1G2-06.pdf).

## Limitations and Future Improvements

Current limitations include the absence of direct lagged-return features, news, economic announcements, interest rates, sentiment and other external factors. Future work could:

- Create lagged returns and previous-day price-change features.
- Add moving averages, momentum indicators and rolling volatility.
- Train separate models for individual market indices.
- Apply class-sensitive learning and validation-based decision thresholds.
- Use walk-forward validation.
- Include approved economic or sentiment data.

## AI Tool Usage

ChatGPT supported explanations, code development, debugging and documentation. Google Colab and Python libraries were used to execute the workflow, create visualizations, train models and calculate evaluation metrics. The project group reviewed and executed the generated code and remains responsible for understanding, validating and presenting the submitted work. No experimental metric was altered or fabricated.

## Conclusion

The project completed dataset understanding, preprocessing, exploratory analysis, six-model implementation, hyperparameter tuning, five-fold cross-validation, final evaluation and model comparison. KNN was selected as the most meaningful observed model because it predicted both classes and achieved the highest ROC AUC and balanced accuracy. However, all models showed weak discrimination, so the honest conclusion is that the available selected features were insufficient for reliable next-day stock-direction prediction.
