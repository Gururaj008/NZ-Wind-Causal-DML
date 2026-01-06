# A Tale of Two Islands — Causal Wind Analysis (NZ)

**Paper:** *A Tale of Two Islands: A Physics-Informed Data Science Framework for Causal Wind Analysis in New Zealand*  
This repository contains the Python code and reproducible artifacts for estimating the **causal** effect of air density on New Zealand wind generation using Double Machine Learning (DML), comparing North Island (Te Apiti) vs South Island (White Hill). 

## **Key idea**
Raw correlations can show air density negatively associated with wind power because synoptic regimes confound wind speed and density; DML estimates the density effect after adjusting for wind dynamics + seasonal/diurnal structure. 

## **What’s inside**
- `NZ_wind_casual_DML.ipynb`: End-to-end pipeline (EDA → causal DAG → DML → robustness + plots/tables). 
- Outputs are written to:
  - `Results/Plots/` (figures such as correlation matrices, polar plots, power curves, forest plots). 
  - `Results/Tables/` (CSV tables such as stationarity tests and DML summaries). 

## **Data requirements**[file:241]
- `ENHANCEDDATASETNORTHISLAND.csv` (North Island: 2005–2024). 
- `ENHANCEDDATASETSOUTHISLAND.csv` (South Island: 2009–2024). 

Expected key columns used in DML:
- Outcome `powermw` 
- Treatment `airdensitykgm3` 
- Controls include `windspeed100m`, `windcubed`, `turbulenceproxy`, `ramprate`, `windlag1`, plus harmonic time features (`hoursin/hourcos`, `monthsin/monthcos`) and wind direction harmonics (`windsin/windcos`).

## **Quick start**
1. Create environment and install dependencies (typical):
   - `pandas, numpy, matplotlib, seaborn, scipy, statsmodels, scikit-learn, xgboost, networkx, tqdm` 
2. Place input CSVs under:
   - `Data/Finaldataset/ENHANCEDDATASETNORTHISLAND.csv`
   - `DataF/inaldataset/ENHANCEDDATASETSOUTHISLAND.csv` 
3. Run:
 `NZ_wind_casual_DML.ipynb`
