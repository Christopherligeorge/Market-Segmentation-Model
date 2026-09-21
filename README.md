# Customer segmentation and lifetime value (R, SQL)

Coursework from a Coursera marketing-analytics course. The R scripts follow the course's five modules and run on the course's **simulated** retail dataset. I worked through them to learn recency/frequency/monetary (RFM) segmentation, scoring models and a transition-matrix lifetime-value projection. This is a learning project, not a deployed model.

## Data

`purchases.txt`: 51,243 purchases by 18,417 customers between 2005 and 2015 (customer id, purchase amount, date). Supplied by the course.

## Scripts

| File | What it does |
|---|---|
| `EDA.R` | Loads the data and summarizes purchases per year, average amount and total amount with SQL (`sqldf`) |
| `clustering and features.R` | Builds recency, frequency and average-amount features in SQL, log-transforms and standardizes them, and runs hierarchical clustering (Ward) on a 10% sample, cut into five clusters |
| `Managerial segmentation.R` | Rule-based segments: inactive, cold, warm and active, split further into new customers and high or low value (average purchase of $100 or more). Repeats the segmentation as of 2014 and compares revenue by segment |
| `Model calibration.R` | Uses 2014 features to predict 2015: a multinomial logit for the probability of being active and a log-log regression for spend, then scores the 2015 customer base |
| `CLTV prediction.R` | Builds the 2014 to 2015 segment transition matrix, projects segment sizes ten years ahead, applies revenue per segment and a 10% discount rate |

## What the scripts produce

Customer segments at the end of 2015:

| Segment | Customers |
|---|---:|
| Inactive (no purchase in 3+ years) | 9,158 |
| Cold | 1,903 |
| Warm, high value | 119 |
| Warm, low value | 901 |
| New warm | 938 |
| Active, high value | 573 |
| Active, low value | 3,313 |
| New active | 1,512 |

Under the scenario coded in `CLTV prediction.R` (1,000 new customers a year, 10% discount rate), discounted revenue for 2016 to 2025 comes to about $3.31 million.

## Run it

R with the `sqldf` and `nnet` packages:

```sh
Rscript "CLTV prediction.R"
```

## Limits

The data is simulated and the script structure comes from the course. The scoring models are calibrated the way the course does it, without a separate hold-out test. Nothing here was used on real customers.
