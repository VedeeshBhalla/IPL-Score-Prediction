# IPL First-Innings Score Prediction

Predict a first-innings final total from the batting and bowling teams, current
runs, wickets, overs, and runs and wickets in the previous five overs.

## Run the notebook

The tested environment uses Python 3.14.2. From this folder, run:

```powershell
python -m pip install -r requirements.txt
python -m jupyterlab "First Innings Score Prediction - IPL.ipynb"
```

Select the Python environment containing these dependencies, then choose
**Restart Kernel and Run All Cells**. The dataset is included in `dataset/ipl.csv`;
run the notebook with this repository as the working directory.

## What the notebook does

- Reads 76,014 ball-by-ball records from 2008-2017.
- Keeps eight historical teams and match states after at least five overs.
- Encodes teams and trains on 2008-2016, with 2017 held out for comparison.
- Evaluates Linear Regression, Decision Tree, Random Forest, and AdaBoost using
  MAE, MSE, and RMSE. MAE and RMSE are measured in runs.
- Demonstrates predictions using Linear Regression and manually entered match states.

Use the freshly executed notebook outputs for presentation results. Tree and
boosting models use a fixed random seed for reproducibility.

## Entering a prediction

After running the training cells, call:

```python
predict_score(
    batting_team="Chennai Super Kings",
    bowling_team="Mumbai Indians",
    overs=9.2,
    runs=79,
    wickets=2,
    runs_in_prev_5=60,
    wickets_in_prev_5=1,
)
```

Use names listed in `consistent_teams`, including historical names such as
`Delhi Daredevils` and `Kings XI Punjab`. Overs use cricket notation: `9.2` means
nine overs and two balls; `9.6` is invalid. Runs and wickets must be nonnegative
integers. The function rejects unsupported teams and inconsistent counts. It
returns the current score for a completed innings and never predicts a final
total below the current score.

## Evaluation limits

The dataset ends in 2017 and supports only eight historical teams. The notebook
retains the original numeric overs representation for compatibility with its
training features. The 2018-2019 examples are manually entered demonstrations,
not an evaluation dataset. Predictions are point estimates; no calibrated
prediction interval is fitted.

The 2017 holdout contains 30 retained matches and 2,778 ball-by-ball records.
Multiple records come from each match, and later innings states are easier to
predict. Report the errors as averages over these records, not as independent
match-level results or classification accuracy. Since the holdout is used to
compare models, an additional season is needed for an independent evaluation
after model selection.

## Source and credits

This standalone project is adapted from the
[IPL Score Prediction project](https://github.com/shsarv/Machine-Learning-Projects/tree/028b322/IPL%20Score%20Prediction)
in [shsarv/Machine-Learning-Projects](https://github.com/shsarv/Machine-Learning-Projects).
The original code attribution is retained in `LICENSE`.
The bundled dataset is reproduced from that project's `dataset/ipl.csv`.

Updates in this version include modern pandas/scikit-learn compatibility,
reproducible model training, input validation, feature-aligned prediction,
and point estimates in place of manually chosen prediction ranges.
Credit the original source and distinguish your own contribution when presenting.
