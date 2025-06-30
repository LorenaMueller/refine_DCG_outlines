# Automated Mapping of Debris-Covered Glaciers – Code and Data Repository

This repository provides a semi-automated pipeline for mapping debris-covered glaciers (DCGs) using freely available satellite data and machine learning techniques. The workflow leverages:

Google Earth Engine (GEE) to preprocess and export satellite-derived features.
Google Colab for loading these features, training an XGBoost classifier, and generating final glacier outlines.

Our approach aims to improve DCG mapping in complex terrain where traditional methods relying on manual delineation or high-resolution data are not scalable. The methodology is tested on glaciers in Switzerland, Italy, and India, and is designed with global applicability in mind.

Last update to this readme: 24 June 2025.

---

## 1. Repository structure

Here is the structure of this repo. Files have been excluded from this tree for readability.

```
├───code
│   ├───a_gee_preprocessing  
│   ├───b_colab_analysis
|   └───msc_thesis_gee_scripts
│      
├───data
│   ├───coherence
│   └───links_existing_glacier_inventories
│
└───results
    ├───figures
    └───polygon_outlines
```

Code
* The scripts in `gee_preprocessing` are Google Earth Engine (GEE) scripts used to derive relevant spectral, thermal, radar, and topographic features from open satellite data (Sentinel-1, Landsat 8, Copernicus   DEM). The processed image layers are exported to Google Drive. Relevant for this is the first script `1_create_input_Layers`. Links to this script in GEE and to the relevant repository can be found in `1a_gee_links`. The additional scripts are not needed for the data preprocessing, but may provide additional background information on the development of the methodology.
* The script in `colab_analysis` is a Google Colab notebook that loads the exported data, trains an XGBoost classifier, statistically evaluates the results against the Swiss Glacier Inventory (SGI), and generates final glacier outlines, including multi-year averaged maps. The script was splitted into a full version, that executes all described steps for a selected glacier, and a second script with only the evaluation und visualisation of the results, to reduce processing time when only interested in the analysis of the results. 
* The scripts in `msc_thesis_gee_scripts` were created during a Master Thesis that served as the basis for the current analysis and code provided. They are not used anymore for the current state of this work, but may be helpful for background information on the development of the methodology. 

Data
* The folder `coherence` contains a brief overview for the processing of InSAR coherence with two Sentinel-1 SLC scenes using ESA SNAP. The processed coherence data was not added due to their large size. They can be accessed via the GEE script '1_create_input_layers' where they are available in the assets.
* The file 'links_existing_glacier_inventories' provied links to the data download of existing glacier inventories that were used for evaluation of the methodology. 

Results
* The folder `figures` contains visualisations of the classification results per glacier. They can be reproduced with the provided code.
* The folder `polygon_outlines` contains the shapefiles of the classification outlines per glacier, averaged for the time frame 2016-2024. They can be reproduced with the provided code.

[back to content](#1-repository-structure)

---

## 2. Required data and software

### Data

The necessary satellite data is accessed and processed via Google Earth Engine using the script provided. Outputs are exported to Google Drive for local access in Colab. 
To process the same glaciers (Zmutt, Unteraar, Oberaletsch, Zinal, Mauvoisin, Belvedere, Satopanth) and years (2016, 2018, 2020, 2022, 2024) as was done in the context of this work, no manual data download is needed. All relevant data is provided and shared in the Assets of the Google Earth Engine (GEE) Script (1_create_input_layers). 

To extend the analysis to other glaciers and/or years, the follwoing steps will have to be followed: 
* Additional Coherence data: e.g. manual processing using the ESA SNAP software and upload of the processed geotiff file to the GEE assets.
* Inspect Landsat scenes of the desired region/year.
* Add the processed coherence and landsat scene in the GEE script (1_create_input_layers), run script and export to Drive.
* Add the new glaciers and/or years in the google colab script (complete_RF_all_glaciers.ipynb).

### Software

* **Google Earth Engine** (JavaScript API via browser)
* **Google Colab** with:
  * Python 3
  * `xgboost`, `numpy`, `pandas`, `rasterio`, `scikit-learn`, `matplotlib`, `geopandas`

* If additional coherence data is processed: **ESA SNAP** 

[back to content](#2-required-data-and-software)

---

## 3. Informations on the method and workflow recommendations

### Method Overview

**Study Glaciers**
The method was developed using seven debris-covered glaciers (DCGs):
- **Swiss DCGs (used for training and evaluation against SGI2016):**  
  Zmutt, Unteraar, Oberaletsch, Zinal, Mauvoisin  
- **Additional evaluation glaciers:**  
  Belvedere and Satopanth

**Years Analyzed**
- **2016:** Used for statistical evaluation against the SGI2016  
- **2018, 2020, 2022, 2024:** Used to assess potential retreat in glacier extent

### Workflow

1. Preprocessing (Coherence)
    * Sentinel-1 SLC (descending orbit, 12-day interval) coherence was computed using ESA SNAP.

3. Input Layer Generation (Google Earth Engine)
For each glacier and year, the following layers were created:
    * `LST NIR Index`: LST normalized for elevation via regression, then combined with NIR as a normalized difference index
    * `NDVI`: Normalized Difference Vegetation Index
    * `NDSI`: Normalized Difference Snow Index
    * `Slope`: From Copernicus DEM
    * `VH`: VH-band radar backscatter, averaged over one summer
    * `Coherence`: From Sentinel-1 SNAP preprocessing
→ All layers were normalized to a \[0, 1\] range and exported to Google Drive.

3. Classification (Google Colab)
    * Performed using **XGBoost**
    * Included neighboring pixels to add spatial context
    * Each dataset was classified using **20 different band combinations** for robustness
    * Results were combined into an **ensemble prediction**

4. Post-processing
- **Otsu’s method** applied to ensemble output for automatic thresholding
- Binary masks from different years can be merged into a **composite outline**, including only areas classified as glacier in ≥ *n* years (e.g., 3+)
- This results in an **averaged glacier outline for 2016–2024**, offering a more robust basis for modeling and analysis



### Workflow recommendations
For the complete process: 
1. Run GEE script `1_create_input_layers` to create the relevant input layers.
2. Run google colab script 'complete_RF_all_glaciers.ipynb' to run the classification.

For visualisation and analysis of the results, when no changes to the input layers are made: 
1. Access the already processed classification ensemble results via: https://drive.google.com/drive/folders/1y7jBnQ6PUZxdVV6GRvbdyzUl_xE3tR7P?usp=sharing
2. Run the google colab script 'snipplet_RF_all_glaciers.ipynb' to visualize and analyse the results.

---

## 4. Contact

Code development: 
Lorena Müller, in support of Gabriele Bramati, Dr. Kathrin Naegeli, Dr. Hendrik Wulf, Dr. Jennifer Susan Adams, Luis Gentner 
University of Zürich, Remote Sensing Laboratories (RSL)

[back to content](#3-contact)

---

## 5. Acknowledgements

We thank ESA and NASA for free access to Sentinel-1 and Landsat 8 data.
Topographic data from the Copernicus Digital Elevation Model was provided by the European Space Agency.
We also acknowledge the Swiss Glacier Inventory (SGI) and the Randolph Glacier Inventory (RGI 7.0) for validation and reference.

[back to content](#4-acknowledgements)


---



