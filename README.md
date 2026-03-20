# trade-by-enduse

This project organizes U.S. import trade data by end-use category using the United Nations **Classification by Broad Economic Categories (BEC)** framework.

Most Recent Data **January 2026**

## About the UN BEC classification

The UN BEC (Classification by Broad Economic Categories) is designed to convert Harmonized System (HS) product codes into economic use categories aligned with national accounts concepts. BEC Revision 5 provides 19 detailed categories that roll up into three main end-use groups:

- **Capital goods (CAP):** goods used to produce other goods/services (e.g., machinery, equipment)
- **Intermediate goods (INT):** inputs used in further production (e.g., parts, industrial supplies)
- **Consumption goods (CONS):** goods for final household/government consumption (e.g., food, clothing, vehicles)

This project uses a simplified 3-category version. The HS-to-BEC concordance file (`HS2012-17-BEC5 -- 08 Nov 2018.xlsx`) contains some dual-classified products (e.g., `INT/CONS`). These are resolved by taking the first category listed, which is a simplification. Out of 5,387 HS6 codes, only 2 remain unmapped (classified as OTHER).

In looking at the dual case products items like `CAP/CONS` have a lot of computer hardware components. `INT/CONS` has a lot of medical diagnostic tests. Items like `CONS/INT` have fuel products or medical items and `CONS/CAP` is largely cars. 

## Analysis and visualization: `make-end-use-files.ipynb` and `end-use-breakdown.ipynb`

The notebook `make-end-use-files.ipynb` creates `data/hs6-enduse.parquet`, an HS6-level mapping from product codes to end-use classes used throughout the analysis.

The main analysis notebook (`end-use-breakdown.ipynb`) performs time-series analysis and visualization of U.S. import trends by end-use category.

### What it produces

**Time-series charts:**
- U.S. aggregate imports and tariff rates
- Consumption goods imports and tariff rates
- Capital goods imports and tariff rates
- Intermediate goods imports and tariff rates
- AI-relevant capital goods (split by AI-relevant vs. not AI-relevant)
- AI-relevant products across all non-excluded trade

**Incomplete quarter handling:** The notebook handles partial quarters (e.g., when only January data is available for Q1). Complete quarters are plotted as connected lines. Incomplete quarters are shown as labeled dots, scaled to their quarterly equivalent (e.g., 1 month × 3). This allows charts to remain meaningful even when run mid-quarter.

**Data exports** (`data-output/` folder):
- Quarterly and monthly series for each time-series visualization
- Exports include import levels (`CON_VAL_MO`) and tariff rates
- Quarterly exports include a `months_in_quarter` column (1, 2, or 3) to identify incomplete quarters
- For AI charts, exports include separate columns for AI-relevant and not-AI-relevant import values and tariff rates

### Key configuration

- **Base year:** 2024 (for relative volume calculations)
- **Excluded HS2 codes:** 27 (mineral fuels), 71 (precious metals/stones), 98, 99 (special classifications)
- **AI relevance mapping:** uses a custom HS6-to-AI-relevance concordance

## Output files

### Mapping file
- [data/hs6-enduse.parquet](data/hs6-enduse.parquet): HS6-to-BEC end-use mapping (5,387 HS6 codes classified into CAP, INT, CONS, or OTHER)

### U.S. aggregate imports (all end-use categories combined)
- [data-output/us_imports_quarterly.csv](data-output/us_imports_quarterly.csv): Quarterly aggregate U.S. imports and tariff rates (through Q4 2025)
- [data-output/us_imports_monthly.csv](data-output/us_imports_monthly.csv): Monthly aggregate U.S. imports and tariff rates

### Consumption goods imports
- [data-output/consumption_goods_quarterly.csv](data-output/consumption_goods_quarterly.csv): Quarterly consumption goods imports and tariff rates
- [data-output/consumption_goods_monthly.csv](data-output/consumption_goods_monthly.csv): Monthly consumption goods imports and tariff rates

### Capital goods imports
- [data-output/capital_goods_quarterly.csv](data-output/capital_goods_quarterly.csv): Quarterly capital goods imports and tariff rates
- [data-output/capital_goods_monthly.csv](data-output/capital_goods_monthly.csv): Monthly capital goods imports and tariff rates

### Intermediate goods imports
- [data-output/intermediate_goods_quarterly.csv](data-output/intermediate_goods_quarterly.csv): Quarterly intermediate goods imports and tariff rates
- [data-output/intermediate_goods_monthly.csv](data-output/intermediate_goods_monthly.csv): Monthly intermediate goods imports and tariff rates

### AI-relevant products (capital goods only)
- [data-output/ai_capital_goods_quarterly.csv](data-output/ai_capital_goods_quarterly.csv): Quarterly capital goods split by AI-relevant and not-AI-relevant import values and tariff rates
- [data-output/ai_capital_goods_monthly.csv](data-output/ai_capital_goods_monthly.csv): Monthly capital goods split by AI-relevant and not-AI-relevant import values and tariff rates

### AI-relevant products (all non-excluded trade)
- [data-output/ai_nonexcluded_quarterly.csv](data-output/ai_nonexcluded_quarterly.csv): Quarterly all non-excluded imports split by AI-relevant and not-AI-relevant import values and tariff rates
- [data-output/ai_nonexcluded_monthly.csv](data-output/ai_nonexcluded_monthly.csv): Monthly all non-excluded imports split by AI-relevant and not-AI-relevant import values and tariff rates

**Note:** All CSV files include a `time` column. End-use category files contain `CON_VAL_MO` (import value) and `tariff` (effective tariff rate). AI files contain `ai_relevant_CON_VAL_MO`, `ai_relevant_tariff`, `not_ai_relevant_CON_VAL_MO`, and `not_ai_relevant_tariff` columns. Quarterly files include `months_in_quarter` to indicate data completeness (3 = full quarter).

