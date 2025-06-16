# refine_DCG_outlines
This repository contains Google Earth Engine scripts to refine Debris-Covered Glacier (DCG) outlines using Land Surface Temperature (LST) data. 

LST from Landsat 5 and 8 was combined with other spectral (Landsat 5 and 8) and topographic information (NASADEM, Copernicus DEM, SwissAlti3D), 
and outlines of existing glacier Inventories (Randolph Glacier Inventory (RGI) 7.0, Swiss Glacier Inventory (SGI) 2010 and SGI2016) to differentiate 
between DCG and its surroundings. 

The followng scripts are available: 

1. Data selection and generation of input layers
   
   Relevant data is selected and processed. Per glacier and year, 14 images are generated that are used as input layers for
   the DCG classification in step 2. This is performed for Zmuttgletscher, Unteraargletscher and Oberaletschgletscher (Swiss
   Alps) for the years 2003, 2010 and 2016, and for Belvedere and Satopanth Glaciers for the year 2003. The input layers can
   either be based on single scenes or a composite image of the summer of analysis.
   
3. DCG classification
   
   2.1 Temporal changes
   
   Data of one glacier of different years is used for training and testing of the classifier.
   
   2.2 Different glaciers Approach I
   
   Data of the same year of different glaciers is used for training and testing of the classifier. The most current glacier
   outlines are included as one of 14 input layers to the classification.
   
   2.3 Different glaciers Approch II
   
   Data of the same year of different glaciers is used for training and testing of the classifier. The RGI 7.0 outlines
   (dating to the year 2003) are included as one of 14 input layers to the classification, regardless of the year of
   analysis.
   
   2.4 Different glaciers Approch III
   
   Data of the same year of different glaciers is used for training and testing of the classifier. No existing glacier
   outlines are included in the classification. The number of band combinations used for the classification was
   increased to 40.

The scripts in this repository were created for the Master Thesis 'Refining Debris-Covered Glacier Outlines using 
Land Surface Temperature Data', by Lorena Müller. Supervised by Gabriele Bramati, Dr. Kathrin Naegeli and Dr. 
Hendrik Wulf, University of Zürich, Remote Sensing Laboratories, December 2024. 
