# Market Regime Detection in Chilean Equities
### Hidden Markov Models on the IPSA, mapped to structural-break events

![R](https://img.shields.io/badge/R-tidyverse%20%2F%20depmixS4-276DC3?logo=r&logoColor=white)
![Method](https://img.shields.io/badge/Method-HMM%20%2B%20k--means-8A2BE2)
![Market](https://img.shields.io/badge/Market-IPSA%20Chile-2E7D32)

Do Chile's political and economic shocks leave a common fingerprint across its stock market? This project infers hidden return regimes for each stock in the **IPSA** (Santiago Exchange's main index), harmonizes them into a handful of interpretable market-wide regime types, and flags the days when the whole market switches at once.

> **When does the Chilean stock market change behavioral regime — and do those switches line up with known political and economic shocks?**

The detected switch dates are then annotated with the real events they coincide with — the 2019 *estallido social*, the constitutional process, national elections, and global market shocks.

---

## Pipeline

1. **Data** — pull daily adjusted prices for the ~28 IPSA constituents from Yahoo Finance (2018–present) and compute log returns.
2. **Per-asset regimes (HMM)** — fit a 5-state Gaussian Hidden Markov Model to each stock's return series (`depmixS4`) and Viterbi-decode its latent state path.
3. **Harmonize regimes (clustering)** — raw HMM states aren't comparable across stocks, so each state is summarized by its (mean, volatility) of returns and k-means-clustered into 5 legible, cross-asset regime types: *Bullish Stable, Bullish Strong, Bearish Stable, Bearish Shock,* and *Volatile Neutral*.
4. **Market-wide switch detection** — count how many assets change regime each day and flag co-movement spikes (rolling mean + 2·SD, with a local-peak filter) as candidate structural-break dates.
5. **Event overlay** — render the whole thing as a regime heatmap (asset × time) and annotate the detected switch dates with the events they align with.

---

## What it surfaces

- **The data-driven switch dates land on real, nameable shocks.** The unsupervised pipeline flags dates that coincide with both domestic political milestones (the October 2019 social uprising, the constitutional-convention and council votes, the 2021 elections) and global market shocks (the April 2025 tariff selloff) — a sanity check that it's catching genuine structural breaks rather than noise.
- **Clustering makes dozens of incomparable HMM states legible.** Mapping every stock's idiosyncratic latent states onto five shared regime types is what lets the entire market fit into a single, readable heatmap.
- **Both local and global breaks show up.** Because regimes are inferred purely from returns, the method picks up Chile-specific political ruptures and worldwide risk-off episodes without being told about either.

*Exploratory and illustrative: events are overlaid on detected dates as a validation check, not identified through a formal structural-break test.*

---

## Repository structure

```
├── trading_regimes_chile.Rmd   # end-to-end pipeline: data → HMM → clustering → plots
├── figures/                     # exported regime heatmaps with event annotations
└── README.md
```
*(adjust as you split things out)*

---

## Data

Yahoo Finance — daily adjusted close for IPSA (Santiago Stock Exchange, `.SN`) constituents, 2018–present.

---

## Tech stack

- **R** — `tidyverse` (wrangling), `tidyquant` (Yahoo Finance data), `depmixS4` (Hidden Markov Models), `kmeans` (regime clustering), `zoo` / `slider` (rolling spike detection), `ggplot2` (heatmap visualization).

---

## Author

**Lucas Leturia**
