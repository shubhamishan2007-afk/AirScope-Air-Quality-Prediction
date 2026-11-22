# AirScope: Air Quality Prediction and Pollution Analysis

A Statistical Machine Learning project that explores historical Delhi air-quality observations and estimates PM2.5 concentration using regression models.

## Project contents

- `SML_Project.ipynb` — data inspection, exploratory analysis, feature preparation, model training, evaluation and prediction demonstration.
- `delhi_processed - delhi_processed.xlsx` — processed Delhi air-quality dataset used by the notebook.

## Dataset

The workbook contains 233,600 observations from 40 Delhi monitoring stations, covering 1 January 2021 to 31 December 2022. The prediction target is `PM2.5`, measured in µg/m³.

## Run the notebook

1. Download or clone this repository.
2. Install the dependencies listed below.
3. Open `SML_Project.ipynb` in Jupyter Notebook, JupyterLab or Google Colab.
4. Keep the dataset workbook in the same directory as the notebook.
5. Run the notebook cells from top to bottom.

The notebook loads the workbook with:

```python
df = pd.read_excel("delhi_processed - delhi_processed.xlsx")
```

## Main modelling approach

The notebook compares Linear Regression, Random Forest Regressor, Gradient Boosting Regressor, tuned Gradient Boosting and Histogram Gradient Boosting.

The principal features used by the Random Forest model are temperature (`AT`), relative humidity (`RH`), wind direction (`WD`), wind speed (`WS`), wind-direction sine/cosine transformations, latitude, longitude, month and hour.

## Reported evaluation

The notebook contains a random 80/20 holdout evaluation for Random Forest:

- MAE: 32.98 µg/m³
- RMSE: 52.51 µg/m³
- R²: 0.6586

It also contains a separate chronological 80/20 Random Forest evaluation:

- MAE: 45.63 µg/m³
- RMSE: 69.39 µg/m³
- R²: 0.4727

These results come from different split strategies and should not be presented as if they were the same experiment. The chronological result is a stricter test of performance on later observations.

## Limitations

- The dataset is historical (2021–2022) and does not represent live air-quality readings.
- Prediction errors remain substantial, especially at higher PM2.5 concentrations.
- Historical performance does not guarantee performance on future data.
- The project estimates PM2.5 concentration; it is not an official AQI calculator.

## Dependencies

See `requirements.txt`.
