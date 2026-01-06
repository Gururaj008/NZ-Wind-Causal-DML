# Two Wind Farms, Two Islands: Physics-Informed Causal Wind Analysis in New Zealand

**Authors:** Gururaj H C & Dr. Vasudha Hegde  
**Preprint:** [EarthArXiv](https://eartharxiv.org/) (Submitted)

This repository contains the complete replication code for the study **"Two Wind Farms, Two Islands,"** which applies **Double Machine Learning (DML)** to estimate the causal drivers of wind power generation in New Zealand.

---

## **1. The Need**
In raw observational data, **air density** often appears negatively correlated with wind power.
* **Physics:** Higher air density ($\rho$) should increase power output ($P \propto \rho v^3$).
* **Data Paradox:** High-pressure systems (high density) are typically associated with calm, stable weather (low wind speed).
* **Operational Risk:** Standard correlation-based forecasting models capture the weather regime rather than the true turbine physics, leading to potentially misleading signals for grid management.

This project moves beyond correlation to **causation**, identifying the marginal effect of air density to improve reliability in power systems planning.

## **2. Input Data**
The analysis integrates **20 years of hourly data (2005–2024)** from two distinct sources:
* **Meteorological:** ERA5 Reanalysis (ECMWF) for 100m wind vectors, temperature, and surface pressure.
* **Generation:** Grid injection logs from the New Zealand Electricity Authority (EMI).

**Study Sites:**
* **North Island:** Te Apiti Wind Farm (Complex terrain, channelled flow).
* **South Island:** White Hill Wind Farm (Westerly flow, "Roaring Forties").

## **3. Methodology**
We employ a **Double Machine Learning (DML)** framework to isolate the causal effect of air density:
1.  **Causal Graph (DAG):** We assume a structural model where *Seasonality* and *Synoptic State* confound both *Air Density* and *Wind Speed*.
2.  **Nuisance Models:** We use **XGBoost** regressors to learn the conditional expectations of both Power ($Y$) and Density ($T$) based on the confounders ($X$).
3.  **Cross-Fitting:** We use time-ordered cross-validation to prevent overfitting and ensure residuals are orthogonal.
4.  **Inference:** The final causal effect is estimated via a residual-on-residual regression, with uncertainty quantified using moving-block bootstrapping.

## **4. Key Results**
* **Paradox Resolved:** After causal adjustment, the effect of air density flips from negative to positive, consistent with aerodynamic laws.
* **Magnitude:**
    * **North Island:** 17.2 MW increase per $0.1kg/m^3$ rise in density.
    * **South Island:** 3.9 MW increase per $0.1kg/m^3$ rise in density.
* **Heterogeneity:** The effect is not constant; it peaks during **Summer** and at **medium wind speeds (6–10 m/s)**, providing a critical operational margin during ramp-up events.

## **5. Implications**
* **Grid Stability:** Integrating density as a causal driver—rather than a passive correlate—adds operational value, particularly during "marginal" wind days in summer.
* **Forecasting:** The framework offers a way to correct physical inconsistencies in "black box" AI models, reducing the risk of large errors during rare high-pressure, high-wind events.
* **Scalability:** This physics-informed data science approach can be adapted for other renewable technologies (e.g., solar, hydro) where environmental drivers are highly confounded.

---

## **Repository Structure**
The core analysis is contained in `NZ_wind_casual_DML.ipynb`, structured into six logical phases:

| Phase | Description | Key Outputs |
| :--- | :--- | :--- |
| **01. EDA Suite** | Quality control, distribution analysis, and "Physics Checks" (Empirical Power Curves, Polar Plots). | `Distributions_*.png`, `Polar_Plot_*.png` |
| **02. Time Series** | Stationarity tests (ADF/KPSS), seasonality decomposition (STL), and Lag/Autocorrelation analysis. | `Seasonality_Boxplot.png`, `Lag_Analysis.png` |
| **03. Causal DML** | The main **Double Machine Learning** engine using XGBoost nuisance models and cross-fitting to estimate conditional effects. | `Forest_Plot_Seasonal.png`, `DML_Summary.csv` |
| **04. Robustness** | Validation suite including Placebo tests (random noise), Temporal Stability (2005-14 vs 2015-24), and Learner sensitivity. | `Robustness_Table.csv` |
| **05. Explainability** | **SHAP** (SHapley Additive exPlanations) to interpret the underlying nuisance models and visualize feature interactions. | `SHAP_Summary.png`, `SHAP_Dependence.png` |
| **06. Sensitivity** | Moving-block bootstrapping for time-series uncertainty and operational-zero candidate filtering. | `Phase06_Overlap_Diagnostics.csv` |

## **Installation & Usage**

### **1. Environment**
This project relies on standard data science and causal inference libraries.
```bash
pip install pandas numpy matplotlib seaborn scipy statsmodels scikit-learn xgboost networkx shap tqdm
