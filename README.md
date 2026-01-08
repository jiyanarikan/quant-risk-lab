# quant-risk-lab

Forensic analysis of public sector procurement using linear algebra and gradient boosting.

Most audit models are boring because they only look at what already failed. This project tries to predict the failure before it happens. I took 26,000+ government contracts, stripped them down with PCA, and trained a weighted XGBoost model to spot structural risk indicators.



The Findings (The Interesting Stuff):

1. Risk is a Switch, Not a Spectrum - I tried to calibrate the probabilities to get a nice smooth curve. The model refused. It polarized everything to 0% or 99%.

The Reality: There is no "medium" risk. A buyer is either structurally sound or they are compromised. The "Binary Switch" theory holds up.

---------------------------------------------------------------------------------

2. Signal Dominance > More Data - I stressed-tested the model by throwing in the chaotic 2021 (COVID) dataset.

Expectation: More data = smarter model.

Reality: It made zero difference. The risk signal from 2022-23 was so dominant that the model effectively ignored the noise of the pandemic year. A bad buyer in 2021 looks just like a bad buyer in 2024.
---------------------------------------------------------------------------------

3. Sensitivity Testing - I ran two configurations to see if the model was overfitting to the "chaos" of the pandemic years:

The "Noise" Run (2021-2023): Caught 86% of failures but flagged 17,000 contracts. Basically useless because it flagged everything.

The "Stable" Run (Post-2022): Surgical. Catches 60% of failures with half the workload. This is the champion model in the repo.




The Method

Strict Temporal Split (Anti-Leakage): Risk scores are derived only from historical data (2022-23) and mapped to the future (2024). The model predicts 2024 failures without ever seeing 2024 outcomes.

PCA / Variance Analysis: Used Principal Component Analysis to visualise variance and prove feature redundancy, preventing the model from overfitting to correlated noise.

Weighted Loss Function: With a ~2% failure rate, standard models just guess Success. I implemented a calculated scale_pos_weight (34.4) to penalise missed failures 34x harder than false alarms.

---------------------------------------------------------------------------------

Results

Recall: ~60% (The amount of actual risk we catch).

Precision: ~3% (The trade-off we accept to not miss the big ones).

Workload Reduction: Filters out ~16,000 safe contracts so auditors can focus on the ones that look sketchy.
