# 📊 Crypto Sentiment vs. Trader Performance Analysis

**Objective:** An exploratory data analysis (EDA) investigating the behavioral patterns and profitability of Hyperliquid traders across varying market sentiment phases (Bitcoin Fear & Greed Index).

**Data:** 211,224 trade executions from 32 Hyperliquid accounts across 246 coins (2023–2025), merged with the daily Bitcoin Fear & Greed Index.

---

## 🛠️ Tech Stack & Architecture

- **Data Pipeline:** Pandas, NumPy (data cleaning, date-alignment, feature engineering)
- **Analysis & Visualization:** Matplotlib, Seaborn (statistical correlations, distribution plots)

**Note on leverage:** the raw dataset does not include a native account-leverage field (no margin/equity data). To keep Step 3 of the brief intact, position size (`Size USD`) is used as a **proxy** for leverage intensity, bucketed into Low/Medium/High tiers by tercile. This is called out explicitly rather than presented as true leverage.

---

## 🚀 Key Insights & Business Recommendations

These traders, as a group, are not the naive "buy the top" crowd the placeholder hypothesis assumed — the real data tells a more disciplined story:

- **Win rate holds up in Extreme Greed, it doesn't collapse.** Win rate was actually *highest* in Extreme Greed (89.2%) and Fear (87.3%), and lowest in Extreme Fear (76.2%). There is no evidence of a leverage-driven blow-up during greed phases in this account sample — average and median Closed PnL were also highest during Extreme Greed ($130 mean / $8.53 median) rather than lowest.
- **Position sizing shrinks, not grows, in Extreme Greed.** Mean position size is *smallest* during Extreme Greed (~$3,113) and largest during Fear (~$7,818) — the opposite of the "traders lever up into greed" assumption. This account cohort appears to size down, not up, at emotional extremes.
- **Short-side edge is real, but only in Fear, not Greed.** During Extreme Fear, shorts significantly outperformed longs on average PnL ($123 vs $79), consistent with a counter-cyclical short bias paying off. During Extreme Greed, the pattern flips: longs modestly outperformed shorts ($62 vs $29 average PnL), so a blanket "always short greed" rule would have underperformed here.
- **Bigger positions are simply more profitable in dollar terms, regardless of sentiment.** The High position-size tier outperformed Low/Medium tiers in every sentiment bucket (e.g. $397.69 mean PnL in the High tier during Extreme Greed vs $11.72 in Low), and overall correlation between position size and Closed PnL is weakly positive (r ≈ 0.12). Sentiment score itself is essentially uncorrelated with both position size (r ≈ -0.03) and PnL (r ≈ 0.01) at the trade level.
- **Risk framing:** rather than "greed causes blow-ups," the more supportable takeaway from this dataset is that *this particular trader cohort is skilled/selective enough that greed phases haven't hurt them* — which itself is useful for a platform: risk monitoring should key off position concentration and account-level drawdown, not sentiment score alone, since sentiment alone is a weak predictor of PnL outcomes here.

*(All figures above are computed live in `notebook.ipynb` — Step 5 — and will regenerate if the underlying data changes.)*

---

## 📁 Repository Structure

```
crypto-sentiment-trader-analysis/
├── README.md
├── notebook.ipynb
├── requirements.txt
└── data/
    ├── historical_data.csv
    └── fear_greed_index.csv
```

## ▶️ Reproducing the Analysis

```bash
pip install -r requirements.txt
jupyter notebook notebook.ipynb
```

## 📓 Notebook Contents

1. **Data Ingestion & Preprocessing** — load both datasets, standardize timestamps to `YYYY-MM-DD`, drop zero-notional/duplicate rows
2. **Merging Datasets** — inner join trades to daily sentiment on `date` (211,218 / 211,224 trades retained)
3. **Feature Engineering** — per-account win rate by sentiment, position-size tiers (leverage proxy), trade duration reconstruction
4. **EDA & Visualization** — PnL-by-sentiment box plot, sentiment/size/PnL correlation heatmap, Long vs. Short bar chart in extreme sentiment
5. **Statistical Validation** — `groupby().describe()` tables for PnL, win rate, and position size across sentiment tiers
