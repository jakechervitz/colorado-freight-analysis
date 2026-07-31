# Colorado Freight Flow Analysis, 2018–2024

**What's driving the decline in Colorado's freight tonnage? An analysis of commodity and mode shifts.**

Colorado's outbound freight tonnage fell **15.8%** between 2018 and 2024. This project uses the federal Freight Analysis Framework (FAF5.7.1) to identify what's behind the decline — and finds that **72% of the drop traces to energy commodities**, while the truck freight that most Colorado businesses depend on remained comparatively stable.

*Case study by Jake Chervitz | Python · pandas · matplotlib | [Portfolio](github.com/jakechervitz/colorado-freight-analysis) · [LinkedIn](www.linkedin.com/in/jake-chervitz-913b36b9)*

---

## Key findings

1. **The decline is concentrated in energy-linked modes.** Pipeline tonnage fell 26.7% and rail 18.8%, while truck — the majority of state tonnage — declined just 9.6%.
2. **One commodity group dominates the story.** Natural gas and other fossil products (SCTG 19) fell 37.2% — a loss of 34.2 million tons, six times larger than the next biggest decline. Coal fell 22.2%.
3. **It's two economies.** Energy commodities (SCTG 15–19) ended 2024 at an index of 76 (2018 = 100) versus 92 for all other freight, driving 72% of the total decline despite being a minority of volume.
4. **The fossil-fuel decline isn't uniform.** Crude petroleum freight *grew* 15.1% as DJ Basin volumes recovered from the 2020–21 trough — a fundamentally different outlook than coal and gas products. (EIA data show production roughly flat vs. 2018, suggesting shipment-pattern changes also contributed; see Validation.)
5. **Trucking's core is stable.** Gravel, mixed freight, and foodstuffs held comparatively steady; the headline decline overstates risk to truckload demand.

### Recommendations (for the fictional stakeholder, Rocky Mountain Freight Advisors)
- Reassess exposure to coal rail lanes and gas-product pipelines as structural, not cyclical, decline
- Treat Colorado truckload demand as stable despite headline tonnage figures
- Watch crude-related capacity as a selective growth pocket; monitor FAF6 benchmark (mid-2026) and 2024 data revisions

---

## Repository contents

| File | Description |
|---|---|
| `01_process_faf5_cleaning.ipynb` | Data cleaning pipeline: filters ~2M-row national file to Colorado domestic flows, decodes FAF5 codes, reshapes to tidy format, validates against FAF Data Tabulation Tool recon |
| `02_analyze_faf5_charts.ipynb` | Five-chart analysis with written findings, key statistics, and recommendations |
| `co_freight_tidy_2018_2024.csv` | Analysis-ready dataset (5.4 MB, output of notebook 01) |
| `charts/` | Exported chart PNGs |

**Note:** The raw FAF5.7.1 file (~663 MB) is not included in this repo due to size. Download `FAF5.7.1_2018-2024.zip` (CSV) from [bts.gov/faf](https://www.bts.gov/faf) and place it in the project root to reproduce notebook 01.

---

## Data source

**Freight Analysis Framework Version 5 (FAF5.7.1)**, Bureau of Transportation Statistics & Federal Highway Administration. Regional database, 2018–2024 annual estimates. Public domain US government data.

- Regional database download: [bts.gov/faf](https://www.bts.gov/faf)
- Data dictionary and SCTG commodity codes: FAF5_metadata workbook (included with the BTS download) and [faf.ornl.gov documentation](https://faf.ornl.gov/faf5/Documentation.aspx)

**Scope of extract:** All flows originating from FAF zones 081 (Denver-Aurora CO) and 089 (Rest of CO); domestic flows only (`trade_type == 1`); all modes; all 42 SCTG commodity groups; tons as primary measure, constant-2017-dollar value retained.

### ROCCC assessment

| Criterion | Assessment |
|---|---|
| **R**eliable | Official US DOT statistical product with publicly documented methodology ✔ |
| **O**riginal | Primary source (BTS/FHWA), downloaded directly — not a third-party rehost ✔ |
| **C**omprehensive | All modes, commodities, and regions; tonnage, value, and ton-miles ✔ |
| **C**urrent | v5.7.1 includes annual estimates through 2024 (2023–24 preliminary) ✔ |
| **C**ited | Fully citable government open data ✔ |

### Limitations & caveats

1. FAF flows are **modeled estimates** built from the Commodity Flow Survey and supplemental sources — not raw shipment records
2. **2024 (and to a lesser degree 2023) estimates are preliminary** and subject to revision
3. Dollar values are in **constant 2017 dollars**
4. Colorado is represented by only **two FAF zones**, limiting intra-state granularity (the experimental county-level FAF product is a future-analysis option)
5. Pipeline "shipments" represent **flow volumes**, which behave differently from discrete freight movements
6. The cereal grains decline (-25.2%) is a **secondary, non-energy finding** whose causes (possibly drought-related) lie outside this dataset's scope

---

## Methodology

**Process (notebook 01):** Loaded the national regional CSV with `usecols` to manage the ~2M-row / 663 MB file; filtered to Colorado origins and domestic trade; dropped foreign-trade, distance-band, nominal-value, and ton-mile columns; decoded mode, zone, and SCTG codes to labels with assertion checks for unmapped values; melted wide year columns to tidy long format; validated totals against independent extracts from the FAF Data Tabulation Tool (domestic-only totals came in appropriately below all-trade-type recon figures).

**Analyze (notebook 02):** Five charts mapped one-to-one to the guiding sub-questions — mode trends, absolute commodity change, energy vs. non-energy indexed comparison, energy commodity small multiples, and truck commodity stability — with findings written against verified statistics computed in-notebook.

**Cleaning changelog** is documented in notebook 01's header and inline decision notes.

### Validation

Mode-level trends were cross-checked against independent FAF Data Tabulation Tool extracts, with domestic-only totals coming in appropriately below all-trade-type recon figures.

**EIA cross-reference (crude petroleum):** FAF crude freight tonnage grew 15.1% from 2018 to 2024, while EIA's Colorado Field Production of Crude Oil series shows production roughly flat over the same span (~463 vs. ~465 thousand b/d annual average), following a 2019 peak, a 2020–21 trough, and a recovery to near-2018 levels. The divergence indicates the freight growth reflects shipment-pattern changes (e.g., more crude moving out of state versus in-state refining) and/or FAF modeling factors in addition to production levels — flagged as an open question for deeper analysis. Finding 4's language reflects this: crude volumes recovered from the 2020–21 trough rather than growing above the 2018 baseline.

---

## Future analysis

- Investigate the crude petroleum freight-vs-production divergence (see Validation) — e.g., destination-level analysis of where Colorado crude flows
- Refresh against the **FAF6 benchmark** (BTS release expected mid-2026) and revised 2024 estimates
- Investigate the cereal grains decline using USDA data
- Short-haul vs. long-haul truck split using the `dist_band` field
- County-level granularity via the experimental FAF county product
- Import/export flows (excluded here) as a trade-lane analysis

---

## Tools & skills demonstrated

- **Python / pandas:** large-file handling (663 MB, ~2M rows) with selective column loading, filtering, code mapping with validation assertions, wide-to-long reshaping
- **matplotlib:** five publication-styled charts with a consistent custom palette, annotations, and small multiples
- **Data sourcing & documentation:** primary-source government data (BTS/FHWA), ROCCC evaluation, documented cleaning changelog, cross-validation against independent extracts
- **Domain analysis:** freight mode economics, SCTG commodity structure, and Colorado energy-market context applied to interpretation and recommendations
