- [x] Downloaded field boundaries
- [x] Downloaded CDL crop data
- [x] Merged datasets using field_id
- [x] Created interactive web map

* Field boundaries file: `fields-EPSG4326.geojson`
* Crop data file: `cdl_EPSG4326.csv`
* Soil data file: `soil_EPSG4326.csv`
* Merged dataset: `field_with_crop.geojson` and `field_summary.csv`
* Interactive map: `my_fields_map.html`

### Key Takeaways From EDA

1. Soil Organic Matter vs Crop Productivity Index
* Higher soil organic matter (~3.5%) generally corresponds with higher crop productivity index values.
* The plot suggests that higher Soil Organic Matter levels tend to be associated with higher NCCPI values. Data points with Organic Matter around 3.5% generally show productivity values between 0.62 and 0.79, which are relatively higher compared to points with Organic Matter around 2.8%
* Productivity varies even at the same organic matter levels, suggesting other factors such as drainage conditions may influence yield.
* A potential outlier appears at 2 fields : 
  - Field 1 - it has higher organic matter ~3.2% but low productivity (NCCPI ~0.59)
  - Field 4 - it has lower organic matter ~2.8% with relatively low productivity (NCCPI ~0.58)
  - both fileds are Kennebec silt loam with Moderate well drained 

2. Crop Coverage Over Time by Commodity
- Corn dominates crop coverage across all years, peaking in 2021 before gradually declining in 2022 and 2023.
- Soybeans show strong variability, dropping to zero in 2022 and increasing again in 2023, suggesting crop rotation practices.
- Other crops such as Alfalfa, Fallow, and Winter Wheat maintain relatively small and stable coverage over time.

Data Cleaning Steps

* Identified potential outliers in the NCCPI values, particularly the point with NCCPI around 0.58 at organic matter ~2.8%.
* Used statistical methods such as the IQR approach to detect extreme values.
* Verified whether the outlier represents a true observation or a potential measurement anomaly before deciding whether to remove or adjust it.
* the result:
  1. Field 1 (Zook soil - outlier): The low NCCPI (0.59) is justified by soil properties:
        - Poorly drained (vs Moderately well drained)
        - Hydrologic Group D (very slow runoff)
        - Very low ksat (1.0) - poor permeability
        - Higher clay (35% vs 18%)
        - Water table present in winter (wtdep_jan_cm = 6)
  2. Field 4 (Kennebec soil - more suspicious):
        - Same soil type as Field 2 (Kennebec silt loam)
        - Same OM (2.8%)
        - Same drainage class (Moderately well drained)
        - But different NCCPI: Field 4 = 0.58, Field 2 = 0.77


### Data Alignment and CRS Challenges

* The Map layers use EPSG:4326 (WGS84 lat/lon). The soil data lat/lon coordinates (42.91-42.94, -95.39 to -95.33) align with the field boundaries in the GeoJSON.
* The challenge lies in ensuring that every command used to generate the map aligns with what i want to show in the dashboard view. for example the legend's placement, what data you want to show. in this case i have included the size of field andthe river locations. these information will make it easier to understand about drainage problems. 


### Weather and Climate Trend Analysis

Anomalies that i found from the data is:
* During May–September 2024, where maximum temperature reached an extreme of around 39°C in weeks 35–36, indicating a heatwave, while minimum temperature dropped unusually to about 9–10°C in weeks 36–37; additionally, sharp daily temperature fluctuations are observed. 
*On the precipitation side, there are extreme spikes up to ~45 mm in week 21, along with other high rainfall events (~30 mm in week 27 and ~25–26 mm in week 35), interspersed with prolonged dry periods during weeks 22–24 and 32–34. Overall, the combination of high temperature and low precipitation in weeks 35–36 suggests potential heat stress and drought conditions, while the spike-based rainfall pattern indicates uneven and localized precipitation distribution.

---

## Assignment 7: Spatial Integration

### Tasks Completed

- [x] Created notebook structure: `07_spatial_integration.ipynb`
- [x] Copied soil data and NDVI raster to `Data/` folder
- [x] Verified all layers are in same CRS (EPSG:4326)
- [x] Calculated mean NDVI for each field using rasterstats
- [x] Added `mean_ndvi` column to fields GeoJSON
- [x] Created matching Iowa soil data for field locations
- [x] Performed spatial join to attach soil health metrics to fields
- [x] Created individual field maps with multiple layers
- [x] Created combined overlay map with:
  - Soil type layer (60% opacity) - distinct colors
  - NDVI heatmap (35% opacity) - RdYlGn colormap
  - Bold black field borders
  - Labels positioned above/below fields
- [x] Created user-friendly dashboard visualization
- [x] Added info panels for non-technical users
- [x] Saved final dashboard to `output/dashboard_asset/integrated_spatial_analysis.png`

### Data Summary

| Field | Crop | Mean NDVI | Soil Type | pH | OM% | Clay% | Drainage |
|-------|------|-----------|-----------|-----|-----|-------|----------|
| field_001 | corn | 0.2951 | Marshall silty loam | 6.5 | 3.2 | 18 | Well drained |
| field_002 | soybeans | 0.2998 | Clarion loam | 6.2 | 2.8 | 25 | Moderately well drained |
| field_003 | corn | 0.293 | Nicollet clay loam | 6.8 | 3.5 | 28 | Well drained |

### Output Files

- `Data/example_fields_EPSG4326.geojson` - Fields with all integrated data
- `Data/combined_field_map.png` - Combined overlay map
- `Data/field_dashboard.png` - User-friendly dashboard
- `notebook/field_dashboard.png` - Dashboard in notebook
- `output/dashboard_asset/integrated_spatial_analysis.png` - Final output