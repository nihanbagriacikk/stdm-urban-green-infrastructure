# stdm-urban-green-infrastructure

# DI722 - Spatio-Temporal Data Mining  
## Project Presentation  

# Urban Green Infrastructure Monitoring Using Sentinel-1 and Sentinel-2 Data  

---

## 1. Introduction & Motivation

Urban green infrastructure (UGI) plays a fundamental role in sustaining urban ecological systems. It contributes to carbon sequestration, microclimate regulation, stormwater management, and overall environmental resilience. In rapidly urbanizing environments, monitoring the spatial and temporal dynamics of green areas is essential for sustainable planning and decision-making.

However, conventional approaches for mapping urban green areas rely heavily on static datasets such as cadastral maps, land-use plans, and field surveys. These approaches are limited in their ability to capture temporal variability and often fail to represent the structural characteristics of vegetation.

Recent advances in remote sensing technologies provide a powerful alternative. Satellite imagery enables continuous, large-scale, and repeatable monitoring of urban environments. In particular, Sentinel-2 optical imagery provides spectral information related to vegetation health, while Sentinel-1 Synthetic Aperture Radar (SAR) data provides structural and moisture-related information independent of cloud cover.

This project is motivated by the need to explore whether integrating Sentinel-1 and Sentinel-2 data can improve the detection and characterization of urban green infrastructure compared to traditional NDVI-based approaches.

---

## 2. Research Question

Can the integration of Sentinel-1 SAR and Sentinel-2 optical data improve the detection and characterization of urban green infrastructure compared to single-source NDVI-based approaches?

---

## 3. Dataset

The project is based on multi-source remote sensing data combined with auxiliary spatial datasets.

### 3.1 Sentinel-2 Optical Imagery

Sentinel-2 provides high-resolution multispectral imagery, including visible, near-infrared, and red-edge bands.

Key bands used:
- B04 (Red)
- B08 (Near Infrared)
- Optional: Red-edge bands

These bands are used to derive vegetation indices such as NDVI, which represent vegetation health and density.

---

### 3.2 Sentinel-1 SAR Imagery

Sentinel-1 provides radar backscatter information, which is particularly useful because it is not affected by cloud cover.

Key features:
- VV backscatter
- VH backscatter
- VV/VH ratio

SAR data provides information about vegetation structure and surface roughness.

---

### 3.3 Auxiliary Data

Additional datasets may include:
- OpenStreetMap (OSM) green areas
- Study area boundary
- Existing land-use data

These datasets are used for contextual information and validation.

---

## 4. Task Definition

The problem is formulated as a spatial data mining task.

### Inputs:
- NDVI
- VV
- VH
- VV/VH ratio

### Outputs:
- Green / non-green classification
- Vegetation clusters
- Spatial patterns of urban green infrastructure

The task can be approached using both supervised and unsupervised methods depending on data availability.

---

## 5. Baseline Method

The baseline method is NDVI thresholding.

```python
if NDVI > 0.3:
    class = "vegetation"
else:
    class = "non-vegetation"
```

This method is simple, interpretable, and widely used in remote sensing.

However, it has several limitations:
- Sensitive to cloud cover
- Affected by atmospheric conditions
- Cannot capture vegetation structure
- Limited in complex urban environments

Therefore, the baseline serves as a reference for evaluating more advanced approaches.

---

## 6. Methodology

The project follows a structured data mining workflow:

```
Raw Data
   ↓
Preprocessing
   ↓
Feature Extraction
   ↓
Baseline (NDVI)
   ↓
Machine Learning
   ↓
Results
```

---

### 6.1 Data Collection

Sentinel-1 and Sentinel-2 images are collected for the study area.

---

### 6.2 Preprocessing

- CRS alignment
- Spatial clipping
- Data cleaning

---

### 6.3 Feature Extraction

Extracted features:
- NDVI
- VV
- VH
- VV/VH ratio

These features are transformed into a structured dataset.

---

### 6.4 Baseline Model

NDVI thresholding is applied.

---

### 6.5 Advanced Model

K-Means clustering is used as a data mining method.

---

## 7. Example Python Workflow

```python
import rasterio
import numpy as np
import pandas as pd
from sklearn.cluster import KMeans
import warnings

warnings.filterwarnings("ignore")

# NDVI calculation
ndvi = (nir - red) / (nir + red + 0.0001)

# SAR ratio
ratio = vv / (vh + 0.0001)

# Feature table
features = pd.DataFrame({
    "NDVI": ndvi.flatten(),
    "VV": vv.flatten(),
    "VH": vh.flatten(),
    "Ratio": ratio.flatten()
})

features = features.replace([np.inf, -np.inf], np.nan)
features = features.dropna()

# Baseline classification
features["NDVI_Baseline"] = np.where(features["NDVI"] > 0.3, 1, 0)

# Clustering
kmeans = KMeans(n_clusters=3, random_state=42)
features["Cluster"] = kmeans.fit_predict(features)

# Save
features.to_csv("results.csv", index=False)
```

---

## 8. Literature Review

A key study supporting this project is conducted by Xiao et al. (2022), which investigates the integration of Sentinel-1 and Sentinel-2 data for urban green space classification and above-ground biomass estimation.

The study applies machine learning techniques, particularly Random Forest, to classify green areas and estimate biomass.

Key findings:
- Multi-source data improves classification accuracy
- Combined models outperform single-source models
- Optical data captures spectral information
- SAR data captures structural information

Reported performance:
- Classification accuracy ≈ 86%
- R² ≈ 0.75–0.77

These findings strongly support the use of multi-sensor data fusion.

---

## 9. H3 / DGGS Investigation

H3 is a hexagonal spatial indexing system.

```python
import h3

h3_index = h3.geo_to_h3(lat, lon, resolution=9)
```

Advantages:
- Equal-distance neighbors
- Reduced spatial bias
- Better aggregation

H3 can be used in advanced stages for spatial modeling.

---

## 10. Preliminary Results

Expected outputs:
- NDVI map
- Green / non-green classification
- Cluster map

NDVI provides a basic approximation, but SAR integration is expected to improve results.

---

## 11. Expected Contribution

- Multi-sensor data fusion approach
- Improved vegetation detection
- Application of data mining techniques to remote sensing

---

## 12. Conclusion

This project integrates Sentinel-1 and Sentinel-2 data within a data mining framework.

It moves beyond simple NDVI-based mapping and applies clustering techniques to better understand urban green infrastructure patterns.

---

## 13. References

Xiao, J. et al. (2022). Identification of Urban Green Space Types and Estimation of Above-Ground Biomass Using Sentinel-1 and Sentinel-2 Data.

Copernicus Sentinel Data  
OpenStreetMap  
DI722 Course Materials  



