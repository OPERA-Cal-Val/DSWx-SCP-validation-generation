# OPERA DSWx validation data using Semi-Automatic Classification Plugin in QGIS (example)

Demonstrates how to:

    Generate validation data for the OPERA Dynamic Surface Water Extent (DSWx) product. This guide shows the steps that the OPERA DSWx team follows to generate validation data from PlanetScope satellite optical imagery. The validation data consists of a classified raster that labels pixels water and not water. These data are necessary to validate the OPERA DSWx products which will be made from the NASA Harmonized Landsat and Sentinel-2 (HLS) data. All OPERA DSWx validation data will be freely available.
    
## Contents
  
1. [Required Software and Plugins](#required-software-and-plugins)
2. [Required Data](#required-data)
3. [Image classification workflow](#image-classification-workflow)
4. [Valiadation data AOIs](#validation-data-aois)
5. [Validation data example](#validation-data-example)
    -   [Unsupervised Classification](#unsupervised-classification)
    -   [Supervised Classification](#supervised-classification)
    -   [Accuracy Assessment](#accuracy-assessment)
6. [Contributors](#contributors)
## Required software and plugins

    1. QGIS (v3.16)
    2. Semi-Automatic Classification Plugin (SCP) (v7.10.6) for QGIS
    3. Serval (v3.10.5)  for QGIS
    4. SNAP (v8.0) (only for Random forest)
        

QGIS can be installed using the binary packages provided (https://www.qgis.org/en/site/forusers/download.html) or via package managers such as conda (https://anaconda.org/conda-forge/qgis). SCP is installed using the QGIS Plugin manager (https://fromgistors.blogspot.com/p/plugin-installation2.html). SNAP can be installed using the latest installers (https://step.esa.int/main/download/snap-download/). Note that SNAP is only needed to run Random Forest classification with SCP.

## Required data

    1. University of Maryland (UMD) Global Land Analysis and Discovery (GLAD) global surface water reference maps (open access)
    2. PlanetScope Imagery (see below for access)

UMD GLAD surface water extent validation data are derived from 5 m pixel RapidEye imagery between 2010-2013 (https://glad.umd.edu/dataset/global-surface-water-dynamics). PlanetScope imagery are commerical data and can be accessed through the NASA Commercial Smallsat Data Acquisition Program (https://csdap.earthdata.nasa.gov/signup/) or through the Planet Education and Research Program (https://www.planet.com/markets/education-and-research/).
 
## Image classification workflow

![flowchart2](https://user-images.githubusercontent.com/29788365/180252328-9ddd9e31-7a83-47b1-a977-e38eef44852a.jpg)


## Validation data AOIs

We use the UMD GLAD global surface water reference maps to select validation AOIs (Area of Interest). 

<!-- ![UMDgladlocation3](https://user-images.githubusercontent.com/29788365/175997936-e417a966-22c4-4115-8583-90c7ee0cf8f3.png) -->

<p align="center">
  <img width="100%" height="100%" src="https://user-images.githubusercontent.com/29788365/175997936-e417a966-22c4-4115-8583-90c7ee0cf8f3.png">
</p>
Figure 1. Location of the UMD GLAD surface water extent validation data. Black symbol shows location of validation data shown below. Background map from ESRI.
<br />
<br />

## Validation data example 

Below we walk through the steps to generate validation data. We selected an AOI (SAMPLE_ID 4_42) from the UMD GLAD dataset. SAMPLE_ID 4_42 corresponds to a region of northern Australia that contains surface water and ocean water (see location in Figure 1).

<!-- ![UMD_GLAD_CHIP](https://user-images.githubusercontent.com/29788365/175831036-1cc9f0b1-39fe-493f-b0c0-4d9a77119da4.jpg) -->

<p align="center">
  <img width="65%" height="65%" src="https://user-images.githubusercontent.com/29788365/175831036-1cc9f0b1-39fe-493f-b0c0-4d9a77119da4.jpg">
</p>
Figure 2. UMD GLAD surface water extent for SAMPLE_ID 4_42. Classified image was made from RapidEye imagery acquired on 2012-04-24.

<br />
<br />
<br />
Next, we co-located a PlanetScope image acquired on the same day as a NASA Harmonized Landsat Sentinel-2 (HLS) product. A notebook for Planet and HLS co-location are located here: https://github.com/OPERA-Cal-Val/calval-DSWx/blob/main/planet_api/Collocation2geojson.ipynb. We found a coincident image acquired on 2021-09-24.
<br />
<br />
<!-- ![Planet_FalseColor](https://user-images.githubusercontent.com/29788365/176041457-1e4d8cf7-009a-4e5c-8a89-27f260cdc9ab.jpg) -->

<p align="center">
  <img width="65%" height="65%" src="https://user-images.githubusercontent.com/29788365/176041457-1e4d8cf7-009a-4e5c-8a89-27f260cdc9ab.jpg">
</p>
Figure 3. False color PlanetScope imagery for AOI.
<br />
<br />

### Unsupervised Classification 
The next step is to perform unsupervised classification with SCP. We use unsupervised classification to gain a better understanding of the different spectral characteristics of the imagery. We use the ISODATA approach (https://semiautomaticclassificationmanual.readthedocs.io/en/latest/remote_sensing.html#isodata-definition) with 10 classes and default parameters (Figure 4). Other methods such as k-means are also acceptable. For more information we recommend following the "Unsupervised Classification using the Semi-Automatic Classification Plugin version 7" tutorial (https://fromgistors.blogspot.com/search/label/Tutorial). 

![isodata_Table](https://user-images.githubusercontent.com/29788365/176042664-1c50f7fc-b4c1-4240-a52c-ec0d08baa471.png)
Figure 4. SCP clustering table showing parameters used for this unsupervised classification. 
<br />
<br />

The classified PlanetScope image helps differentiate water and no water regions. Visual inspection of the classified image and the false color optical image shows surface water in the form of channel networks near the shoreline.

<!-- ![isoddata](https://user-images.githubusercontent.com/29788365/176062184-814142dd-d794-43c7-a91e-8577405ef60f.jpg) -->

<p align="center">
  <img width="65%" height="65%" src="https://user-images.githubusercontent.com/29788365/176062184-814142dd-d794-43c7-a91e-8577405ef60f.jpg">
</p>
Figure 5. ISODATA classification of PlanetScope image with 10 classes. 
<br />
<br />

### Supervised Classification 
We then use both the classified image and original PlanetScope image to create training data for supervised classification. Our training data consisted of hand drawn labels for water and no water.

<!--![train3](https://user-images.githubusercontent.com/29788365/176083851-364be0a9-7511-4ed9-b733-68fa836d4ea9.jpg) -->


<p align="center">
  <img width="105%" height="105%" src="https://user-images.githubusercontent.com/29788365/176083851-364be0a9-7511-4ed9-b733-68fa836d4ea9.jpg">
</p>
Figure 6. Side by side comparison of false color image and classified image showing the locations of training data.
<br />
<br />

These training data are then used for performing supervised classification (Minimum Distance, Maximum Likelihood, Spectral Angle Mapping, or Random Forest). More details are found here: https://fromgistors.blogspot.com/p/user-manual.html. Below we show an example of using the minimum distance method to classify the PlanetScope imagery.

We apply the minimum distance algorithm with SCP using the default parameters.
![min_distance screen grab](https://user-images.githubusercontent.com/29788365/176080935-6b5b18ca-e589-40ba-b1ec-2b8c7ff5abc7.png)
Figure 7. SCP classification table showing parameters used for supervised classification. 
<br />
<br />

<!-- ![Class_editedFixed](https://user-images.githubusercontent.com/29788365/180098652-1ccb724f-ed5d-471e-82e8-37481aa97d77.png)  -->
<!--![Class_notedited](https://user-images.githubusercontent.com/29788365/180100066-00fb570d-b6c6-41f3-9b2e-bcb0597f68c9.png) -->


<p align="center">
  <img width="65%" height="65%" src="https://user-images.githubusercontent.com/29788365/180100066-00fb570d-b6c6-41f3-9b2e-bcb0597f68c9.png">
</p>
Figure 8. Supervised classification (min. distance) of PlanetScope image with water and no water classes. 
<br />
<br />

### Accuracy Assessment  

#### a. Visual Inspection  

Our workflow provides a classified image that delineates the water and no water regions. However, there will inevitably be areas that are misclassified. In other words, there are water areas classified as not water and vice versa. To get a better sense of classification quality, we zoom into a small region in the northwest region of the AOI.

![3zoomnotedited](https://user-images.githubusercontent.com/29788365/180100529-3c0b5551-915b-4a67-a1a2-f9146424d017.png)

Figure 9. Zoom in of PlanetScope image (false color), unsupervised classification (ISODATA), and supervised classification (min. distance). 
<br />
<br />

Visual inspection shows that overall, the classification result is of good quality, but there are some classification errors. To improve the classification, we add additional training data and rerun the supervised classification. It sometimes takes several iterations to develop a reliable training dataset. Finally, when there are only very few classification errors remaining, it is possible to manually edit the classified raster using the Serval plugin.

![zoomin_w_edits](https://user-images.githubusercontent.com/29788365/180101973-db15613d-a448-4026-beb3-0bb7ef8c32e8.png)

Figure 10. Zoom in of image, classified image, refined and edited classified image
<br />
<br />

#### b. Quantitative Assessment  

There are several methods to calculate the accuracy assessment. Here we will use the Stratified Random Points workflow in SCP to calculate the error matrix and accuracy estimate. The full tutorial is provided here: https://fromgistors.blogspot.com/2019/09/Accuracy-Assessment-of-Land-Cover-Classification.html. We will summarize the key steps.

The SCP accuracy assessment approach compares a random sample of points in which the true land cover is known to a classified image. The sample size is chosen in a way to attain a low standard error (Olofsson et al., 2014) and is calculated as 


<p align="center">
  <img width="21%" height="21%" src="https://user-images.githubusercontent.com/29788365/180312613-3fab8ca3-fc14-4433-ad4f-c569407e81dd.png">
</p>
<br />

where W<sub>i</sub> = mapped area proportion of class i, S<sub>o</sub>  =  standard deviation of overall accuracy that we would like to achieve, and S<sub>i</sub>  = standard deviation of stratum i,. S<sub>i</sub> can be defined as sqrt[U<sub>i</sub>(1-U<sub>i</sub>)], where U<sub>i</sub> are conjectured values of user's accuracies.

## Contributors

    Alexander Handwerger
    Charlie Marshak
    Grace Bato
    John Jones
    Matt Bonnema
    Simran Sangha

