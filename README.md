# Porter: Optimizing Delivery for the Digital Diner

## Project Overview

This Excel analytics project investigates delivery performance for a food-delivery platform using order, restaurant-category, market, timestamp, order-value, and delivery-partner data. The goal is to identify delivery bottlenecks and provide operational recommendations that could improve delivery speed and customer experience.

**Tools:** Microsoft Excel, PivotTables, calculated/helper columns, charts, and an interactive-style dashboard.

**Dataset:** [Porter Delivery Time Estimation — Kaggle](https://www.kaggle.com/datasets/ranitsarkar01/porter-delivery-time-estimation)

## Dashboard KPIs

| KPI | Result |
|---|---:|
| Orders counted by market | 196,441 |
| Average delivery time | 48.47 minutes |
| Average order value (subtotal) | 2,682.33 |
| Orders delivered within 30 minutes | 13% |

> The workbook contains 197,428 order records, while 196,441 have a non-blank `market_id`. The market-volume KPI therefore counts records with a market ID; totals can differ when a field has missing values.

## Basic-Level Questions and Findings

### 1. What is the distribution of order volumes across markets?

| Market ID | Orders |
|---:|---:|
| 1 | 38,037 |
| 2 | 55,058 |
| 3 | 23,297 |
| 4 | 47,599 |
| 5 | 18,000 |
| 6 | 14,450 |

**Finding:** Market 2 has the highest recorded volume (55,058), followed by Market 4 (47,599). Market 6 has the lowest volume (14,450). Operations should prioritize partner coverage and capacity planning in Markets 2 and 4 while reviewing whether lower-volume markets need different staffing levels.

### 2. How does average delivery time vary by restaurant category?

The overall average delivery time is **48.47 minutes**. The ten categories with the highest average delivery times in the PivotTable are:

| Restaurant category | Average delivery time |
|---|---:|
| Italian | 70.20 min |
| Spanish | 56.18 min |
| Caribbean | 56.10 min |
| Brazilian | 55.97 min |
| Belgian | 54.99 min |
| Tapas | 54.25 min |
| Cajun | 52.50 min |
| Malaysian | 52.29 min |
| Japanese | 52.08 min |
| Afghan | 51.66 min |

**Finding:** Italian orders have the highest category-level average (70.20 minutes), considerably above the overall average. These categories should be investigated for preparation delays, pickup wait time, location, and partner availability. Some categories have very small order counts, so averages should be interpreted alongside sample size.

### 3. What are the peak hours for order placement?

| Order hour | Orders |
|---:|---:|
| 02:00 | 36,976 |
| 01:00 | 28,190 |
| 03:00 | 27,068 |
| 20:00 | 15,560 |
| 04:00 | 15,250 |
| 19:00 | 13,541 |
| 00:00 | 12,669 |
| 21:00 | 11,465 |
| 22:00 | 8,821 |
| 23:00 | 8,163 |

**Finding:** The largest recorded order volumes occur at 01:00–03:00, with 02:00 the highest. There is also a notable evening cluster around 19:00–21:00. Before changing staffing, confirm the timezone and timestamp interpretation because the concentration in the early morning is unusual for food delivery.

### 4. Which days have the highest order volumes?

| Day | Orders |
|---|---:|
| Saturday | 34,541 |
| Sunday | 33,620 |
| Friday | 27,875 |
| Monday | 27,403 |
| Thursday | 25,673 |
| Wednesday | 24,254 |
| Tuesday | 24,062 |

**Finding:** Saturday and Sunday have the highest volumes. Weekend staffing and partner incentives should be planned around this demand, with Saturday receiving the highest priority.

### 5. What is the correlation between the number of items and subtotal?

The Pearson correlation between `total_items` and `subtotal` is **0.5581**.

**Finding:** This is a moderate positive relationship: orders with more items tend to have higher subtotals, although item count does not determine order value by itself. Promotions should be evaluated using both basket size and subtotal.

### 6. What is the distribution of delivery times, and are there outliers?

The median delivery time is approximately **44.33 minutes**. The first quartile (Q1) is **35.07 minutes**, and the third quartile (Q3) is **56.35 minutes**. Using the 1.5 × IQR rule, the upper outlier threshold is approximately **88.28 minutes**. There are **6,277** recorded delivery durations above this threshold, and the raw field contains an extreme maximum of about **141,947.65 minutes**.

**Finding:** The extreme maximum is likely a data-quality issue or an anomalous timestamp, and it can distort averages and standard deviations. Validate unusually long records before operational conclusions; report median and percentile measures alongside the mean.

### 7. What percentage of orders are delivered within 30 minutes?

Approximately **13.13%** of non-missing delivery-duration records are 30 minutes or less (displayed as **13%** on the dashboard).

**Finding:** Only about one in eight orders meets the 30-minute threshold. Porter should examine pickup delays, partner availability, dispatch timing, and distance-related factors before setting a more ambitious delivery target.

### 8. How do order volumes correlate with busy delivery partners?

The dataset does not provide a standalone order-volume count for every busy-partner value, so `total_outstanding_orders` is used as a workload proxy. The correlation between `total_busy_partners` and `total_outstanding_orders` is **0.933** across records with both values available.

**Finding:** Busy-partner counts rise strongly with outstanding workload. This is consistent with delivery partners becoming occupied as the backlog grows. Staffing decisions should use outstanding orders and partner availability together rather than relying on busy-partner counts alone.

### 9. What is the impact of weekday on average delivery time?

| Day | Average delivery time |
|---|---:|
| Sunday | 52.72 min |
| Monday | 51.71 min |
| Tuesday | 46.33 min |
| Wednesday | 44.47 min |
| Thursday | 47.10 min |
| Friday | 44.75 min |
| Saturday | 50.09 min |

**Finding:** Sunday has the highest average delivery time (52.72 minutes), followed by Monday (51.71) and Saturday (50.09). Wednesday is the fastest on average (44.47). The combination of higher weekend order volumes and longer Sunday/Saturday delivery times suggests weekend capacity and dispatch performance deserve attention.

### 10. Which five stores have the longest average delivery times?

The workbook's helper-duration PivotTable shows these top five anonymized store IDs:

| Store ID | Average helper delivery time |
|---|---:|
| `decc2e06a44e61f12a030bc4951563eb` | 87.88 min |
| `af141196b05c7fe6f8d290c41aa63f96` | 87.52 min |
| `a081cab429ff7a3b96e0a07319f1049e` | 87.18 min |
| `42d02bd0c73cb27e4ffc7862910ea1f4` | 87.13 min |
| `e355ad06c5a89f911fbb0aff2de52435` | 85.98 min |

**Finding:** These stores are candidates for a targeted operational review. Store IDs are anonymized, so the workbook cannot identify their real-world names. Confirm each store's order count and validate the helper-duration calculation before treating the ranking as definitive.

## Medium-Level Questions and Findings

### 1. How efficient are the different order protocols?

| Order protocol | Average delivery time |
|---:|---:|
| 7 | 41.37 min |
| 5 | 45.71 min |
| 3 | 46.96 min |
| 2 | 47.36 min |
| 4 | 48.43 min |
| 1 | 52.52 min |
| 6 | 61.09 min |

**Finding:** Protocol 7 has the lowest average delivery time (41.37 minutes), while Protocol 6 has the highest (61.09 minutes). The protocol codes are not mapped to descriptions in the supplied data dictionary, so the operational meaning of each code should be confirmed before changing processes.

### 2. How does average order value vary by market?

| Market ID | Average subtotal |
|---:|---:|
| 4 | 2,896.37 |
| 1 | 2,744.39 |
| 2 | 2,632.54 |
| 3 | 2,575.67 |
| 6 | 2,512.99 |
| 5 | 2,411.93 |

**Finding:** Market 4 has the highest average order value (2,896.37), while Market 5 has the lowest (2,411.93). Marketing can test basket-building offers in Market 5 and study Market 4's product mix. Values are in the dataset's original monetary units.

### 3. What is the relationship between on-shift partners and delivery time during peak hours?

The highest-volume hours are 01:00, 02:00, and 03:00. For these hours combined, the correlation between `total_onshift_partners` and delivery time is approximately **-0.038**, indicating a very weak negative linear relationship. The broader PivotTable also shows average delivery time generally falling from about 54.54 minutes when one partner is on shift to roughly 46–48 minutes at higher partner counts, although the pattern is not perfectly consistent.

**Finding:** More on-shift partners may help, but partner count alone explains little of the delivery-time variation. Combine staffing analysis with outstanding orders, pickup delays, market, and store-level factors.

### 4. What is the impact of total items and distinct items on delivery time?

The calculated correlations with raw delivery time are very small: `total_items` **0.0049** and `num_distinct_items` **0.0067**. A simple linear regression using delivery durations capped to a plausible range (0–180 minutes) also has low explanatory power (R² approximately **0.026**).

**Finding:** In this dataset, item count and distinct-item count alone do not meaningfully explain delivery duration. Preparation time, restaurant operations, market conditions, dispatch, and partner availability likely need to be considered. The model is descriptive and does not establish causation.

### 5. What trends appear in delivery-time fluctuations throughout the week?

The weekday averages range from **44.47 minutes on Wednesday** to **52.72 minutes on Sunday**. Monday is also high at **51.71 minutes**, and Saturday averages **50.09 minutes**.

**Finding:** Delivery performance is weakest on Sunday and Monday in the weekday PivotTable. Operations should compare staffing and backlog levels on these days with Wednesday and Friday, which have lower average delivery times.

### 6. How does item price range relate to order subtotal?

The correlation between `Price Range` (maximum item price minus minimum item price) and `subtotal` is **0.5096**.

**Finding:** There is a moderate positive relationship: orders with a wider spread between the cheapest and most expensive item tend to have higher subtotals. This is an association, not proof that price range causes higher spending.

### 7. Which restaurant categories are most likely to have above-average order values?

The overall average subtotal is **2,682.37**. The highest proportions of orders above this average are:

| Restaurant category | Share of orders above average value |
|---|---:|
| Moroccan | 80.00% |
| Cheese | 70.83% |
| Burmese | 67.60% |
| Lebanese | 66.67% |
| Irish | 65.45% |
| Italian | 63.69% |
| Steak | 63.19% |
| Russian | 63.16% |
| Spanish | 62.16% |
| Malaysian | 61.76% |

**Finding:** Moroccan, cheese, and Burmese categories have the highest shares of above-average-value orders in the PivotTable. These categories may be useful for basket-value campaigns, but order counts should be checked because smaller categories can produce volatile percentages.

### 8. Is delivery-time variability significant across markets or categories?

The PivotTable's standard deviation of delivery time by market is:

| Market ID | Standard deviation |
|---:|---:|
| 1 | 728.45 min |
| 2 | 17.95 min |
| 3 | 19.93 min |
| 4 | 33.67 min |
| 5 | 17.88 min |
| 6 | 19.80 min |

**Finding:** Market 1's standard deviation is dramatically higher than the others and is likely inflated by extreme delivery-time outliers. Market 4 also has greater variability (33.67 minutes) than Markets 2, 3, 5, and 6. Clean anomalous durations before making a final comparison. The workbook's displayed table reports variability by market; it does not provide a complete category-level standard-deviation table.

### 9. How does busy-versus-on-shift partner availability relate to delivery time across markets?

The market-level PivotTable shows the average busy-partner ratio and average delivery time:

| Market ID | Average busy-partner ratio | Average delivery time |
|---:|---:|---:|
| 1 | 0.903 | 55.21 min |
| 2 | 0.963 | 46.07 min |
| 3 | 0.789 | 47.70 min |
| 4 | 0.967 | 47.37 min |
| 5 | 0.855 | 46.50 min |
| 6 | 0.042 | 47.26 min |

Across these six market averages, the correlation is approximately **+0.149**, which is weak. Market 1 has a relatively high busy-partner ratio and the highest average delivery time, but Markets 2 and 4 also have high ratios without similarly high delivery times.

**Finding:** The relationship is not consistent across markets. Market 1 should be investigated first, while the very low ratio for Market 6 should be checked for missing data or a calculation issue before drawing conclusions.

### 10. How do first-time and repeat customers differ in items and subtotal?

**Not answerable with the available dataset.** The workbook does not contain a customer ID or order-history identifier that can reliably distinguish first-time from repeat customers. This question was excluded rather than inferring customer status without evidence.

## Key Recommendations

1. **Prioritize high-volume markets:** Plan delivery-partner capacity around Markets 2 and 4, which have the highest recorded order volumes.
2. **Investigate slow categories and stores:** Review Italian-category orders and the five anonymized stores with the longest helper delivery times. Validate order counts before taking action.
3. **Improve weekend operations:** Saturday and Sunday have the highest order volumes; Sunday also has the highest average delivery time.
4. **Review protocol performance:** Investigate why Protocol 7 is faster and Protocol 6 slower, after mapping protocol codes to their real operational meanings.
5. **Strengthen data quality checks:** Investigate extreme delivery durations, negative partner/order values, missing market IDs, and the unusual early-morning order concentration.
6. **Use multiple workload indicators:** Combine outstanding orders, partner availability, and store/market context instead of using a single metric to schedule partners.
7. **Improve the next analysis iteration:** Add customer IDs, store locations, delivery distance, preparation/pickup timestamps, and documented protocol labels to enable deeper analysis.

## Limitations

- Market IDs, store IDs, and protocol codes are anonymized or not fully documented.
- Some fields contain missing values and implausible/extreme observations.
- The 30-minute KPI is based on recorded delivery durations; it is not a customer satisfaction score.
- Correlation and regression describe relationships in this dataset and do not establish causation.
- First-time versus repeat-customer analysis cannot be performed without a customer identifier.
