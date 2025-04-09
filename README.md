# Raptor Migration Weather Impact Analysis 

Quantifying the effects of local weather conditions on raptor migration patterns using GLMMs, based on raptor observation data collected at Allegheny Front, USA in 2018 and 2019.

## 📁 Dataset
- **Source**: Allegheny Front hawk watch observations (Spring 2018 - Fall 2019)
- **Variables**:
  - Response: Daily counts of 4 raptor groups (Eagles, Hawks, Falcons, Buzzards)
  - Predictors: Wind speed/direction, temperature, precipitation, humidity, barometric pressure, etc.
  - Metadata: Observer IDs, counter IDs, observation duration
- **Key challenges**: 
  - Missing values in predictors (e.g., 3.41% NAs in humidity)
  - Observer skill variability
  - Zero-inflated count data

## ⚙️ Methodology
### Statistical Modeling
```r
# Core GLMM implementation example
library(mgcv)
nbgam01.3 <- gam(TOTAL ~ log(Duration) + period + Wind.Spd + Humidity +
                 Temp + BARO + Precipitation + Visibility +
                 s(Counter, bs = "re") + s(skilled.observer, bs = "re"),
                 data = tidy_HawkWatch5,
                 family = nb,
                 method = "REML")
```

## Key Techniques
- **Data preprocessing**:
  - MICE imputation for humidity missing values
  - Anomaly detection (e.g., humidity >100%)
  - Wind direction reclassification (N/S/Other)
- **Model selection**: AIC-based backward elimination
- **Species-specific analysis**: Separate models for eagles, hawks, falcons, and buzzards

