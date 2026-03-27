# Assignment 7: Integrated Spatial Analysis

## Overview
This project integrates spatial data layers (soil health metrics and NDVI vegetation index) to create a comprehensive field analysis dashboard.

## Data Layers

### 1. Field Boundaries
- **File**: `example_fields_EPSG4326.geojson`
- **CRS**: EPSG:4326 (WGS84)
- Contains 3 fields with crop type and area information

### 2. NDVI Raster
- **File**: `sentinel2_field_001_20240615_NDVI_EPSG4326.tif`
- **Source**: Sentinel-2 satellite imagery
- **Date**: June 15, 2024
- **Resolution**: 10m

### 3. Soil Data
- **File**: `soil_iowa_EPSG4326.csv`
- Contains soil health metrics for each field:
  - Soil series
  - Clay, silt, sand percentage
  - Organic Matter (OM%)
  - pH
  - AWC (Available Water Content)
  - Ksat (Hydraulic conductivity)
  - Drainage class

## Processing Steps

1. **CRS Verification**: All layers verified to be in EPSG:4326
2. **NDVI Extraction**: Mean NDVI calculated per field using `rasterstats`
3. **Spatial Join**: Soil health metrics attached to fields based on location
4. **Visualization**: Multi-layer maps created with overlay effects

## Data Summary

| Field | Crop | Mean NDVI | Soil Type | pH | OM% | Clay% | Drainage |
|-------|------|-----------|-----------|-----|-----|-------|----------|
| field_001 | corn | 0.2951 | Marshall silty loam | 6.5 | 3.2 | 18 | Well drained |
| field_002 | soybeans | 0.2998 | Clarion loam | 6.2 | 2.8 | 25 | Moderately well drained |
| field_003 | corn | 0.293 | Nicollet clay loam | 6.8 | 3.5 | 28 | Well drained |

## Visualizations

### 1. Combined Field Map
- **File**: `Data/combined_field_map.png`
- Shows soil types with distinct colors + NDVI overlay

### 2. User Dashboard
- **File**: `Data/field_dashboard.png`
- **Output**: `output/dashboard_asset/integrated_spatial_analysis.png`
- Includes map + info panels for non-technical users

## Key Findings

- All fields have similar NDVI values (0.29-0.30), indicating moderate vegetation health
- Soil types vary across fields (Marshall, Clarion, Nicollet)
- pH range: 6.2-6.8 (optimal for crop growth)
- Organic Matter: 2.8-3.5% (good levels)
- All fields have well to moderately well drainage

## Files Structure

```
Assignment-07-Integrated-spatial-analysis/
├── project_tracker.md              # Project documentation
├── Data/
│   ├── example_fields_EPSG4326.geojson    # Integrated field data
│   ├── sentinel2_field_001_20240615_NDVI_EPSG4326.tif
│   ├── soil_EPSG4326.csv
│   ├── soil_iowa_EPSG4326.csv
│   ├── combined_field_map.png
│   └── field_dashboard.png
├── notebook/
│   ├── 07_spatial_integration.ipynb
│   └── field_dashboard.png
└── output/
    └── dashboard_asset/
        └── integrated_spatial_analysis.png
```

## Tools Used

- **Python**: pandas, geopandas, rasterio, rasterstats, matplotlib
- **Data Sources**: USDA Soil Data Access, Sentinel-2

## License
MIT License