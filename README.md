# OPERA DSWx validation data using SCP (example)

Demonstrates how to:

    1. Classify satellite optical imagery to identify water/non-water areas
    2. Generate validation data for the OPERA DSWx product
  
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

### UMD GLAD data to select AOI
Below we walk through the steps to generate validation data. We select AOI (4_42) from the UMD GLAD dataset. This corresponds to a region of northern Australia that contains surface water and ocean water (see location in Figure 1).

<!-- ![UMD_GLAD_CHIP](https://user-images.githubusercontent.com/29788365/175831036-1cc9f0b1-39fe-493f-b0c0-4d9a77119da4.jpg) -->

<p align="center">
  <img width="65%" height="65%" src="https://user-images.githubusercontent.com/29788365/175831036-1cc9f0b1-39fe-493f-b0c0-4d9a77119da4.jpg">
</p>
Figure 2. UMD GLAD surface water extent.

### PlanetScope Imagery (8band)

<!-- ![Planet_FalseColor](https://user-images.githubusercontent.com/29788365/176041457-1e4d8cf7-009a-4e5c-8a89-27f260cdc9ab.jpg) -->

<p align="center">
  <img width="65%" height="65%" src="https://user-images.githubusercontent.com/29788365/176041457-1e4d8cf7-009a-4e5c-8a89-27f260cdc9ab.jpg">
</p>
Figure 3. Falsecolor PlanetScope imagery for AOI.
<br />
<br />

Next we perform undersupervised classification with SCP to get a understanding of the different spectral characteristics of the imagery. We use the ISODATA approach (https://semiautomaticclassificationmanual.readthedocs.io/en/latest/remote_sensing.html#isodata-definition) with 10 classes and default parameters. 


![isodata_Table](https://user-images.githubusercontent.com/29788365/176042664-1c50f7fc-b4c1-4240-a52c-ec0d08baa471.png)
Figure 4. SCP clustering table showing parameters used for this undersupervised classification. 
<br />
<br />
SCP returns a classified image that identifies water and land with different classes.







