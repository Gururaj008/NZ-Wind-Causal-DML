# Two Wind Farms, Two Islands: Physics-Informed Causal Wind Analysis in New Zealand

**Authors:** Gururaj H C & Dr. Vasudha Hegde  
**Preprint:** [EarthArXiv](https://eartharxiv.org/) (Submitted)

This repository contains the complete replication code for the study **"Two Wind Farms, Two Islands,"** which applies **Double Machine Learning (DML)** to estimate the causal drivers of wind power generation in New Zealand.

By integrating 20 years of hourly ERA5 reanalysis data with grid generation logs (2005–2024), this framework disentangles the physical effect of air density from meteorological confounders, resolving the "negative correlation paradox" often seen in raw observational data.

---

## **The Problem: Correlation vs. Causation**
In raw observational data, **air density** often appears negatively correlated with wind power.
* **Physics:** Higher air density ($\rho$) should increase power output ($P \propto \rho v^3$).
* **Data Paradox:** High-pressure systems (high density) are often associated with calm, stable weather (low wind speed).
* **Result:** Standard correlation metrics capture the weather regime, not the turbine physics.

This project uses a structural causal model (DAG) and **Double Machine Learning** to isolate the marginal effect of density by adjusting for wind dynamics, seasonality, and synoptic state.

---

## **Key Findings**
The analysis compares two distinct wind regimes: **Te Apiti (North Island)** and **White Hill (South Island)**.

1.  **Causal Effect:** After adjustment, the effect of air density flips to positive (consistent with physics).
    * **North Island:** ~17.2 MW increase per $0.1~kg/m^3$ rise in density.
    * **South Island:** ~3.9 MW increase per $0.1~kg/m^3$ rise in density.
2.  **Heterogeneity:** The density effect is not constant. It peaks during **Summer** and at **medium wind speeds (6–10 m/s)**, providing a crucial margin during ramp-up events.
3.  **Robustness:** Results hold up against placebo tests (random noise treatment), temporal stability checks (early vs. late era), and alternative learner specifications.

---

## **Repository Structure**
The core analysis is contained in `NZ_wind_casual_DML.ipynb`, which is structured into six logical phases:

| Phase | Description | Key Outputs |
| :--- | :--- | :--- |
| **01. EDA Suite** | Quality control, distribution analysis, and "Physics Checks" (Empirical Power Curves, Polar Plots). | `Distributions_*.png`, `Polar_Plot_*.png` |
| **02. Time Series** | Stationarity tests (ADF/KPSS), seasonality decomposition (STL), and Lag/Autocorrelation analysis. | `Seasonality_Boxplot.png`, `Lag_Analysis.png` |
| **03. Causal DML** | The main **Double Machine Learning** engine using XGBoost nuisance models and cross-fitting to estimate conditional effects. | `Forest_Plot_Seasonal.png`, `DML_Summary.csv` |
| **04. Robustness** | Validation suite including Placebo tests (random noise), Temporal Stability (2005-14 vs 2015-24), and Learner sensitivity. | `Robustness_Table.csv` |
| **05. Explainability** | **SHAP** (SHapley Additive exPlanations) to interpret the underlying nuisance models and visualize feature interactions. | `SHAP_Summary.png`, `SHAP_Dependence.png` |
| **06. Sensitivity** | Moving-block bootstrapping for time-series uncertainty and operational-zero candidate filtering. | `Phase06_Overlap_Diagnostics.csv` |

---

## **Data Requirements**
The code requires two primary CSV files placed in `Data/Final_dataset/`:

1.  `ENHANCED_DATASET_NORTH_ISLAND.csv` (2005–2024)
2.  `ENHANCED_DATASET_SOUTH_ISLAND.csv` (2009–2024)

**Key Variables:**
* **Outcome:** `power_mw` (Hourly mean generation)
* **Treatment:** `air_density_kgm3` (Derived from ERA5 Temp/Pressure)
* **Controls:** `wind_speed_100m`, `wind_cubed`, `turbulence_proxy`, `ramp_rate`, `wind_lag_1`
* **Time Features:** Harmonic encoding (`hour_sin/cos`, `month_sin/cos`)

---

## **Installation & Usage**

### **1. Environment**
This project relies on standard data science and causal inference libraries.
```bash
pip install pandas numpy matplotlib seaborn scipy statsmodels scikit-learn xgboost networkx shap tqdm
