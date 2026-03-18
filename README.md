# Predicting Coral Reef Bleaching: A Machine Learning Approach to Marine Conservation

## Summary
Rising sea surface temperatures are a primary driver of coral reef bleaching, but predicting these events globally is notoriously difficult. A sudden spike to 29°C might be a normal summer day in the Caribbean but a catastrophic anomaly in the Great Barrier Reef.This project built a supervised machine learning pipeline to predict coral bleaching events. By pivoting from a struggling global baseline model to a regionally-tuned model, and by engineering a custom Cumulative Heat Stress feature, the final Random Forest classifier successfully prioritized "early warnings" (Recall) to identify impending bleaching events.

## The Problem: Why Raw Data Fails
An initial Exploratory Data Analysis (EDA) revealed a critical insight: corals do not bleach simply because the water reaches a specific temperature. They bleach due to prolonged thermal stress.Feeding raw Sea Surface Temperature (SST) into a baseline global Random Forest resulted in a poor recall rate (30%), meaning the model missed 70% of bleaching events. The model was fundamentally confused by the varying biological thresholds of different oceans.

## The Solution: Feature Engineering & Model Iteration
To solve this, I implemented an iterative, A/B/C testing approach to model optimization:
- Feature Engineering (Domain Knowledge): I engineered a Heat_Stress_12Wk feature. By calculating the 12-week rolling sum of temperature anomalies for each region, I created a proxy for NOAA's "Degree Heating Weeks" metric. Visually and mathematically, this proved to be a far superior predictor than daily temperatures.
- Hyperparameter Tuning: Utilized GridSearchCV to constrain tree depth, preventing the model from overfitting to noisy oceanic data.
- Regionalization: Isolated the data to a single ecosystem (The Great Barrier Reef). This allowed the algorithm to learn the specific biological rules of one environment rather than searching for an impossible "global" rule.
- Threshold Adjustment: Lowered the decision threshold to 0.40. In a conservation context, a False Positive (deploying a team to check a healthy reef) is far less costly than a False Negative (missing a mass bleaching event entirely).

## Key Findings & Results
The final tuned, regional Random Forest achieved a 50% Recall for severe bleaching events. More importantly, extracting the feature importances mathematically validated the feature engineering phase:
- Feature,Importan, ce Weight
- Heat_Stress_12Wk (Engineered),36.8%
- pH Level (Ocean Acidification),31.7%
- SST (°C) (Raw Temperature),29.4%
- Marine Heatwave (Binary flag),1.9%

The model confirms that prolonged, accumulating heat stress and ocean acidification (pH) are much stronger indicators of ecosystem collapse than simple temperature spikes. 

## Limitations & Next Steps
Because this model was restricted to the Great Barrier Reef to improve accuracy, the test set size was reduced to just 18 samples. While the predictive patterns are mathematically sound, the model requires a larger volume of localized historical data before it could be deployed as a live early-warning system in production.
