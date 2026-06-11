# Fixed-distance bid depth ratio deciles

These tables complement the share-based thinning metrics with distributional thinning magnitude. Interpret the ratio as follows: `ratio < 1` means below the comparison baseline, `ratio = 0.5` means roughly half of the baseline, and `ratio > 1` means above the comparison baseline. Deciles run from `p10` to `p100` across all valid finite coin/event rows. Missing values, infinite values, and ratios made undefined by a zero baseline are excluded from the decile calculation and counted in the CSV column `n_missing_or_nonfinite`.

## Exact ADL peak vs hourly baseline

| metric   | band   |   n |   p10_ratio |   p20_ratio |   p30_ratio |   p40_ratio |   p50_ratio |   p60_ratio |   p70_ratio |   p80_ratio |   p90_ratio |   p100_ratio |
|:---------|:-------|----:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|-------------:|
| spread   | spread | 162 |       1.902 |       2.861 |       3.862 |       4.613 |       5.918 |       7.622 |       9.723 |       15.23 |       30.26 |        101.8 |


| metric    | band    |   n |   p10_ratio |   p20_ratio |   p30_ratio |   p40_ratio |   p50_ratio |   p60_ratio |   p70_ratio |   p80_ratio |   p90_ratio |   p100_ratio |
|:----------|:--------|----:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|-------------:|
| bid depth | 25 bps  | 109 |           0 |           0 |           0 |           0 |           0 |       0     |       0     |       0     |       0     |        5.162 |
| bid depth | 50 bps  | 142 |           0 |           0 |           0 |           0 |           0 |       0     |       0     |       0.031 |       0.247 |        9.438 |
| bid depth | 100 bps | 161 |           0 |           0 |           0 |           0 |           0 |       0.008 |       0.032 |       0.09  |       1     |       40.52  |

## ADL peak +/-1 minute window vs hourly baseline

| metric     | band   |   n |   p10_ratio |   p20_ratio |   p30_ratio |   p40_ratio |   p50_ratio |   p60_ratio |   p70_ratio |   p80_ratio |   p90_ratio |   p100_ratio |
|:-----------|:-------|----:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|-------------:|
| max spread | spread | 162 |       3.083 |       4.572 |       6.541 |       8.548 |       10.11 |       12.87 |       18.74 |        27.6 |       41.05 |        217.7 |


| metric        | band    |   n |   p10_ratio |   p20_ratio |   p30_ratio |   p40_ratio |   p50_ratio |   p60_ratio |   p70_ratio |   p80_ratio |   p90_ratio |   p100_ratio |
|:--------------|:--------|----:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|-------------:|
| min bid depth | 50 bps  | 142 |           0 |           0 |           0 |           0 |           0 |           0 |           0 |       0     |       0.014 |        0.503 |
| min bid depth | 100 bps | 161 |           0 |           0 |           0 |           0 |           0 |           0 |           0 |       0.004 |       0.042 |       18.7   |

## Exact fill peak vs hourly baseline

| metric   | band   |   n |   p10_ratio |   p20_ratio |   p30_ratio |   p40_ratio |   p50_ratio |   p60_ratio |   p70_ratio |   p80_ratio |   p90_ratio |   p100_ratio |
|:---------|:-------|----:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|-------------:|
| spread   | spread | 182 |       0.593 |       1.001 |        1.52 |        2.11 |       2.797 |       3.713 |       4.739 |       6.605 |       11.79 |        68.43 |


| metric    | band    |   n |   p10_ratio |   p20_ratio |   p30_ratio |   p40_ratio |   p50_ratio |   p60_ratio |   p70_ratio |   p80_ratio |   p90_ratio | p100_ratio   |
|:----------|:--------|----:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|:-------------|
| bid depth | 25 bps  | 119 |           0 |           0 |       0     |       0     |       0     |       0     |       0.006 |       1.125 |       10.32 | 32,497.9     |
| bid depth | 50 bps  | 159 |           0 |           0 |       0     |       0     |       0.036 |       0.233 |       1.378 |       2.753 |        8.94 | 1,626.7      |
| bid depth | 100 bps | 179 |           0 |           0 |       0.016 |       0.077 |       0.238 |       0.886 |       1.589 |       4.075 |       11.8  | 522.8        |

## Exact fill peak vs previous 5-minute local baseline

| metric   | band   |   n |   p10_ratio |   p20_ratio |   p30_ratio |   p40_ratio |   p50_ratio |   p60_ratio |   p70_ratio |   p80_ratio |   p90_ratio |   p100_ratio |
|:---------|:-------|----:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|-------------:|
| spread   | spread | 182 |       1.222 |       1.938 |       2.324 |         3.1 |       3.879 |       4.629 |       5.934 |       8.509 |       13.04 |         35.6 |


| metric    | band    |   n |   p10_ratio |   p20_ratio |   p30_ratio |   p40_ratio |   p50_ratio |   p60_ratio |   p70_ratio |   p80_ratio |   p90_ratio |   p100_ratio |
|:----------|:--------|----:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|-------------:|
| bid depth | 50 bps  | 166 |           0 |           0 |       0     |       0     |       0.005 |       0.026 |       0.108 |       0.324 |       0.662 |       214.7  |
| bid depth | 100 bps | 177 |           0 |           0 |       0.006 |       0.022 |       0.052 |       0.125 |       0.206 |       0.388 |       0.862 |        35.43 |

## How to read

- `p50_ratio` is the median. For example, `bid depth 50 bps p50_ratio = 0.5` means half of the samples have 50 bps bid depth no higher than half of the baseline.
- For bid depth, lower values indicate a thinner bid book. For spread, higher values indicate a wider spread.
- These are single-metric ratio distributions, not the joint share where both spread widens and bid depth thins at the same time.
- `p100_ratio` is the maximum valid finite sample value. It can be affected by very small baselines or liquidity replenishment, so the `p10-p90` body of the distribution should carry more interpretive weight.
