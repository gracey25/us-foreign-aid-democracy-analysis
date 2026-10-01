### Raw Data Sources & Version Documentation

1. **V-Dem Governance Dataset (Subset)**
   - **Source:** V-Dem Institute (Varieties of Democracy, v16 Release)
   - **File Path:** `data/raw/vdem_1999_2023_subset.csv`
   - **Dimensions:** 4,455 rows × 6 columns
   - **In-Memory Size:** ~0.60 MB
   - **Timeframe:** 1999-2023
   - **Variables Retained:** `country_name`, `country_text_id`, `year`, `v2x_polyarchy`, `v2x_libdem`, `v2x_cspart`

2. **U.S. Foreign Assistance Dataset**
   - **Source:** ForeignAssistance.gov (U.S. Agency for International Development / Department of State)
   - **File Path:** `data/raw/foreign_assistance_raw.csv`
   - **Dimensions:** 189,242 rows × 11 columns
   - **In-Memory Size:** ~66.15 MB
   - **Timeframe:** 2001–2024
   - **Filter Criteria:** Transaction Type = Obligations; US Category Name = Democracy, Human Rights, and Governance (DRG)
