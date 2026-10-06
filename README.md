# Insect Trap Captures and Weather: Time-Series Analysis and Prediction

Business Intelligence coursework (MSc in Data Science, University of Naples Federico II, 2024–25). The question: **can local weather explain or predict how many insects are caught in field traps?** We used hourly weather records and trap-capture counts from three monitoring sites (*Cicalino 1*, *Cicalino 2* and *Imola 1*, summer 2024). The notebook commentary is in Italian.

![Python](https://img.shields.io/badge/Python-3-3776AB?logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-time_series-150458?logo=pandas&logoColor=white)
![statsmodels](https://img.shields.io/badge/statsmodels-STL%20%7C%20SARIMAX%20%7C%20Ljung--Box-4051B5)
![scikit-learn](https://img.shields.io/badge/scikit--learn-KMeans%20%7C%20RF-F7931E?logo=scikitlearn&logoColor=white)

## Method

1. **Data preparation**: merged each site's historical weather CSV (mean, min and max temperature, mean humidity) with its capture CSV into one hourly table.
2. **Time-series analysis**: seasonal decomposition, autocorrelation, residual analysis with 3σ detection of unusual days, Ljung-Box test for weekly seasonality.
3. **Correlation**: density plots and Pearson correlation of temperature and humidity against captures, plus polynomial (degree 2) regression.
4. **Clustering**: K-Means (k = 3, chosen by the elbow method) on weather features, then one-way ANOVA to test whether captures differ between weather clusters.
5. **Prediction**: Random Forest regression and SARIMAX for counts; Random Forest classification of capture levels; linear regression baseline.

## Key findings

| | Cicalino 1 | Imola 1 |
|---|---|---|
| Weekly seasonality (Ljung-Box, lag 7) | significant, p = 0.003 | not significant, p = 0.09 |
| K-Means silhouette (k = 3) | 0.56 | 0.51 |
| ANOVA: captures differ across weather clusters | no, p = 0.18 | yes, p < 0.001 |
| Random Forest regression R² | −0.26 | 0.05 |
| Random Forest classifier, macro F1 | 0.52 | 0.64 |

What this means:

- Weather alone explains very little of the hour-to-hour variation in captures. R² is near zero or negative.
- The high headline accuracies in the notebook (0.83–0.96) come from class imbalance (for example 263 against 14 test samples at Cicalino 1). Macro F1 and minority-class recall are the honest measures, and they are weak: minority-class recall is 0.07 at Cicalino 1 and 0.30 at Imola 1.
- At Imola 1 the cool, humid weather cluster had more captures than the others (cluster mean 0.77 against 0.42), which is the one usable signal.

Next steps: model daily rather than hourly counts with a count model (Poisson or negative binomial), add lagged captures and degree-day features, and evaluate with a time-based split.

## Files

| File | Content |
|---|---|
| `Homework_flora.ipynb` | Full analysis for all three sites (136 cells) |

The raw CSVs are not included. Paths in the notebook point to `/content/` (Google Colab).

## Author

Faiz Hussain
