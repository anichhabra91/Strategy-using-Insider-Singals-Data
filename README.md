# Insider Signals: To Trade or Not To Trade

## Overview

This project builds a full research and data pipeline to evaluate whether insider trading activity provides predictive information about short-term stock returns in modern, highly efficient markets. Using SEC Form 3/4/5 filings from 2006–2025, combined with market and volume data, the project constructs insider-based signals, performs clustering analysis, runs causal regressions, and executes bias-free systematic backtests.

## Objective

- Determine whether insider trades still carry short-horizon predictive power.
- Improve raw insider signals using insider seniority, clustering, and engineered features.
- Build naïve and refined insider trading signals.
- Construct realistic, look-ahead-free backtests.
- Evaluate strategy robustness across market regimes.

## Dataset

- SEC Form 3, 4, 5 filings (2006–2025)
- Daily price & volume data
- Fama–French 3-factor data
- Firm-level metadata (sector, index membership)

## Feature Engineering

Key constructed variables include:

- **Net Transaction Value**: Aggregated insider buys/sells per firm-day  
- **Insider Seniority Counts**: Level 1 (executives), Level 2, Level 3 insiders  
- **Pre-Trade Returns**: 7-day and 30-day rolling excess returns  
- **Volume Attention**: 7-day and 30-day volume z-scores  
- **Rolling Beta**: 60-day market beta estimate  
- **Clustering Features**: Transaction magnitude, seniority mix, volume context  

## Clustering

Insider transactions are grouped using KMeans and economically aggregated into:

- **Buy-Dominant Clusters**
- **Sell-Dominant Clusters**
- **Neutral Clusters**

This helps separate high-conviction trades from routine, noise-driven activity.

## Signals

### 1. Naïve Insider Signal
A 52-week z-score of net insider transaction value:
- Go long top 10% (heavy insider buying)
- Go short bottom 10% (heavy insider selling)

### 2. Refined Public Score
Weighted insider signal incorporating seniority:
PublicScore = z(NetTransaction) + z(0.5 * L1 + 0.3 * L2 + 0.2 * L3)
Where L1/L2/L3 are counts of trades by insider level.

This improved score places more emphasis on senior executives, who exhibit the strongest informational advantage.

## Backtesting

- Weekly portfolio formation (signals computed from last week’s filings)
- Trades executed at next-week open
- Holding periods: **5-day** and **21-day**
- Strict avoidance of look-ahead bias
- Comparison vs naïve signal and baseline benchmarks

## Performance Summary

Key findings:

- Insider activity retains **statistically significant predictive power** over 7–30 day horizons.
- Senior executives (Level 1) provide the strongest signals.
- Combining buys and sells into a unified score increases robustness.
- The refined Public Score achieves **higher Sharpe ratios** and **shallower drawdowns**.
- Proper timing (bias-free) dramatically reduces inflated performance.

## Key Takeaways

- Insider trades contain meaningful but subtle information.
- Raw insider activity is noisy; clustering and seniority weighting are essential.
- Predictive strength is short-lived, reflecting gradual market incorporation.
- Insider signals work best as a **complementary factor**, not standalone alpha.
- Realistic execution requires accounting for filing delays and reporting noise.

## Conclusion

Modern insider trading data still contains valuable predictive content, but only after careful cleaning, filtering, and contextualization. Rule-based, interpretable insider signals—especially those weighted by insider seniority—provide improved predictive power and more stable returns compared to simple net transaction metrics. Future extensions include machine-learning–based scoring, insider track record modeling, and integration with multi-factor systems.
