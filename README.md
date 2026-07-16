# Germany Electricity Load Forecasting

## Purpose

Build and compare forecasts for German electricity demand using benchmark rules, statistical time-series models, Random Forest and an hourly LSTM.

## Main outcome

Random Forest provides the strongest weekly result in this version. Its RMSE is 2,475.07 MW, MAE is 1,789.35 MW and MAPE is 3.44%. Compared with the seasonal-naive RMSE of 3,006.76 MW, this is an improvement of about 17.7%.

The result makes Random Forest a practical weekly choice: it is more accurate than the tested statistical models, easier to maintain than the LSTM and can still be discussed through feature importance.

## Performance notes

Seasonal naive remains a valuable fallback with 3,006.76 MW RMSE and 4.41% MAPE.

The final SARIMA improves on that benchmark with 2,788.72 MW RMSE and 4.09% MAPE. The lowest-AIC SARIMA, however, records 9,470.26 MW RMSE, showing why the final decision cannot rely on training fit alone.

Temperature-only SARIMAX reaches 2,847.08 MW RMSE. When holidays are included, RMSE falls to 2,670.12 MW and MAPE falls to 3.75%. Holidays therefore add useful signal, although future observed temperature would not be available during a real forecast.

The LSTM reports an hourly RMSE of 1,166.40 MW, MAE of 893.31 MW and MAPE of 1.66%. Because it uses hourly observations, those values should not be ranked directly against weekly errors. It demonstrates strong hourly skill, not automatic superiority over the weekly Random Forest.

## Data handling

The source covers German load from 2015 to 2020 and contains more than 50,000 hourly records. Timestamps are converted into a time index, the German load variable is selected and the main statistical and machine-learning analysis uses weekly aggregation.

The last 104 weeks form the test period. Lagged demand variables are shifted so the target week cannot predict itself. Same-week observed temperature makes weather-based results conditional; an operational pipeline must replace it with an actual forecast.

## Suggested use

Deploy Random Forest for weekly point forecasts, retain seasonal naive for monitoring, and add bootstrap, quantile or conformal uncertainty before production. Use SARIMA or SARIMAX when interpretability and statistical intervals are more important. Use the LSTM only for a genuinely hourly service with enough computing and monitoring support.

## Local setup

The Git repository includes the Word report and the Python notebook. No separate document download is required.

bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install jupyterlab pandas numpy scipy matplotlib seaborn statsmodels scikit-learn tensorflow openpyxl
jupyter lab


On Windows PowerShell, activate with .venv\Scripts\Activate.ps1. Open the notebook available in JupyterLab, verify the local data location and run all cells. The Word file contains the extended interpretation and the notebook contains the reproducible calculations.
