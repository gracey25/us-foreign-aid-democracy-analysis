# Data Dictionary

## 1. Scope and Unit of Observation

This project combines V-Dem democracy measures with U.S. foreign
assistance obligations to examine DRG funding and democratic
trajectories during 2001–2024.

The processed analysis panel contains country-year observations.
Aid represents U.S. fiscal-year obligations; democracy measures
represent calendar-year observations. Matching year labels does
not align identical reporting periods.

## 2. Data Sources and File Documentation

### V-Dem Country-Year Dataset: Preprocessed Subset

- Provider: V-Dem Institute, University of Gothenburg.
- Release: Version 16.
- Repository file: `data/raw/vdem_1999_2024_subset.csv`.
- Coverage: 1999–2024.
- Dimensions: 4,634 rows × 7 columns.
- Preprocessing notebook: `notebooks/00_vdem_preprocessing.ipynb`.
- Status: This file is a reduced subset, not the original full download.
- Retained variables: country_name, country_text_id, year, v2x_polyarchy, v2x_libdem, v2x_cspart, v2x_regime

I reduced the original download to the variables required for
analysis. Observations from 1999–2000 support lag construction
for the analysis period beginning in 2001.

### U.S. Foreign Assistance Dataset

- Providers: U.S. Agency for International Development and
  U.S. Department of State.
- Repository file: `data/raw/foreign_assistance_raw.csv`.
- Reported dimensions: 189,242 rows × 11 columns.
- Reported coverage: 2001–2024.
- Download date: September 24, 2026.
- Status: Unchanged from the downloaded export.

During cleaning, I restrict records to:
- `Transaction Type Name` = `Obligations`.
- `US Category Name` = `Democracy, Human Rights, and Governance`.

### Processed Analysis Panel

- Repository file: `data/processed/merged_vdem_aid_panel`.
- Cleaning notebook: `notebooks/01_data_cleaning.ipynb`.
- Analysis notebook: `notebooks/02_eda_visualizations.ipynb`.
- Analysis window: 2001–2024.
- Intended key: Country identifier and year.
- Dimensions: 4936 rows × 13 columns.

## 3. V-Dem Source Variables

| Variable | Type | Definition | Scale / role |
|---|---|---|---|
| `country_name` | Text | Country name supplied by V-Dem. | Human-readable identifier. |
| `country_text_id` | Text | Country identifier supplied by V-Dem. | Country linkage key; verify alignment with aid country codes. |
| `year` | Integer | Calendar year of the V-Dem observation. | Time identifier. |
| `v2x_polyarchy` | Numeric | Electoral Democracy Index, covering electoral competition, elected officials, suffrage, clean elections, freedom of association, and expression. | 0–1; higher values indicate greater electoral democracy. |
| `v2x_libdem` | Numeric | Liberal Democracy Index, combining electoral democracy with liberal protections and constraints on executive power. | 0–1; higher values indicate greater liberal democracy. |
| `v2x_cspart` | Numeric | Civil Society Participation Index, covering participation, consultation, women’s participation, and aspects of candidate selection. | 0–1; higher values indicate greater civil society participation. |
| `v2x_regime` | Categorical code | Regimes of the World classification. | 0 = closed autocracy; 1 = electoral autocracy; 2 = electoral democracy; 3 = liberal democracy. Confirm inclusion in the committed input. |

## 4. Foreign Assistance Source Variables Used

| Variable | Type | Definition | Units / role |
|---|---|---|---|
| `Country Name` | Text | Recipient name supplied by the aid dataset. | Human-readable identifier. |
| `Country Code` | Text | Recipient country code supplied by the aid dataset. | Country linkage key. |
| `Fiscal Year` | Integer | U.S. fiscal year associated with the transaction. | October 1 of the preceding calendar year through September 30 of the labeled year. |
| `US Category Name` | Text | Broad assistance category. | Filtered to DRG. |
| `Transaction Type Name` | Text | Funding phase or transaction classification. | Filtered to Obligations. |
| `Constant Amount` | Numeric | Inflation-adjusted transaction amount. | Constant U.S. dollars; base year should be documented from the export metadata. |

Obligations represent legally binding commitments, not necessarily
cash disbursements within the same year. Negative amounts may reflect
adjustments to prior commitments; their specific causes are not
verified in this analysis.

## 5. Derived Variables

The names below must match the actual notebook and processed file.
Rows using naming patterns describe families of variables.

| Variable / naming pattern | Definition or calculation | Interpretation |
|---|---|---|
| `aid_millions` | Sum of DRG obligation `Constant Amount` by country and fiscal year, divided by 1,000,000. | Inflation-adjusted obligations in millions of U.S. dollars. |
| `log_aid` | `np.log10(aid_millions + 1)`. | Base-10 logarithm of aid in millions plus 1; used in within-country analysis. |
| `v2x_polyarchy_lag1`, `v2x_libdem_lag1`, `v2x_cspart_lag1` | Previous observation’s index value within country after sorting by year. | One-year lag only when consecutive observations represent consecutive years. |
| `v2x_regime_lag1` | Previous observation’s `v2x_regime` value within country. | Regime category used to group subsequent aid allocations. |

## 6. Cleaning and Missing-Value Decisions

- I fill three missing Bahrain liberal-democracy scores using
  forward fill followed by backward fill, assuming short-term continuity.
- I exclude seven small legacy Czechoslovakia records lacking
  country codes because they do not align with V-Dem identifiers.
- I replace missing aid values after merging with zero, assuming
  absent records represent no obligations rather than unreported funding. This assumption may misclassify unreported funding.
- I match country codes and year labels through an outer merge.
- Lag construction creates missing values at the beginning of
  each country’s series.

## 8. Interpretation and Limitations

- Democracy-index values summarize different dimensions and are
  not interchangeable or summable measures.
- Lagged variables support analysis of delayed associations but
  do not establish causal direction.
- Demeaning removes country-specific averages, not all potential
  confounding.
- Country-year comparisons give each observation equal weight.
- The analysis identifies associations rather than causal effects.

## 9. Source References

Coppedge, Michael, et al. 2026. “V-Dem Country-Year Dataset v16.”
Varieties of Democracy (V-Dem) Project.
https://doi.org/10.23696/vdemds26.

U.S. Agency for International Development and U.S. Department of State.
ForeignAssistance.gov. Downloaded September 24, 2026.
https://www.foreignassistance.gov/.
