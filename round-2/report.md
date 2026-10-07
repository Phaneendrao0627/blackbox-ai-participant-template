# round-2 — Investigate

**Team:** BB-012
**Queries used:** 149

## What we concluded

The system appears to have a **stable high-scoring region** rather than a simple linear rule. The strongest observed configuration was around:

* `container_count = 100`
* `declared_value = 4–5`
* `discrepancy_ratio ≈ 0.9–0.99`
* `port = D`
* `recent_seizures = 4`
* `route_age_days ≈ 24–27`, with 27 repeatedly performing strongly
* `shipper_score ≈ 890–900`
* `shipper_years ≈ 30–31`
* `transfers` had little measurable effect in the tested high-scoring region

The best observed score in Round 2 was **0.9774 (APPROVE)**.

The system is particularly sensitive to some features. `port` can cause a catastrophic change: Port D produced scores around 0.9765–0.9774, while Port C produced **0.0430 (DECLINE)** under an otherwise strong configuration. `discrepancy_ratio` also has a strong effect when moved far from the high-scoring region: values of 0.5 and 0 produced scores of 0.9328 and 0.8134 respectively.

Overall, the evidence suggests a **high-performing operating region with feature-specific thresholds and interactions**, rather than every input contributing equally.

## How we got there

### 1. Established the high-scoring region

Repeated queries around:

* `discrepancy_ratio = 0.9`
* `port = D`
* `declared_value = 4`
* `recent_seizures = 4`
* `route_age_days = 27`
* `shipper_score = 900`
* `shipper_years = 31`

consistently produced scores around **0.976–0.9774**.

This established a stable baseline for investigating individual features.

### 2. Investigated discrepancy ratio

A wide range was tested.

Observed scores included:

| Discrepancy ratio |         Score |
| ----------------: | ------------: |
|                 0 |        0.8134 |
|               0.5 |        0.9328 |
|               0.6 |        0.9571 |
|              0.89 |        0.9744 |
|              0.90 | 0.9765–0.9774 |
|              0.94 |        0.9762 |
|              0.96 |        0.9770 |
|              0.97 |        0.9773 |
|              0.99 |        0.9766 |
|              1.00 |        0.9760 |

The important result is that the relationship is **not simply monotonic**. The score rises sharply from 0–0.6, reaches a high region around 0.9–0.99, and then remains relatively stable near the top.

### 3. Investigated port

Ports A, B, C and D were compared under otherwise strong configurations.

* Port A: approximately **0.9743–0.9753**
* Port B: approximately **0.9753**
* Port D: approximately **0.9765–0.9774**
* Port C: **0.0430 — DECLINE**

Port C is therefore a major outlier and was effectively ruled out for the high-scoring configuration.

### 4. Investigated recent seizures

Recent seizure values showed a strong effect:

* `0` → approximately **0.8868**
* `3` → approximately **0.9619**
* `4` → approximately **0.9765**
* `5` → approximately **0.9759**

The best tested region was therefore around **4–5**, with 4 repeatedly producing the strongest scores.

### 5. Investigated route age

Route age was tested over a wide range.

Scores were strongest around the mid-20s:

* 18 → **0.9661**
* 20 → approximately **0.9738**
* 23 → **0.9727**
* 24 → **0.9769**
* 25 → approximately **0.9743**
* 26 → **0.9760**
* 27 → **0.9765**
* 28 → **0.9741**
* 29 → **0.9693**
* 35 → **0.9619**
* 45 → **0.9446**
* 50 → **0.9228**
* 60 → **0.8939**

This indicates a clear high-performing region around **24–27 days**, with performance deteriorating as route age becomes substantially larger.

### 6. Investigated shipper score

Shipper score showed a strong threshold/range effect:

* 315 → **0.8741**
* 500 → **0.8521**
* 600 → **0.9485**
* 866 → **0.9755**
* 872 → **0.9755**
* 887 → **0.9751**
* 890 → **0.9774**
* 896 → **0.9765**
* 898 → **0.9759**
* 899 → **0.9754**
* 900 → **0.9765**

The score therefore improves substantially when moving from low shipper scores into the high range. Around **866–900** the system is already operating near its maximum observed performance, with **890 producing the best observed score in this set**.

### 7. Investigated shipper years

Shipper years showed a relatively smooth but non-linear effect.

Examples:

* 28 → **0.9739**
* 29 → **0.9750**
* 30 → **0.9763**
* 31 → **0.9765**
* 32 → **0.9759**
* 33 → **0.9747**
* 35 → **0.9745**
* 40 → **0.9700** under the comparable high-score configuration

The strongest region observed was around **30–31 years**, with performance gradually weakening as the value moved farther away.

### 8. Investigated declared value

Declared value had a comparatively small effect inside the normal operating range.

At the strong baseline:

* 2 → **0.9729**
* 3 → **0.9749**
* 4 → **0.9763–0.9774**
* 5 → **0.9762–0.9774**
* 6 → **0.9752**
* 7 → **0.9735**
* 8 → **0.9746**

The data suggest that **4–5 is a strong region**, but declared value is much less influential than features such as port, discrepancy ratio, recent seizures, route age, and shipper score.

### 9. Investigated container count

Container count was changed between values including 80, 86, 88, 91, 95, 99 and 100.

In the strong configuration, scores remained around **0.9765–0.9774** despite these changes.

This suggests that container count has **little measurable effect within the tested range**, at least under the high-scoring configuration.

### 10. Investigated transfers

Transfers from 0 through 6 were tested repeatedly.

When the other important features were held in the strong region, the score remained approximately **0.9759–0.9765** across many transfer values.

This provides evidence that `transfers` has **little or no measurable effect** in the tested high-scoring region.

### 11. Investigated prior shipments

Prior shipments were varied from 0 through higher values including 3, 10, 18 and 20.

Under comparable high-scoring configurations, the score remained close to the same range.

This suggests that `prior_shipments` is **weak or inactive** relative to the dominant features identified above.

## What we ruled out

### A simple monotonic discrepancy-ratio rule

Rejected.

Increasing discrepancy ratio does not simply increase or decrease the score. Very low values are strongly penalized, while the region around 0.9–0.99 performs best.

### Port C as a viable high-scoring choice

Rejected.

Port C produced **0.0430 / DECLINE** while Port D under the comparable configuration produced approximately **0.9765**.

### Very low shipper scores

Rejected as a high-performing strategy.

Values such as 315 and 500 produced substantially lower scores than the 866–900 range.

### Extreme route ages

Rejected as a high-performing region.

Route ages around 45–60 days produced much lower scores than the 24–27 day region.

### Recent seizures = 0 as a strong configuration

Rejected.

Recent seizures of 0 produced approximately **0.8868**, substantially below the approximately 0.976 range observed around 4–5.

### Transfers as a major scoring lever

Not supported.

Changing transfers repeatedly produced little measurable movement when the dominant variables were held in their strong region.

### Container count as a major scoring lever

Not supported.

Large changes in container count produced little change in the strong configuration.

## What we are still unsure about

* The exact optimum within the `discrepancy_ratio` region between approximately 0.9 and 0.99 is not fully resolved.
* The exact optimum for `shipper_score` between roughly 866 and 900 is not fully resolved, although 890 produced the best observed score of 0.9774.
* The precise interaction between `route_age_days`, `shipper_years`, and `shipper_score` remains uncertain.
* Some low-scoring observations involved simultaneous changes to multiple features, so they should not automatically be interpreted as the effect of a single variable.
* The data establish strong empirical patterns, but they do not establish the internal causal mechanism of the black-box system.
* Scores around 0.9774 are very close to one another, so small differences should not be treated as meaningful without repeated controlled tests.

Overall, Round 2 narrowed the search from broad exploration toward a **stable high-scoring region**, while identifying several sharp failure regions that should be avoided.
