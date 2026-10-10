# Aviation Accident Analysis: Which Aircraft Are Safest to Insure?

**Author:** Ian Wakali ([@Wakali-Ian](https://github.com/Wakali-Ian))

## Business problem
An airline/aircraft insurer wants to know which airplane **makes and models** have a low rate of total destruction and a low likelihood of fatal or serious occupant injury **when an accident happens**, and which general conditions drive those outcomes. The client wants **separate recommendations for small aircraft and larger passenger aircraft**, and only for professionally built airplanes that could still be flying (events from **1983** onward, assuming a 40-year life).

## Data
NTSB aviation accident records (1948-2022, 88,889 rows) in `data/AviationData.csv` ([Kaggle source](https://www.kaggle.com/datasets/khsamaha/aviation-accident-database-synopses)). After cleaning: **59,990 accidents, 56 makes, ~4,700 plane types** (`data/cleaned_aviation_data.csv`).

## Repository structure
```
.
├── Aviation_Accidents_Cleaning.ipynb        # Part 1: load, inspect, clean, derive metrics, save csv
├── Aviation_Accidents_Data_Analysis.ipynb   # Part 2: EDA, recommendations, other factors
├── data/
│   ├── AviationData.csv                     # raw data
│   ├── USState_Codes.csv
│   └── cleaned_aviation_data.csv            # output of notebook 1
├── images/                                  # figures used below
└── README.md
```

## Method (short)
- **Filters:** airplanes only (missing `Aircraft.Category` imputed from make/model, then make, then engine type); `Amateur.Built == No`; `Event.Date >= 1983`.
- **Metrics:** `Fatal.Serious.Fraction = (fatal + serious) / total occupants`; `Destroyed = 1` if damage is "Destroyed".
- **Cleaning:** standardised and merged make names, kept makes with >= 50 records, `Plane.Type = Make + Model` (model labels are not unique across makes), placeholders (`Unknown`, `UNK`) turned into NaN, rare categories grouped/removed, columns with <= 20,000 non-nulls dropped.
- **Small vs large:** a plane type is *large* if >= 25% of its accidents had 20+ people aboard (threshold of 20 applied per plane type, so one data error cannot turn a Piper into a jet).
- **Statistical robustness:** minimum 10 accidents per group (30 in the robustness check), 95% confidence intervals on every bar, chi-square / Mann-Whitney / Kruskal-Wallis tests for factors.

## Key findings
Baseline per accident: **small** aircraft 27.5% of occupants fatally/seriously injured and 21.4% destroyed; **large** aircraft 10.7% and 11.8%.

### Recommended makes
| | Recommended (well supported) | Watch (small samples) |
|---|---|---|
| Small | **Maule** (n=571, 15.4% injured, 9.3% destroyed), **Aviat**, **Waco**, **Helio**, **Diamond** | Bombardier small models (n=19) |
| Large | **McDonnell Douglas** (n=228, 3.6% / 7.0%), **Boeing** (n=852, 10.5% / 11.2%) | Bombardier, Embraer, Canadair |

![Makes mean fraction](images/makes_mean_fraction.png)
![Makes destroyed](images/makes_destroyed.png)

### Recommended plane types
- **Large:** Boeing 777, Boeing 757, McDonnell Douglas MD-88 / MD-80 / DC-9-31, Bombardier CL-600-2B19, Boeing 727-200; the Boeing 737 is solid and best measured (n=121, 13.4%).
- **Small (>= 30 accidents):** Piper PA-18-160 and PA-20, Cessna 180A and 185 family, Beech C-23.

![Large types](images/large_types_mean.png)
![Small types](images/small_types.png)

### Factors that drive outcomes
1. **Weather:** an IMC accident injures 68% (small) / 22% (large) of occupants versus 23% / 6% in VMC, and destroys the aircraft 60% / 25% vs 17% / 6% (p < 1e-15).
![Weather](images/weather.png)
2. **Number of engines:** twin-engine light aircraft do *worse* (35% injured, 32% destroyed) than single-engine (25%, 19%); for large jets engine count makes no significant difference.
![Engines](images/engines.png)
3. **Phase of flight (bonus):** taxi and landing accidents are mostly minor (< 4% injured), maneuvering (53%), climb (41%) and cruise (35%) are the deadliest.
![Phase](images/phase.png)

## Limitations
No flight-hour (exposure) data, so figures are *risk given an accident*, not accident rates; occupancy is a proxy for aircraft size; some model spellings remain unmerged; associations are not controlled for pilot experience or operation type.

## How to run
```bash
pip install pandas numpy matplotlib seaborn scipy jupyter
jupyter notebook   # run the Cleaning notebook first, then the Data Analysis notebook
```
