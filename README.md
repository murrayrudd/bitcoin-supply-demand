# Bitcoin Supply-Demand Price Model

An interactive web-based implementation of the Epstein-Zin supply-demand framework for Bitcoin price formation, based on peer-reviewed research.

## Overview

This tool allows users to explore how changes in market demand, investor time preferences, Bitcoin liquid supply assumptions, and withdrawal behavior shape Bitcoin price trajectories from the 4th halving (April 2024) through the 7th halving (April 2036).

The model computes daily equilibrium prices by intersecting Bitcoin's perfectly inelastic supply curve with a demand function that grows over time according to a logistic adoption path. All computations run client-side in the browser; no data is collected or transmitted.

**Live model:** [https://murrayrudd.github.io/bitcoin-supply-demand/](https://murrayrudd.github.io/bitcoin-supply-demand/)

## Research basis

This tool implements the framework described in:

- **Rudd, M.A. & Porter, D. (2025).** "Bitcoin supply, demand, and price dynamics." *Journal of Risk and Financial Management*, 18(10), 570. [https://doi.org/10.3390/jrfm18100570]

The model builds on earlier work establishing the supply-demand framework:
- **Rudd, M.A. & Porter, D. (2025).** "A supply and demand framework for Bitcoin price forecasting." *Journal of Risk and Financial Management*, 18(2), 66. [https://doi.org/10.3390/jrfm18020066]


## Features

- **Seven adjustable parameters** spanning supply assumptions, adoption curve shape, demand preferences, and withdrawal behavior
- **Five chart views:** price trajectory, liquid supply, supply flows (miner production vs. reserve withdrawals), adoption curve dynamics, and price-supply overlay
- **Summary statistics:** forecast April 2036 price, 12-year CAGR, total market capitalization, date of $1M milestone, and final liquid supply
- **Dynamic Y-axis scaling** that auto-adjusts for moderate scenarios and caps at $25M for hyperbolic trajectories
- Fully self-contained single HTML file with no build step or server dependencies

## Adjustable parameters

| Parameter | Description | Default | Range |
|-----------|-------------|---------|-------|
| Satoshi coins | BTC presumed inaccessible in Satoshi wallets | 968,000 | 0 -- 1.1M |
| Lost BTC | Permanently lost coins | 1,570,000 | 0 -- 4M |
| Illiquid BTC | Long-term HODLed coins | 5,760,000 | 0 -- 14M |
| T* | Years to adoption saturation | 14 | 6 -- 16 |
| L_min | Logistic curve minimum (shape parameter) | 0.05 | 0.01 -- 0.08 |
| D | Demand multiplier | 20 | 10 -- 100 |
| rho | EZ intertemporal substitution parameter | 2.0 | 0.5 -- 2.5 |
| q_base | Daily BTC withdrawals to reserve | 2,000 | 1,000 -- 8,000 |
| alpha | Withdrawal sensitivity to price | 0.10 | 0.025 -- 0.50 |

Initial liquid supply is derived from circulating supply (19,687,500 BTC at the 4th halving) minus Satoshi, lost, and illiquid coins, constrained to a 1M -- 16.5M range.

## Technical details

The price equation at each daily time step is:

```
P(t) = A'(t) * P0 * [(q0 - illiquid - satoshi) / liquid_supply(t)]^(1/rho)
```

where A'(t) is the logistic demand trajectory scaled by multiplier D, and liquid supply evolves daily based on miner emissions (following the halving schedule) minus price-sensitive reserve withdrawals.

Key implementation details matching the Analytica reference model:
- Logistic growth rate derived from logit transformation of L_min and L_max
- Withdrawal sensitivity uses the previous day's price (one-step lag)
- Day indexing starts at 1 (matching the Analytica convention)
- Lost coins reduce liquid supply but are excluded from the demand calibration base

## Usage

The model is a single `index.html` file. To run locally, download and open in any modern browser. To host publicly, deploy to any static hosting service (GitHub Pages, Netlify, Vercel, or your own server).

### GitHub Pages deployment

1. Fork or clone this repository
2. Go to **Settings > Pages**
3. Set source to **Deploy from a branch**, branch **main**, folder **/ (root)**
4. The model will be live at `https://YOURUSERNAME.github.io/bitcoin-supply-demand/`

## Dependencies

All dependencies are loaded from CDN at runtime:
- React 18.2
- Recharts 2.12
- Babel Standalone 7.23 (for in-browser JSX compilation)
- Google Fonts (IBM Plex Sans, JetBrains Mono, Playfair Display)

## License

MIT License. See [LICENSE](LICENSE) for details.

## Citation

If you use this tool in research or publications, please cite the underlying paper:

```bibtex
@article{RN17969,
   author = {Rudd, Murray A. and Porter, Dennis},
   title = {Bitcoin supply, demand, and price dynamics},
   journal = {Journal of Risk and Financial Management},
   volume = {18},
   number = {10},
   pages = {570},
   ISSN = {1911-8074},
   DOI = {https://doi.org/10.3390/jrfm18100570},
   url = {https://www.mdpi.com/1911-8074/18/10/570},
   year = {2025},
   type = {Journal Article}
}

```

## Author

**Murray A. Rudd, Ph.D.** -- Applied microeconomist and policy researcher

- Blog: [murrayrudd.pro](https://murrayrudd.pro)
- ORCID: [0000-0001-9533-5070](https://orcid.org/0000-0001-9533-5070)
- Google Scholar: [Profile](https://scholar.google.co.uk/citations?hl=en&user=84qbofEAAAAJ)

