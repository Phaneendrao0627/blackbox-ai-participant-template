# round-1 — Observe

**Team:** BB-012
**Queries used:** 10 / budget

## What we concluded

The system's output is sensitive to several input features, and the effects are not uniformly monotonic.

The strongest observed result was a score of **0.9763 (APPROVE)**. The most important effects observed were:

* **discrepancy_ratio:** values below 1.00 performed substantially better. The score increased from 0.6534 at 1.00 to 0.9610 at 0.90 and reached 0.9763 at 0.99.
* **recent_seizures:** 4 performed slightly better than 5 in the tested configuration (0.9691 vs 0.9686).
* **shipper_score:** 900 performed better than 600 in the clean comparison (0.8680 vs 0.7495).
* **shipper_years:** the best tested region was around 30 years. Scores improved from 0.9610 at 40 years to 0.9686 at 30, then fell to 0.9500 at 25.
* **route_age_days:** increasing from 18 to 27 improved the score from 0.9691 to the best observed 0.9763.
* **port:** different ports produced substantially different results. Port D gave the best observed result, while Port C produced a much lower score of 0.0430.

Overall, the system appears to respond strongly to some features while other tested features can be inactive
