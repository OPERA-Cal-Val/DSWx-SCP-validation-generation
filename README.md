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
