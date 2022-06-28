# OPERA DSWx validation data using SCP (example)

Demonstrates how to:

    Generate validation data for the OPERA DSWx product
  
## Required software and plugins

    1. QGIS
    2. Semi-Automatic Classification Plugin (SCP)
    3. SNAP
        

QGIS can be installed for your platform using the binary packages provided (https://www.qgis.org/en/site/forusers/download.html) or via package managers such as conda (https://anaconda.org/conda-forge/qgis). SCP is installed using the QGIS Plugin manager (https://fromgistors.blogspot.com/p/plugin-installation2.html). SNAP can be installed using the latest installers (https://step.esa.int/main/download/snap-download/). Note that SNAP is only needed to run Random Forest classification with SCP.

## Image classification workflow

![flowchart](https://user-images.githubusercontent.com/29788365/175801911-e99a12d0-5344-43ff-afdf-07ad3082aeca.jpg)

## Valiadation data AOIs

We use the University of Maryland (UMD) Global Land Analysis & Discovery group (GLAD) global surface water reference maps to select validation AOIs. GLAD derived surface water extent validation data from 5 m pixel RapidEye imagery between 2010-2013 (https://glad.umd.edu/dataset/global-surface-water-dynamics). 

![UMDgladlocation3](https://user-images.githubusercontent.com/29788365/175997936-e417a966-22c4-4115-8583-90c7ee0cf8f3.png)

Figure 1. Location of the UMD GLAD surface water extent validation data. Black symbol shows location of validation data shown below. Background map from ESRI.


## Validation data example 

Below we walk through the steps to generate validation data. We selected an AOI (chip 4_42) from the UMD GLAD dataset. Chip 4_42 corresponds to a region of northern Australia that contains surface water and ocean water (see location in Figure 1).

<!-- ![UMD_GLAD_CHIP](https://user-images.githubusercontent.com/29788365/175831036-1cc9f0b1-39fe-493f-b0c0-4d9a77119da4.jpg) -->

<p align="center">
  <img width="65%" height="65%" src="https://user-images.githubusercontent.com/29788365/175831036-1cc9f0b1-39fe-493f-b0c0-4d9a77119da4.jpg">
</p>
Figure 2. UMD GLAD surface water extent for chip 4_42. Classified image was made from RapidEye imagery acquired on 2012-04-24.

<br />
<br />
<br />
Next, we co-located a PlanetScope image acquired on the same day as a NASA Harmonized Landsat Sentinel-2 (HLS) product. A notebook for Planet and HLS co-location are located here: https://github.com/OPERA-Cal-Val/calval-DSWx/blob/main/planet_api/Collocation2geojson.ipynb. We found a coincedent image acquired on 2021-09-24.

<!-- ![Planet_FalseColor](https://user-images.githubusercontent.com/29788365/176041457-1e4d8cf7-009a-4e5c-8a89-27f260cdc9ab.jpg) -->

<p align="center">
  <img width="65%" height="65%" src="https://user-images.githubusercontent.com/29788365/176041457-1e4d8cf7-009a-4e5c-8a89-27f260cdc9ab.jpg">
</p>
Figure 3. False color PlanetScope imagery for AOI.
<br />
<br />

### Unsupervised Classification 
The next step is to peform unsupervised classification with SCP. We use unsupervised classification to gain a better understanding of the different spectral characteristics of the imagery. We use the ISODATA approach (https://semiautomaticclassificationmanual.readthedocs.io/en/latest/remote_sensing.html#isodata-definition) with 10 classes and default parameters (Figure 4). Other methods such as k-means are also acceptable. For more information we recommend following the "Unsupervised Classification using the Semi-Automatic Classification Plugin version 7" tutorial (https://fromgistors.blogspot.com/search/label/Tutorial). 

![isodata_Table](https://user-images.githubusercontent.com/29788365/176042664-1c50f7fc-b4c1-4240-a52c-ec0d08baa471.png)
Figure 4. SCP clustering table showing parameters used for this undersupervised classification. 
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
We then use both the classified image and original PlanetScope image to create training data for supervised classification. Our training data consited of hand drawn labels for water and no water.

<!-- ![training2](https://user-images.githubusercontent.com/29788365/176068431-bea98279-1411-4e8f-806d-fd93d0c85f21.jpg)  -->

<p align="center">
  <img width="105%" height="105%" src="https://user-images.githubusercontent.com/29788365/176068431-bea98279-1411-4e8f-806d-fd93d0c85f21.jpg">
</p>
Figure 6. Side by side comparison of false color image and classified image showing the locations of training data.
<br />
<br />

These training data are then used for performing supervised classification (Minimum Distance, Maximum Likelihood, Spectral Angle Mapping, or Random Forest). More details are found here: https://fromgistors.blogspot.com/p/user-manual.html. Below we show an example of using the minimum distance method to classify the PlanetScope imagery.

We apply the minimum distance algorithm with SCP using the default parameters.
![min_distance screen grab](https://user-images.githubusercontent.com/29788365/176080935-6b5b18ca-e589-40ba-b1ec-2b8c7ff5abc7.png)
Figure 7. SCP classifictation table showing parameters used for supervised classification. 
<br />
<br />

<!-- ![minDist](https://user-images.githubusercontent.com/29788365/176081596-b8588f94-a496-4f8d-a44e-7557e214c410.jpg)  -->

<p align="center">
  <img width="65%" height="65%" src="https://user-images.githubusercontent.com/29788365/176081596-b8588f94-a496-4f8d-a44e-7557e214c410.jpg">
</p>
Figure 8. Supervised (min. distance) classification of PlanetScope image with water and no water classes. 
<br />
<br />

