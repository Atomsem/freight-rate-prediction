# Freight Rate Prediction Challenge

## Machine Learning Engineering Assessment

A machine learning regression project that predicts freight rates using shipment information, including pickup location, delivery location, distance, equipment type, weight, and date.

## 1. Project Objective

The objective is to develop a machine learning model that estimates the posted freight rate for a shipment.

The project includes three main tasks:

- Explore and prepare historical freight data.
- Train and validate a regression model using the labeled development dataset.
- Generate predictions for 12,000 validation loads and a fixed shipment scenario covering December 2025.

## 2. Project Structure

```text
Freight-Rate-Prediction/
│
├── Data/
│   ├── train-test.csv
│   ├── validation.csv
│   ├── validation-predictions-template.csv
│   └── december-chart-inputs.csv
│
├── reports/
│   └── Freight_Rate_Technical_Report.pdf
│
├── scorer_results/
│   └── candidate_december.png
│
├── ML_Engineering_Assessment.ipynb
├── score.py
├── requirements.txt
├── validation_predictions.csv
├── december_predictions.csv
└── README.md
```

## 3. Dataset Description

### Development Dataset

- File: `Data/train-test.csv`
- Rows: 48,000
- Target variable: `posted_rate`
- Date coverage: January–October 2025

### Validation Dataset

- File: `Data/validation.csv`
- Rows: 12,000
- Contains unique load IDs and shipment features.
- Used to generate final freight rate predictions.

### December Scenario Dataset

- File: `Data/december-chart-inputs.csv`
- Rows: 31
- Covers December 1–31, 2025.
- Fixed shipment scenario:
  - Pickup: Lexington
  - Delivery: Fort Wayne
  - Distance: 360 miles
  - Equipment: Dry Van
  - Weight: 32,000 lb

## 4. Exploratory Data Analysis

The exploratory analysis examined:

- Freight rate distribution.
- Relationship between freight distance and posted rate.
- Equipment category distribution.
- Missing values and numerical feature relationships.
- Monthly freight rate patterns.

One important finding was the strong positive correlation between distance and posted rate (approximately 0.9085).

The average posted rate in the development data was approximately $2,373.98.

## 5. Data Preprocessing and Feature Engineering

The preprocessing workflow included:

- Converting date values into datetime format.
- Extracting month, day, and day-of-week features.
- Handling missing numerical values using median imputation.
- Handling missing categorical values using most-frequent-value imputation.
- Applying one-hot encoding to categorical features.
- Excluding `load_id` from model training.

The final shared-feature model used:

- Pickup
- Delivery
- Distance
- Equipment
- Weight
- Month
- Day
- Day of week

These features were selected because they were available across the development, validation, and December datasets.

## 6. Model Development

The selected algorithm was `HistGradientBoostingRegressor`, a tree-based gradient boosting regression model.

The model configuration included:

| Parameter | Value |
|---|---:|
| Maximum iterations | 200 |
| Learning rate | 0.07 |
| Maximum leaf nodes | 31 |
| L2 regularization | 2 |
| Random state | 42 |

A logarithmic transformation using `log1p` was applied to the target during model training. Predictions were converted back to the original rate scale using `expm1`.

### Validation Strategy

A chronological split was used during model experimentation:

- Training period: January–September 2025
- Internal testing period: October 2025

This approach evaluates the model on later observations without randomly mixing future data into the training period.

An earlier reduced-feature log-target experiment achieved:

- MAE: approximately $112.65
- RMSE: approximately $648.61
- R²: approximately 0.8199

These metrics belong to the October internal experiment, not the final shared-feature model.

The final shared-feature model was subsequently trained on all 48,000 development records.

## 7. Final Prediction Files

### Validation Predictions

File: `validation_predictions.csv`

Contains exactly two columns:

```text
load_id,predicted_rate
```

The file contains 12,000 predictions corresponding to the validation dataset.

### December Predictions

File: `december_predictions.csv`

Contains seven columns:

```text
pickup,delivery,distance,equipment,weight,date,predicted_rate
```

The file contains 31 daily predictions for the fixed shipment scenario.

## 8. Scorer Verification

The provided `score.py` script was used to validate the prediction files and generate the December chart.

The command used was:

```bash
python score.py --predictions validation_predictions.csv --december-predictions december_predictions.csv
```

Successful execution confirmed:

- 12,000 validation predictions passed.
- 31 December predictions passed.
- The December prediction chart was generated.

The chart is saved at:

```text
scorer_results/candidate_december.png
```

Final validation performance metrics are calculated by Spotter after submission.

## 9. Installation and Execution

### Requirements

Install the required Python packages:

```bash
python -m pip install -r requirements.txt
```

The project uses:

- Python
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- Jupyter Notebook

### Run the Project

1. Clone or download the repository.
2. Install the required dependencies.
3. Open `ML_Engineering_Assessment.ipynb` in Jupyter Notebook or VS Code.
4. Run the notebook cells in order to explore the data, train the model, and generate predictions.
5. Run the scorer:

```bash
python score.py --predictions validation_predictions.csv --december-predictions december_predictions.csv
```

## 10. Submission Deliverables

The project submission includes:

- GitHub repository containing the solution code and dependencies.
- `validation_predictions.csv` containing 12,000 final predictions.
- PDF technical report describing the methodology and validation approach.
- December prediction chart generated by the provided scorer.
- A 2–3 minute Loom video explaining the project and implementation.

## 11. Conclusion

This project developed a freight rate prediction workflow using historical shipment data and gradient boosting regression.

The workflow covers data exploration, preprocessing, feature engineering, model experimentation, chronological validation, and final prediction generation.

Both prediction files successfully passed the provided scorer's structural and scenario validation checks.

The final objective is to provide reliable freight rate estimates and a reproducible machine learning workflow.