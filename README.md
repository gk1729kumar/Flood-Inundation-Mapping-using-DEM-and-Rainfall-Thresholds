
# Flood Inundation Mapping using DEM and Rainfall

## 📌 Project Overview

This project aims to identify **flood-prone zones in the Kosi Basin, Bihar (India)** by integrating multiple geospatial datasets. The analysis combines terrain, rainfall, and satellite observations to improve disaster preparedness and risk assessment.

The study was conducted as part of the **Summer Training Programme on Remote Sensing and GIS (2025)** at the *India Space Academy (ISA)*.


## 🎯 Objectives

* Utilize **Digital Elevation Model (DEM)** for terrain and hydrological analysis.
* Integrate **CHIRPS Rainfall Data** to assess precipitation impacts.
* Validate flood-prone zones using **Sentinel-1 SAR imagery**.
* Develop flood risk classification maps in **QGIS** and validate them in **Google Earth Engine (GEE)**.


## 🗺 Study Area

* **Region**: Kosi Basin, Bihar, India
* **Districts Covered**: Supaul, Saharsa, Madhepura, Khagaria
* **Key Characteristics**:

  * Riverine floodplains
  * Low-lying agricultural lands
  * High monsoon rainfall


## 📂 Data Sources

| Dataset                 | Source                     | Resolution     | Time Period |
| ----------------------- | -------------------------- | -------------- | ----------- |
| **SRTM DEM (30m)**      | USGS Earth Explorer        | 30m            | Feb 2000    |
| **CHIRPS Rainfall**     | UCSB Climate Hazards Group | 0.05° (\~5 km) | Jul 2023    |
| **Sentinel-1 SAR (VV)** | Copernicus Open Hub        | 10m            | Jul 2023    |


## ⚙️ Methodology

### 🔹 QGIS Workflow

1. **DEM Processing**: Derived slope, flow direction (D8), and flow accumulation; applied fill sinks.
2. **Rainfall Overlay**: Masked regions >200 mm rainfall (CHIRPS).
3. **Flood Risk Classification**:

   * High Risk (Red) = low elevation + high rainfall
   * Medium Risk (Yellow) = moderate elevation + rainfall
   * Low Risk (Blue) = higher elevation

### 🔹 Google Earth Engine (GEE) Validation

1. **Sentinel-1 SAR Flood Detection**: VV polarization, threshold `VV < -15 dB`.
2. **Accuracy Assessment**: Compared QGIS flood map vs Sentinel-1 extent.

   * Overall Accuracy: **85%**
   * Kappa Coefficient: **0.78**


## 📊 Results

* **Flood-prone zones identified**:

  * High Risk: \~1200 km² (near riverbanks)
  * Medium Risk: \~800 km² (peripheral areas)
* **Validation**: 85% overlap between predicted and observed flood zones.

## 🚀 How to Run

### Requirements

* **QGIS (≥3.x)** with **SAGA GIS**
* **Google Earth Engine (Code Editor)**: [https://code.earthengine.google.com](https://code.earthengine.google.com)

### Steps

1. Clone this repository:

   ```bash
   git clone https://github.com/gk1729kumar/Flood-Inundation-Mapping-using-DEM-and-Rainfall-Thresholds.git
   cd Flood-Inundation-Mapping-using-DEM-and-Rainfall-Thresholds
   ```
2. Open QGIS and load the DEM and rainfall layers. Follow the methodology in the report.
3. Upload results to Google Earth Engine for validation. Example GEE snippet:

   ```js
   var floodRiskMap = ee.Image("users/your_username/flood_risk");
   var sentinel1 = ee.ImageCollection('COPERNICUS/S1_GRD')
       .filterBounds(kosiBasin)
       .filterDate('2023-07-01', '2023-07-31')
       .filter(ee.Filter.eq('transmitterReceiverPolarisation', ['VV']))
       .first();
   var water = sentinel1.select('VV').lt(-15);
   Map.addLayer(water.selfMask(), {palette: ['blue']}, 'Sentinel-1 Water Extent');
   ```


## 📌 Future Work

* Use higher-resolution DEMs (ALOS, LiDAR).
* Integrate **land use/land cover (LULC)** data.
* Automate flood mapping workflows with Python + GEE API.

## 📖 References

1. [USGS Earth Explorer – SRTM DEM](https://earthexplorer.usgs.gov/)
2. [Copernicus Open Hub – Sentinel-1 SAR](https://scihub.copernicus.eu/)
3. [CHIRPS Rainfall Data – UCSB](https://www.chc.ucsb.edu/data/chirps)

✍️ **Author**: Gautam Kumar
🎓 *B.Tech Mechanical Engineering, IITRAM Ahmedabad*
📅 August 2025
