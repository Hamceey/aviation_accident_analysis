# Jet Airline Safety: Aviation Accident Analysis

## Project overview
An airline/airplane insurer wants to know which airplane makes and models show a low rate of total destruction and a low likelihood of fatal or serious injuries when an accident happens, and which conditions influence the outcome. The analysis uses the NTSB aviation accident data (1948-2022) and is restricted to **professionally built airplanes involved in events from 1983 onward** (assuming a maximum 40-year aircraft life). Results are reported separately for **small airplanes** and **large passenger airplanes** (threshold of 20 people on board).

## Repository contents
| File | Description |
|---|---|
| `aviation_cleaning.ipynb` | Loads the raw data, filters, cleans and builds the measures, then saves `cleaned_aviation_data.csv` |
| `aviation_analysis.ipynb` | EDA, make/model comparisons, other factors and recommendations |
| `cleaned_aviation_data.csv` | Output of the cleaning notebook (input of the analysis notebook) |
| `images/` | Key visualizations used below |

The raw file `AviationData.csv` (from the Kaggle dataset) must be placed next to the cleaning notebook to re-run it.

## Data cleaning and measures (summary)
- Kept events from 1983 onward with `Amateur.Built == 'No'` and `Aircraft.Category == 'Airplane'`. Missing categories were imputed from the most common category of the same make/model (then the same make).
- **Fatal/serious injury fraction** = (fatal + serious injuries) / people on board, where people on board is the sum of the four injury columns (missing counts assumed 0, events with no occupants dropped).
- **Destroyed** = 1 if `Aircraft.damage == 'Destroyed'`.
- Make names standardized (upper case, merged variants such as `CESSNA AIRCRAFT CO`, `HAWKER BEECHCRAFT`, `AIRBUS INDUSTRIE`), keeping makes with at least 50 events.
- Unique plane type = `Make.Model`. Weather, engine type, purpose and phase of flight labels standardized.
- Final dataset: 59,228 accident records.
- Size group: a type is **Large** if the 95th percentile of people on board in its accidents is at least 20.

## Key findings

### Makes
![Mean fatal/serious fraction by make](images/mean_injury_fraction_by_make.png)

- **Small airplanes:** Maule is the best-supported low-risk make (569 events, 0.153 fatal/serious fraction, 9.3% destroyed). Aviat and Diamond have the lowest destruction rates; Waco, Helio, Great Lakes and Stinson have low injury fractions but smaller samples.
- **Large airplanes:** McDonnell Douglas (0.051), Bombardier (0.075), Boeing (0.105, the largest sample with 852 events) and Embraer (0.111) have the lowest fatal/serious fractions.

![Destroyed rate by make](images/destroyed_rate_by_make.png)

### Plane types
![Large airplane types](images/large_models_mean.png)
![Small airplane types](images/small_models_mean.png)

- Large: Boeing 777, 757 variants, 727-200 and 747-400, the MD-80 family, MD-11F and the Bombardier CL-600-2B19.
- Small: the Maule M-4 / M-7 / MX-7 family, Stinson 108-1, Great Lakes 2T-1A-2.

### Factors affecting severity
![Weather condition](images/weather_condition.png)
![Phase of flight](images/phase_of_flight.png)

- **Weather:** accidents in IMC have a 0.669 mean fatal/serious fraction vs 0.221 in VMC, and the aircraft is destroyed in 58.6% vs 16.7% of accidents.
- **Phase of flight:** maneuvering (0.533), climb (0.420) and cruise (0.352) are the most severe phases; landing (0.034) and taxi (0.016) are the least severe.
- **Number of engines:** twin-engine accidents are more severe than single-engine accidents (0.325 vs 0.251), likely a reflection of aircraft type and mission rather than a direct effect.

## Recommendations
- **Small aircraft:** Maule (M-4 / M-7 / MX-7 family), then Aviat, Diamond (low destruction) and, with lower confidence, Waco, Helio, Great Lakes and Stinson 108-1.
- **Large aircraft:** Boeing (777, 757, 727-200, 747-400), McDonnell Douglas (MD-80 series, MD-11F), Bombardier CL-600-2B19 and Embraer.
- **Conditions:** price in the much higher severity of IMC accidents and of maneuvering/climb/cruise events.

## Limitations
- Only accidents/incidents are recorded, with no exposure (flight hours), so the analysis describes severity conditional on an accident, not accident likelihood.
- Many model-level samples are small (10-40 events) and confidence intervals overlap.
- Size is inferred from occupancy in accident records and may misclassify models with very few events.
- Results are associations, not causal effects.
