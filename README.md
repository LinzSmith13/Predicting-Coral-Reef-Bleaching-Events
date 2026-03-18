# Coral Reef Bleaching: Building a Predictive Model

## Summary
Rising sea surface temperatures are a primary driver of coral reef bleaching, but predicting these events globally is difficult. What counts as "hot" to corals would vary drastically from region to region. This project built a supervised machine learning pipeline to predict coral bleaching events. By pivoting from a struggling global baseline model to a regionally-tuned model, and by engineering a Cumulative Heat Stress feature, the final Random Forest classifier successfully prioritized "early warnings" (Recall) to identify possible bleaching events.

## The Problem with a Simple Global Model
An initial exploratory data analysis revealed that corals do not bleach simply because the water reaches a specific temperature. They bleach due to prolonged thermal stress. Feeding raw Sea Surface Temperature (SST) into a baseline global Random Forest resulted in a poor recall rate (30%), meaning the model missed 70% of bleaching events. The model was confused by the varying biological thresholds of different oceans. 

## Feature Engineering & Model Iteration
To solve this, I implemented an iterative testing approach to model optimization in three different ways:
- Feature Engineering: I engineered a Heat_Stress_12Wk feature. By calculating the 12-week rolling sum of temperature anomalies for each region, similar to NOAA's "Degree Heating Weeks" metric. This proved to be a much better predictor than daily temperatures.
- Hyperparameter Tuning: I used GridSearchCV to constrain tree depth, preventing the model from overfitting.
- Regionalization: I isolated the data to a single area (The Great Barrier Reef). This allowed the algorithm to learn the specific biological rules of one environment rather than searching for an impossible "global" rule.
- Threshold Adjustment: I lowered the decision threshold to 0.40. In a conservation context, a False Positive is far less costly than a False Negative (missing a mass bleaching event entirely).

## Key Findings & Results
The final tuned, regional Random Forest achieved an underwhelming 50% Recall for severe bleaching events. More importantly, extracting the feature importances mathematically validated the feature engineering phase:
- Feature, Importance, Weight
- Heat_Stress_12Wk (Engineered),36.8%
- pH Level (Ocean Acidification),31.7%
- SST (°C) (Raw Temperature),29.4%
- Marine Heatwave (Binary flag),1.9%

The model confirms that prolonged, accumulating heat stress and ocean acidification (pH) are much stronger indicators of ecosystem collapse than simple temperature spikes. 

## Limitations & Next Steps
Because this model was restricted to the Great Barrier Reef to improve accuracy, the test set size was reduced to just 18 samples. While the predictive patterns are mathematically sound, the model requires a larger volume of localized historical data before it could be realistically functional.
