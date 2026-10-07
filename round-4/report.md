# round-4 — Reconstruct

**Team:** BB-012
**Queries used:** \_\_7\_ / budget

## What we concluded

The system behaves like a black-box ML model that takes all shipment features as input and produces a clearance score between 0 and 1, followed by an APPROVE or DECLINE decision.

From our experiments, we observed that the output is not controlled by a single feature. Instead, multiple features interact and contribute differently to the final score. We used the collected query results as a dataset and built a surrogate ML model to approximate the behaviour of the hidden system.

Our reconstruction therefore treats score prediction as a regression problem and decision prediction as a classification problem.

## How we got there

We started with baseline queries to understand the normal range of the score and decision.

Next, we changed individual features while keeping the remaining values constant. This helped us observe which features had a stronger effect on the score.

We then tested combinations of high and low values to check whether the system depended only on individual features or on interactions between multiple features.

We also tested intermediate and fractional values because the system accepts continuous values. This helped us observe smoother changes in the output instead of testing only extreme values.

Finally, we collected the query responses into a dataset and trained ML models on the input features, score, and decision. We compared their predictions with the black-box outputs and selected the model that reproduced the behaviour most closely.

## What we ruled out

We ruled out the hypothesis that one single feature determines the output, because changing different features produced different changes in the score.

We also ruled out a simple fixed-rule system such as “high value = APPROVE” or “low value = DECLINE.” Similar values could produce different outputs when other features were changed.

We ruled out relying on the real-world meaning of the feature names. Some results did not follow intuitive cargo-inspection logic, which is consistent with the problem statement stating that the system is synthetic.

We also found that the score is not simply a direct copy of one input field. The behaviour suggests that several features are combined before producing the final output.

## What we are still unsure about

We cannot determine the exact internal algorithm, weights, or mathematical formula used by the original system because we only have black-box access and a limited query budget.

We are also not completely certain about the exact interactions between every pair of features or the precise threshold used to convert the score into APPROVE or DECLINE.

Therefore, our reconstructed model should be considered an approximation of the hidden system rather than an exact copy.
