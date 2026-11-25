# AML_Assignment

## Set 1 Logistic Regression: 
| | Predicted legitimate | Predicted phishing |
| -------- | -------- | -------- |
| Actually legitimate | 1175 correct     | 56 (false positives)     |
| Actually phishing | 79 (false negatives)     | 901 correct     |


| Metric | Value | Meaning |
| -------- | -------- | -------- |
|Accuracy   | 0.9389     | About 94% of all sites were classified correctly.     |
| Precision    | 0.9415     | When the model says "phishing", it is right 94% of the time. The other 6% are legitimate sites wrongly blocked (inconvenience).     |
| Recall   | 0.9194     | The model catches 92% of the real phishing sites.     |
| FNR   | 0.0806     | 8% of phishing sites slip through as "legitimate". This is the most dangerous error and the one your proposal focuses on.     |
| F1    | 0.9303     | The balance of precision and recall.     |
| ROC-AUC    | 0.9886     | If you pick one phishing and one legitimate site at random, the model ranks the phishing one as more suspicious 98.9% of the time.     |

Note: 
- Precision = of the sites flagged as phishing, how many really were?
- Recall = of the real phishing sites, how many were caught?
- FNR = False Negative Rate
- F1 = How good is the model when I need both precision and recall to be high?
- ROC-AUC = How well does the model rank phishing sites above legitimate ones, regardless of the threshold? (Overall ranking quality)


**What this tells you:**
- **No overfitting or leakage.** The cross-validation recall on the training data (0.9232) is very close to the test recall (0.9194). The model generalises well, and the split and encoding are working properly.
- **C = 10 is a sensible choice.** It is in the middle of your grid (0.01 to 100), so the best value wasn't cut off at an edge. It means the model preferred weak regularization, i.e. it benefits from using the features freely.
- **Errors are not balanced.** There are 79 false negatives against 56 false positives. For phishing detection, a missed phishing site is the costlier error, which is why your project tracks FNR and not just accuracy.
- **AUC is much higher than accuracy.** The model ranks sites well, but a straight linear boundary at the default 0.5 threshold can't separate every case. This leaves room for the non-linear FCNN to improve, and that comparison is the point of your study.


## Set 2 FCNN

| Metric | Run 1 | Run 2 | Run 3 | Mean ± std |
| -------- | -------- | -------- | -------- | -------- |
| Accuracy    | 0.9683     | 0.9715     | 0.9679     | 0.9692 ± 0.0016    |
| Precision    | 0.9730     | 0.9683     | 0.9662     | 0.9692 ± 0.0028     |
| Recall    | 0.9551     | 0.9673     | 0.9612     | 0.9612 ± 0.0050     |
| F1    | 0.9640     | 0.9678     | 0.9637     | 0.9652 ± 0.0019     |
| ROC-AUC    | 0.9960     | 0.9966     | 0.9958     | 0.9961 ± 0.0003    |
| FN    | 44     | 32     | 38     | 38.0 ± 4.9     |
| FP    | 26     | 31     | 33     | 30.0 ± 2.9     |
| FNR    | 4.49%     | 3.27%     | 3.88%     | 3.88% ± 0.50%     |

Note: 
- Neural networks start from random initial weights and use random shuffling and dropout, so no 3 runs give identical results.
- Each run is simply a different draw from the same model. Thus, running a multi-seed loop is needed.
- For each seed, the loop does the following:
  - Sets the random seed (1, 2, 3, 4, 5).
  - Builds a fresh FCNN with new random starting weights.
  - Trains it on the same training data.
  - Evaluates it on the same test set.
  - Records the metrics (FN, recall, F1, etc.).

**Suggestion:**
**Use five seeds for the final numbers.** Three runs is a small sample for a standard deviation, so run the 5-seed loop from before to get a more reliable mean ± std. The six experiments should all use the same seeds (1 to 5).
