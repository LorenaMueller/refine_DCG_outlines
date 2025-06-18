# Automated Mapping of Debris-Covered Glaciers – Code and Data Repository

This repository provides a semi-automated pipeline for mapping debris-covered glaciers (DCGs) using freely available satellite data and machine learning techniques. The workflow leverages:

Google Earth Engine (GEE) to preprocess and export satellite-derived features.
Google Colab for loading these features, training an XGBoost classifier, and generating final glacier outlines.

Our approach aims to improve DCG mapping in complex terrain where traditional methods relying on manual delineation or high-resolution data are not scalable. The methodology is validated on glaciers in Switzerland, Italy, and India, and is designed with global applicability in mind.

Last update to this readme: 16 June 2025.

---

## 1. Repository structure

Here is the structure of this repo. Files have been excluded from this tree for readability.

```
├───code
│   ├───a_gee_preprocessing  
│   ├───b_colab_analysis
|   └───
│      
├───data
│   └───results
│       ├───figures
│       └───polygon_outlines
│
├───figures
│
└───tables
```

* The script in `gee_preprocessing` is a Google Earth Engine (GEE) script used to derive relevant spectral, thermal, radar, and topographic features from open satellite data (Sentinel-1, Landsat 8, Copernicus DEM). The processed image layers are exported to Google Drive.

* The script in `colab_analysis` is a Google Colab notebook that loads the exported data, trains an XGBoost classifier, evaluates the results, and generates final glacier outlines, including multi-year averaged maps.

* The folder `data` is empty by default and should contain the exported image stacks and reference glacier outlines. These can be obtained by running the GEE script or using links provided in the manuscript (see Zenodo DOI when available).

* The folders `figures` and `tables` are used to store outputs from the analysis, including classification maps, comparisons with glacier inventories, and performance metrics.

[back to content](#1-repository-structure)

---

## 2. Required data and software

### Data

The necessary satellite data is accessed and processed via Google Earth Engine using the script provided. Outputs are exported to Google Drive for local access in Colab. 
To process the same glaciers (Zmutt, Unteraar, Oberaletsch, Zinal, Mauvoisin, Belvedere, Satopanth) and years (2016, 2018, 2020, 2022, 2024) as was done in the context of this work, no manual data download is needed. All relevant data is provided and shared in the Assets of the Google Earth Engine (GEE) Script (1_create_input_layers). 

To extend the analysis to other glaciers and/or years, the follwoing steps will have to be followed: 
* Additional Coherence data: e.g. manual processing using the ESA SNAP software and upload of the processed geotiff file to the GEE assets.
* Inspect Landsat scenes of the desired region/year.
* Add names of the processed coherence and landsat scene in GEE script (1_create_input_layers), run script and export to Drive.
* Add the new glaciers and/or years in the google colab script (complete_RF_all_glaciers.ipynb).

### Software

* **Google Earth Engine** (JavaScript API via browser)
* **Google Colab** with:

  * Python 3
  * `xgboost`, `numpy`, `pandas`, `rasterio`, `scikit-learn`, `matplotlib`, `geopandas`

* If additional coherence data is processed: **ESA SNAP** 

[back to content](#2-required-data-and-software)

---

## 3. Contact

Code development and support: 
\Lorena Müller, Gabriele Bramti, Dr. Kathrin Naegeli, Dr. Hendrik Wulf, Jennifer Susan Adams, Luis Gentner 
\University of Zürich, Remote Sensing Laboratories (RSL)
For questions, suggestions, or bug reports, please contact: \[[your.email@institution.org](mailto:your.email@institution.org)]

[back to content](#3-contact)

---

## 4. Acknowledgements

We thank ESA and NASA for free access to Sentinel-1 and Landsat 8 data.
Topographic data from the Copernicus Digital Elevation Model was provided by the European Space Agency.
We also acknowledge the Swiss Glacier Inventory (SGI) and the Randolph Glacier Inventory (RGI 7.0) for validation and reference.

[back to content](#4-acknowledgements)


---

## 5. License

The scripts in this repository (`*.js`, `*.ipynb`) are licensed under the MIT License (see LICENSE file).
??

**Creative Commons License**
All other content (e.g. figures, tables, documentation) is licensed under a [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).
??
[back to content](#6-license)

---

