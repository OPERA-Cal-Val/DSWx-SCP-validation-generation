# OPERA DSWx validation data using SCP (example)

Demonstrates how to:

    1. Classify satellite optical imagery to identify water/non-water areas
    2. Generate validation data for the OPERA DSWx product
  
## Required software and plugins

    1. QGIS
    2. Semi-Automatic Classification Plugin (SCP)
    3. SNAP
        

QGIS can be installed for your platform using the binary packages provided (https://www.qgis.org/en/site/forusers/download.html) or via package managers such as conda (https://anaconda.org/conda-forge/qgis). SCP is installed using the QGIS Plugin manager (https://fromgistors.blogspot.com/p/plugin-installation2.html). SNAP can be installed using the latest installers (https://step.esa.int/main/download/snap-download/). Note that SNAP is needed to run Random Forest classification with SCP.

## Image classification workflow

![flowchart](https://user-images.githubusercontent.com/29788365/175801911-e99a12d0-5344-43ff-afdf-07ad3082aeca.jpg)

## Valiadation data AOIs

We use the University of Maryland (UMD) Global Land Analysis & Discovery group (GLAD) global surface water reference maps to select validation AOIs. GLAD derived surface water extent validation data from 5 m pixel RapidEye imagery between 2010-2013 (https://glad.umd.edu/dataset/global-surface-water-dynamics). 

![UMDgladlocation](https://user-images.githubusercontent.com/29788365/175829732-65669cff-1608-424b-a1fb-92a63ed39557.jpg)
Above image shows the location of the UMD GLAD surface water extent validation data.


## Validation data example 

Below we walk through the steps to generate validation data. We select AOI (4_42) from the UMD GLAD dataset. This corresponds to a region of northern Australia that contains surface water and ocean water.

![UMD_GLAD_CHIP](https://user-images.githubusercontent.com/29788365/175831036-1cc9f0b1-39fe-493f-b0c0-4d9a77119da4.jpg)

