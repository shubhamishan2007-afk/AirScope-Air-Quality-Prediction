# AirScope: Air Quality Prediction and Pollution Analysis

**A Statistical Machine Learning project for estimating Delhi's PM2.5 concentration from historical air-quality, weather, location and time features.**

## Project overview

AirScope explores historical air-quality observations from Delhi and builds regression models to estimate PM2.5 concentration, measured in micrograms per cubic metre (µg/m³).

The project includes data inspection, exploratory data analysis (EDA), feature preparation, regression modelling, model evaluation and a notebook-based prediction demonstration.

> **Scope note:** This is a historical PM2.5 prediction project. It does not provide live monitoring data and it is not an official AQI measurement or forecasting service.

## Repository contents

| File | Description |
|---|---|
| `SML_Project.ipynb` | Main Jupyter notebook containing data analysis, model training, evaluation and prediction demonstration. |
| `requirements.txt` | Python packages used by the notebook. |
| `README.md` | Project overview, dataset source and instructions. |

The processed Excel dataset is not stored in this GitHub repository because of its file size. It can be obtained from the source linked below.

## Dataset

**Source:** [Delhi, Mumbai, Kolkata and Guwahati PM2.5 Dataset on Zenodo](https://zenodo.org/records/20789755)  
**DOI:** [10.5281/zenodo.20789755](https://doi.org/10.5281/zenodo.20789755)

The Zenodo record describes the Delhi dataset as containing:

- 233,600 observations
- 40 Delhi monitoring stations
- Coverage from 2021 to 2022
- PM2.5 measurements and co-located meteorological variables
- Temperature, relative humidity, wind speed and wind direction, including sine/cosine wind-direction features
- Month and hour-of-day information

The record states that the measurements originate from the Central Pollution Control Board (CPCB), India.

### Download and prepare the dataset

1. Open the [Zenodo dataset record](https://zenodo.org/records/20789755).
2. Download the dataset archive (`Bishwadip_Zenodo_Data.zip`).
3. Extract the archive.
4. Locate the processed Delhi workbook used for this project.
5. Place that workbook in the same folder as `SML_Project.ipynb`.
6. Rename the workbook to exactly:

   `delhi_processed - delhi_processed.xlsx`

The notebook currently reads the dataset using:

```python
df = pd.read_excel("delhi_processed - delhi_processed.xlsx")
```

If you use a different filename or file location, update the path in the first code cell accordingly.

## Methodology

### 1. Data inspection and exploratory analysis

The notebook examines the dataset structure, duplicate observations, monitoring stations, date range and descriptive statistics. It also visualises PM2.5 distributions and trends, station-level averages, relationships with weather variables and correlations.

### 2. Input features and prediction target

The notebook creates `Month` and `Hour` from the `From Date` field.

The model input features are:

- `AT` — air temperature
- `RH` — relative humidity
- `WD` — wind direction
- `WS` — wind speed
- `Sin_WD` and `Cos_WD` — wind-direction encodings
- `Latitude` and `Longitude` — monitoring station coordinates
- `Month` and `Hour` — time features

**Target variable:** `PM2.5`

### 3. Regression models

The notebook implements and evaluates:

- Linear Regression
- Random Forest Regressor
- Gradient Boosting Regressor
- Tuned Gradient Boosting Regressor
- Histogram Gradient Boosting Regressor

The principal Random Forest configuration in the random holdout experiment uses 100 trees (`n_estimators=100`), maximum depth 15 (`max_depth=15`) and `random_state=42`.

### 4. Evaluation metrics

The project uses:

- **MAE (Mean Absolute Error):** average absolute difference between actual and predicted PM2.5 values.
- **RMSE (Root Mean Squared Error):** error metric that gives greater weight to larger prediction errors.
- **R² score:** indicates how much of the variation in the target is explained by the model relative to a mean-prediction baseline.

Because PM2.5 is a continuous numerical target, this is a **regression task**. Accuracy percentage is not the appropriate primary metric.

## Model results recorded in the notebook

The notebook contains two different Random Forest evaluation setups. They must be interpreted separately.

### Random 80/20 train-test split

The random split uses `train_test_split` with `test_size=0.2` and `random_state=42`.

| Metric | Random Forest result |
|---|---:|
| MAE | 32.98 µg/m³ |
| RMSE | 52.51 µg/m³ |
| R² | 0.6586 |

### Chronological 80/20 split

The chronological experiment sorts observations by date, trains on the earlier 80% and tests on the later 20%. This experiment uses a Random Forest with 100 trees, maximum depth 15 and `min_samples_leaf=5`.

| Metric | Random Forest result |
|---|---:|
| MAE | 45.63 µg/m³ |
| RMSE | 69.39 µg/m³ |
| R² | 0.4727 |

The chronological result is a stricter test of performance on later observations. Its lower R² and higher errors indicate that performance is weaker when predicting a later time period than under the random split.

**Evaluation note:** The notebook's final comparison table includes models evaluated with different split strategies. Therefore, those entries should not be treated as a fully like-for-like model ranking. A fair final comparison should train and evaluate every candidate model using the same split strategy.

## How to run the project

### Option A: Google Colab

1. Download `SML_Project.ipynb` from this repository.
2. Open [Google Colab](https://colab.research.google.com/).
3. Upload and open the notebook.
4. Download and extract the Delhi dataset from the Zenodo source above.
5. Upload the processed Delhi workbook to the Colab session.
6. Make sure its filename matches the name expected in the first notebook cell, or edit that cell.
7. Run the notebook from top to bottom.

### Option B: Run locally with Jupyter

1. Clone or download this repository.
2. Download and extract the processed Delhi dataset from Zenodo.
3. Place the workbook beside the notebook using the expected filename.
4. Install the dependencies:

   ```bash
   pip install -r requirements.txt
   ```

5. Start Jupyter:

   ```bash
   jupyter notebook
   ```

6. Open `SML_Project.ipynb` and run the cells in order.

## Limitations

- The project uses historical observations from 2021–2022, not live sensor feeds.
- Prediction errors remain substantial, particularly for high or extreme PM2.5 values.
- Random train-test splitting can produce an optimistic estimate when observations close in time or from the same stations appear in both sets.
- The chronological evaluation gives a more cautious indication of performance on later observations.
- The model uses a limited set of meteorological, location and time features. Other emission and atmospheric factors may affect PM2.5.
- Model performance on historical data does not guarantee performance on future data.
- The model estimates PM2.5 concentration and does not calculate or certify an official AQI.

## Future scope

- Evaluate all candidate models using consistent chronological validation.
- Test on more recent observations not used during model development.
- Explore additional pollutant, weather and temporal features.
- Investigate methods to improve predictions during severe pollution episodes.
- Improve the prediction interface and document deployment requirements.

## Acknowledgement and data attribution

The project uses the Delhi PM2.5 dataset published on Zenodo. The Zenodo record attributes the underlying measurements to CPCB, India. Please consult the dataset record for its authorship, version, licence and citation details before redistributing the data.

**Dataset record:** https://zenodo.org/records/20789755  
**DOI:** https://doi.org/10.5281/zenodo.20789755
