# Building-Extreaction-From-Lidar-Data

This repository contains Python scripts to generate a building map from LiDAR data using the Normalized Digital Surface Model (NDHM) approach. The process involves generating Digital Surface Model (DSM) and Digital Terrain Model (DTM) from LiDAR data, computing the NDHM, and then applying different image processing algorithms to detect buildings from the NDHM.


## Table of Contents
* [Introduction](#introduction)
* [Requirements](#requirements)
* [Setup](#setup)
* [Usage](#usage)
* [Understanding The Project](#understanding-the-project)

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
   ```
7. Install Scipy 
   ```bash
   pip install scipy
   ```
8.  Install QGIS by following the instructions [here](https://www.qgis.org/en/site/forusers/download.html).
  
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
10. Use QGIS vectorizer to vectorize our buildings
![1](https://user-images.githubusercontent.com/92017528/228175495-6a880729-346d-4fc3-9cba-727233bc1032.png)


11. Select the vectorized layer and select properties tab after that go the the Symbology tab and  change the layer to catogirized
![2](https://user-images.githubusercontent.com/92017528/228175908-c298e6c8-fcbd-41e6-95bb-b7922a242901.png)

12. Select DN for value and apply the Classification for our building features by pressing Classify
![3](https://user-images.githubusercontent.com/92017528/228176306-6cc4fa0f-ad9f-461c-a328-659b484ca2ae.png)


13. After that we get 3 tabs on our vectorized layer so select the 255 one who has our buildings on it and right click and use the select features tab
![4](https://user-images.githubusercontent.com/92017528/228176631-1189df46-aa70-4c13-8521-ea40f4034072.png)

14. After selecting the featurs right click on the vector layer and cllick on the Export>Save Selected Features tabs
![5](https://user-images.githubusercontent.com/92017528/228176960-e4d7a83e-73d8-4a3c-ba51-5b36255677bf.png)

15. Fill out the neccesary tabs as seen on the bellow ficture and remove the check from id field.
![6](https://user-images.githubusercontent.com/92017528/228177163-76f47c0a-3eac-46c2-aeae-6846bb2661c1.png)

16.Now you got a layer with only the buildings we can compare it to our base layer by using overlap analysis in QGIS 
   ![image](https://user-images.githubusercontent.com/92017528/228202010-3d8f69eb-8650-49c0-a63d-a5959d0add54.png)

17.Enter your ground truth layer to the input Layer section and your building vector layer to Overlay layers section and press run
   ![image](https://user-images.githubusercontent.com/92017528/228202320-e4993cd9-4bda-4e91-b0f1-2a8d8aa00ac2.png)

18.By looking at the attrubite table of the newly created overlap layer we can see the overlapped area in the left and percentage of the polygons in the right
   
   ![image](https://user-images.githubusercontent.com/92017528/228202572-d123cfcb-04ce-4fb4-ba0a-2963317be689.png)

## Understanding The Project
We use  is using the laspy library to read in a LAS file which contains point cloud data. It then sets the CRS information to projection and writes the updated LAS file.

Next, we mannualy define the CRS (coordinate reference system) for the output raster and the code extracts the x, y, and z coordinates from the LAS file. It then sets the resolution of the digital elevation model (DEM) and calculates the grid bounds based on the x and y coordinates.

It generates a grid of points for the DEM and creates the DEM using linear interpolation of the point cloud. Finally, it saves the DEM to a GeoTIFF file using rasterio.

Afterwards we generate a digital terrain model (DTM) from a LiDAR point cloud dataset.

Here is a summary of what the code does:

- It uses the laspy package to read a LAS file (LiDAR point cloud data).
- It classifies the points in the point cloud as either ground or non ground points.
- It determines the bounds of the point cloud and calculates the size of the output raster based on the resolution specified.
- It creates an empty numpy array for the output DTM, and counts for each cell in the array.
- It creates a KDTree from the x, y coordinates of the ground points, and interpolates the z values of the ground points onto a mesh grid using linear interpolation.
- It loads the digital surface model (DSM) from the same LAS file and interpolates the non-ground (aboveground) points onto the mesh grid using linear interpolation.
- It subtracts the interpolated non-ground values from the interpolated ground values to generate the DTM.

Finally, it writes the output DTM to a GeoTIFF file using the rasterio package and the resulting output file dtm.tif will contain elevation values for the ground surface at each point in the raster grid.

After generating the DTM we generate a nDHM by subtracting the DTM from the DSM.

In our filtering phase the code uses the Python libraries rasterio and cv2 (OpenCV) to process a raster image file of an nDHM (Normalized Digital Height Model) and extract a binary mask of the buildings in the image.

First, the code opens the nDHM image using rasterio and reads the raster data into a NumPy array. It also creates a copy of the metadata (profile) of the input file, which will be used later when writing the output file.

Next, the code performs image processing operations using cv2 to separate the buildings from the rest of the image. It first normalizes the NDHM image to 8-bit values using cv2.normalize(), and then applies adaptive thresholding using cv2.adaptiveThreshold() to produce a binary image with the buildings as white pixels and the background as black pixels.

The code then uses cv2.findContours() to find the contours (outlines) of the buildings in the binary image. It filters out contours with low "squareness" (aspect ratio) or small size, which are likely to correspond to vegetation or noise, using a minimum squareness threshold of 0.9 and a minimum size threshold that is computed based on the pixel size of a square meter in the image.

Finally, the code creates a binary mask image of the buildings using cv2.drawContours() and writes the output file as a GeoTIFF using rasterio.open() and dst.write(), with the metadata of the output file updated to reflect a binary output.
