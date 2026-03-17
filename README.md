# Wine Quality Analysis

Predicting wine quality ratings from chemical properties using regression modeling and feature selection in R.

## Overview

This project analyzes a dataset of 1,143 red wine observations to identify the physicochemical 
properties that most strongly influence expert quality ratings (scored 0–10). The goal was to 
build a reliable predictive model while understanding which chemical factors actually drive 
perceived wine quality.

## Problem

Wine producers need to understand which chemical properties lead to higher quality ratings — 
but with 11 potential predictors and complex interactions between them, it's not obvious which 
variables matter most or how they relate to quality.

## Approach

**1. Variable Selection**
- Applied the Boruta algorithm to identify statistically significant predictors from 11 
  chemical features
- All 11 quantitative variables were confirmed as relevant (alcohol, sulphates, pH, 
  chlorides, volatile acidity, and others)

**2. Exploratory Analysis**
- Generated histograms, density plots, and scatterplots for each variable
- Overlaid fitted normal distributions to assess skewness and distributional shape
- Built a correlation matrix to detect multicollinearity between predictors
- Ran Box-Cox tests — most variables showed skewness suggesting log or power transformations

**3. Regression Modeling**
- Fit an initial OLS model using key predictors: alcohol, sulphates, pH, chlorides, 
  residual sugar
- Tested for multicollinearity using VIF (all values < 2 — no issue detected)
- Ran RESET test — identified potential non-linearity, prompting model enhancements
- Added polynomial term for alcohol (I(alcohol²)) and an interaction term 
  (alcohol × residual.sugar) to capture diminishing returns and conditional effects

**4. Model Diagnostics**
- Residual vs. Fitted plots and Q-Q plots to assess model fit
- Jarque-Bera test confirmed mild non-normality in residuals
- Cook's Distance analysis identified ~74 influential observations for review
- Bootstrapped coefficient estimates (n=1,000) to validate stability

## Key Findings

- **Alcohol** is the strongest positive predictor of quality, though with diminishing returns 
  at very high levels (captured by the quadratic term)
- **Sulphates** positively influence quality, likely due to their role in preservation and 
  flavor
- **Chlorides** and **pH** both have significant negative effects on quality ratings
- **Residual sugar** alone was not significant, but interacts with alcohol content

## Model Performance

| Metric | Value |
|--------|-------|
| Adjusted R² | 0.3145 |
| Residual Std. Error | 0.667 |
| Model F-statistic | 75.85 (p < 2.2e-16) |

The model explains ~31% of variance in quality ratings — a reasonable result given that 
sensory quality scores are inherently subjective and influenced by factors beyond chemistry.

## Tools & Methods

- **Language:** R
- **Libraries:** ggplot2, dplyr, Boruta, car, lmtest, boot, tseries, corrplot
- **Methods:** OLS Regression, Boruta Feature Selection, Box-Cox Transformation Testing, 
  VIF, RESET Test, Cook's Distance, Bootstrapping, Polynomial & Interaction Terms

## Files

| File | Description |
|------|-------------|
| `Wine_Quality_Analysis.pdf` | Full project report with all code, plots, and interpretation |
| `WineQT.csv` | Source dataset (1,143 observations, 11 chemical features + quality score) |
