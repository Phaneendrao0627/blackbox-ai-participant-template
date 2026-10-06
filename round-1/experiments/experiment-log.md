# Round 1 Experiment Log

**Team:** BB-012
**Total queries used:** 10

## Experiment 1 — Discrepancy ratio

| Value |  Score |
| ----: | -----: |
|  1.00 | 0.6534 |
|  0.90 | 0.9610 |
|  0.95 | 0.9605 |
|  0.99 | 0.9763 |

**Observation:** Moving away from 1.00 substantially improved the score. The highest tested value was 0.9763 at 0.99.

---

## Experiment 2 — Recent seizures

| Value |  Score |
| ----: | -----: |
|     5 | 0.9686 |
|     4 | 0.9691 |

**Observation:** Reducing recent_seizures from 5 to 4 produced a small improvement of 0.0005.

---

## Experiment 3 — Shipper score

Clean comparison with the other tested inputs held fixed:

| Value |  Score |
| ----: | -----: |
|   600 | 0.7495 |
|   900 | 0.8680 |

**Observation:** Increasing shipper_score from 600 to 900 produced a substantial improvement.

**Note:** A different experiment state produced a conflicting comparison and was not used as formal evidence.

---

## Experiment 4 — Shipper years

| Value |  Score |
| ----: | -----: |
|    40 | 0.9610 |
|    35 | 0.9651 |
|    30 | 0.9686 |
|    25 | 0.9500 |

**Observation:** Performance improved toward 30 years but decreased again at 25, suggesting a non-monotonic relationship in the tested range.

---

## Experiment 5 — Route age

| Value |  Score |
| ----: | -----: |
|    18 | 0.9691 |
|    27 | 0.9763 |

**Observation:** Increasing route_age_days from 18 to 27 improved the score by 0.0072 and produced the best overall observed result.

---

## Experiment 6 — Port

Ports A, B, C and D were tested under the high-scoring configuration.

**Observation:** Port D produced the best observed result. Port C produced a severe drop to 0.0430, showing that port choice can strongly affect the output.

---

## Additional observations

### Declared value

* 5 → 4: score remained 0.9691.
* 4 → 3: score decreased.

**Conclusion:** 4 was retained, while 3 was rejected. No strong improvement over 5 was established.

### Transfers

* 6 → 5: score remained 0.9691.

**Conclusion:** No measurable effect was observed in this configuration.

### Prior shipments

* 0 → 10: score remained unchanged.

**Conclusion:** No measurable effect was observed in this configuration.
