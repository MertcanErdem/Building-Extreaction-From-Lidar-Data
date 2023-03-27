# Building-Extreaction-From-Lidar-Data

This repository contains Python scripts to generate a building map from LiDAR data using the Normalized Digital Surface Model (NDHM) approach. The process involves generating Digital Surface Model (DSM) and Digital Terrain Model (DTM) from LiDAR data, computing the NDHM, and then applying different image processing algorithms to detect buildings from the NDHM.


## Table of Contents
* [Introduction](#introduction)
* [Requirements](#requirements)
* [Understanding The Project](#understanding-the-project)
* [Setup](#setup)
* [Usage](#usage)
* [Possible Uses for the Project](#possible-uses-for-the-project)
* [Project Status](#project-status)
* [Room for Improvement](#room-for-improvement)
* [Acknowledgements](#acknowledgements)
* [Contact](#contact)

## Introduction:

This Jupyter notebook provides a complete workflow for generating a building map from aerial imagery using Python. The main target of this notebook is to provide a solution for GIS Cup 2022 challenge. The workflow includes steps for downloading and processing aerial imagery, generating a digital surface model (DSM) and a digital terrain model (DTM) using the provided Python scripts, computing a normalized digital surface model (nDHM), and using image processing techniques to extract buildings from the nDHM. 

Note that this notebook assumes you have basic familiarity with Python and the Jupyter notebook environment. If you are new to Python or Jupyter notebooks, you may want to review some introductory resources before diving in.


## Requirements

- Anaconda Distribution
- QGIS
- Python 3.x
- GDAL
- rasterio
- numpy
- opencv-python
- scipy
- laspy 2.0.0
- pyproj

## Setup

1. Install Anaconda Distribution by following since it includes some of the libraries and Jupyer Notebook the instructions [here](https://www.anaconda.com/products/individual). Make sure to add Anaconda to your system's PATH variable during the installation process.
2. Install GDAL

   ```bash
   pip install gdal
   
3. Install rasterio
   
   ```bashl
   pip install rasterio
4. Install opencv

   ```bash
   pip install opencv-python

5. Install laspy

   ```bash
   pip install laspy

6. Install pyrpoj
    ```bash
   pip install pyproj

7.  Install QGIS by following the instructions [here](https://www.qgis.org/en/site/forusers/download.html).
   
  
## Usage
1. Clone the repository and download test LiDAR data from [here](https://sigspatial2022.sigspatial.org/giscup/download.html) or use your own aerial LiDAR data.
2. Place the test data into the folder where you downloaded the Jupyter Notebook or dont forget the folder of your LiDAR data.
3. Open up JupyterLab or Jupyter Notebook from the command line or the Anaconda Navigator.
4. Select folder you placed the notebook file and open up the BuildingEXT.ipynb
5. In the first cell of the notebook change the line to:
   ```python
   las_file2 = laspy.read('Yourfile.laz')
   ```
   If your file is in different directory change it to: 

   ```python
   las_file2 = laspy.read("path/to/Yourfile.laz")
6. Run the first cell and you will be faced with the "Enter the epsg for your laz file:" in the bellow enter your laz files EPSG and press enter.
7. Run the below 3 cells without changing anything
8. Open up your QGIS and the folder where the BuildingEXT.ipynb is located.
9. Drag and Drop <Your las file>, if you have it you <Confiramtion Geojson>,"dsm.tif", "dtm.tif", "ndhm.tif", "buildings.tif" to QGIS and press zoom to layer in one of them
10. 
11. 
12. 
13. 
14. 
15. 
16. 
