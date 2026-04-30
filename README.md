# Road safety analysis in Tallinn (2021-2024)

**University:** University of Tartu

**Author:** Mihhail Batura

**Supervisors:** Prof. Evelyn Uuemaa, PhD Raivo Aunap 

## Background

Let's assume the Department of Transport has received a request to analyze problem areas of the city's road network. The focus should be on areas where drivers most frequently violate speed limits and where pedestrian accidents are most common, with a particular focus on areas near schools and kindergartens. While the client has data on violations and school locations, they lack a comprehensive overview of Tallinn's most dangerous roads—a gap this analysis aims to fill.

## Purpose

The analysis aims to identify road segments in Tallinn where accidents occur most frequently, considering both total accident counts and pedestrian-involved incidents. It seeks to provide a general overview and rank the most dangerous sections of the city's road network. Additionally, the study investigates whether a statistically significant relationship exists between pedestrian accidents and environmental conditions, specifically road surface state and weather.

## Research Questions

1. Which road segments in Tallinn have the highest number of traffic accidents, both total and pedestrian-related?
2. Is there a statistically significant relationship between pedestrian accidents and road surface condition?
3. Is there a statistically significant relationship between pedestrian accidents and weather conditions?
4. Which specific conditions (road surface type / weather type) pose the highest risk for pedestrians?
5. Which factor has a stronger influence on pedestrian accident risk — road condition or weather?

## Data Sources

- **Traffic accidents (point):** Estonian Transport Administration (Transpordiamet, 2025).  
  Tallinn traffic accident data (2021–2024) – `data/road_accs_tln_21_24.csv`  
  (https://avaandmed.eesti.ee/datasets/inimkannatanutega-liiklusonnetuste-andmed)

- **Road network (line):** Estonian Road Administration (Teeregister), WFS services.  
  `data/tee.gpkg`

- **Settlement units (polygon); Municipalities (polygon):** Estonian Land and spatial development board (Maa- ja Ruumiamet).  
  Administrative and settlement division. (https://geoportaal.maaamet.ee/est/ruumiandmed/haldus-ja-asustusjaotus-p119.html)

## Workflow

**Data preparation (Python) – `code/road_accs_prep.ipynb`**
- Filters raw CSV data for Tallinn (2021–2024)
- Cleans and renames columns
- Converts coordinates to geometry (EPSG:3301)
- Exports to Shapefile and GeoPackage

**Hexagonal grid & heatmap (QGIS)**
- Creates hexagon grid (100×100 m) over Tallinn boundary
- Counts accidents per hexagon (total and pedestrian-only)
- Generates kernel density heatmap (500 m radius)

**Road buffer & spatial join (QGIS)**
- Creates 10 m buffer around road network for heatmap and 15 m for hexagonal grid
- Counts accidents within buffers
- Joins accident counts back to road segments

**Statistical analysis (Python) – `code/stat_analysis_pedestrian.ipynb`**
- Constructs 2×2 contingency tables for road condition and weather
- Calculates Odds Ratio, Chi-square, and Cramér's V
- Analyzes pedestrian accident share by specific road/weather types
- Visualizes results with bar charts

**Final maps (QGIS)**
- Heatmap for decision-makers (continuous risk zones)
- Hexagonal grid with accident counts (discrete values)
- Hexagonal grid with  pedestrian involved accident counts (discrete values)

## Key results

*   The most problematic area is Tallinn's city center, which is generally expected. Other problematic areas include **Tammsaare Street and Akademi Street in Mustamäe, Telliskivi and Tehnika Streets in Kristiine, and the Mustakivi Bridge in Lasnamäe**.
*   In terms of pedestrian accidents, it's worth paying attention to **Sõle Street in Põhja-Tallinn, Akademi Street in Mustamäe, and Tartu Street in Kesklinn** – many pedestrian accidents have been recorded there.
*   **Pedestrian accident risk is 2-3 times higher on roads with poor surface conditions** (snow, ice, wetness, dirt) compared to dry roads (OR = 2.48, p < 0.001).
*   **Loose snow is the most dangerous road surface:** 57.7% of accidents on such roads involve pedestrians.
*   **Adverse weather increases pedestrian accident risk by 1.5-2 times** (OR = 1.75, p < 0.001), with snowfall being the most hazardous condition (40.4% pedestrian share).
*   **Road condition has a stronger impact on pedestrian safety than weather conditions.**
