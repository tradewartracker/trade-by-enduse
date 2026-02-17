# trade-by-enduse

This project organizes U.S. import trade data by end-use category using the United Nations **Classification by Broad Economic Categories (BEC)** framework.

## About the UN BEC classification

The UN BEC (Classification by Broad Economic Categories) is designed to convert Harmonized System (HS) product codes into economic use categories aligned with national accounts concepts. BEC Revision 5 provides 19 detailed categories that roll up into three main end-use groups:

- **Capital goods (CAP):** goods used to produce other goods/services (e.g., machinery, equipment)
- **Intermediate goods (INT):** inputs used in further production (e.g., parts, industrial supplies)
- **Consumption goods (CONS):** goods for final household/government consumption (e.g., food, clothing, vehicles)

This project uses a simplified 3-category version. The HS-to-BEC concordance file (`HS2012-17-BEC5 -- 08 Nov 2018.xlsx`) contains some dual-classified products (e.g., `INT/CONS`). These are resolved by taking the first category listed, which is a simplification. Out of 5,387 HS6 codes, only 2 remain unmapped (classified as OTHER).

## Analysis and visualization: `make-end-use-files.ipynb` and `end-use-breakdown.ipynb`

The notebook `make-end-use-files.ipynb` creates `data/hs6-enduse.parquet`, an HS6-level mapping from product codes to end-use classes used throughout the analysis.

The main analysis notebook (`end-use-breakdown.ipynb`) performs time-series analysis and visualization of U.S. import trends by end-use category.

### What it produces

**Time-series charts** (quarterly data with November 2025 monthly overlay):
- U.S. aggregate imports and tariff rates
- Consumption goods imports and tariff rates
- Capital goods imports and tariff rates
- Intermediate goods imports and tariff rates
- AI-relevant capital goods (split by AI-relevant vs. not AI-relevant)
- AI-relevant products across all non-excluded trade

**Comparison bar charts:**
- End-use tariff rates: November 2024 vs. November 2025
- AI-relevant product tariffs: November 2024 vs. November 2025

**Data exports** (`data-output/` folder):
- Quarterly series (excluding incomplete last quarter) and full monthly series for each time-series visualization
- Exports include import levels (`CON_VAL_MO`) and tariff rates
- For AI charts, exports include separate columns for AI-relevant and not-AI-relevant import values

### Key configuration

- **Base year:** 2024 (for relative volume calculations)
- **Excluded HS2 codes:** 27 (mineral fuels), 71 (precious metals/stones), 98, 99 (special classifications)
- **AI relevance mapping:** uses a custom HS6-to-AI-relevance concordance

## Output files

- `data/hs6-enduse.parquet`: cleaned HS6-to-BEC end-use mapping (from `make-end-use-files.ipynb`)
- `data-output/*.csv`: time-series exports at quarterly and monthly frequency (from `end-use-breakdown.ipynb`)

