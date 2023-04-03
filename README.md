# Building Extreaction From Lidar Data

This repository contains Python scripts to generate a building map from LiDAR data using the Normalized Digital Surface Model (nDHM) approach. The process involves generating Digital Surface Model (DSM) and Digital Terrain Model (DTM) from LiDAR data, computing the nDHM, and then applying different image processing algorithms to detect buildings from the nDHM.


## Table of Contents
* [Introduction](#introduction)
* [Requirements](#requirements)
* [Setup](#setup)
* [Usage](#usage)
* [Understanding The Project](#understanding-the-project)
* [Acknowledgements](#acknowledgements)
* [Contact](#contact)

## Introduction:

This Jupyter notebook provides a complete workflow for generating a building map from aerial imagery using Python. The main target of this notebook is to provide a solution for GIS Cup 2022 challenge. The workflow includes steps for downloading and processing aerial imagery, generating a digital surface model (DSM) and a digital terrain model (DTM) using the provided Python scripts, computing a normalized digital surface model (nDHM), and using image processing techniques to extract buildings from the nDHM. 

Note that this notebook assumes you have basic familiarity with Python and the Jupyter notebook environment. If you are new to Python or Jupyter notebooks, you may want to review some introductory resources before diving in.


## Requirements

- Anaconda Distribution
- QGIS 3.8
- Python 3.x
- GDAL 3.2.3
- rasterio 1.3.4
- numpy 1.23.5
- opencv-python 4.7.0.72
- scipy 1.10.1
- laspy 2.0.0 with laszip extension
- pyproj 3.4.1

## Setup

1. Install Anaconda Distribution by following since it includes some of the libraries and Jupyer Notebook. You can download it with instructions [here](https://www.anaconda.com/products/individual). Make sure to add Anaconda to your system's PATH variable during the installation process.
2. Install GDAL

   ```bash
   pip install gdal
   
3. Install rasterio
   
   ```bashl
   pip install rasterio
4. Install opencv

   ```bash
   pip install opencv-python

5. Install laspy 2.0.0 and laszip requirement

   ```bash
   python -m pip install "laspy[lazrs,laszip]"==2.0.0
   ```
   or
   ```bash
   python3 -m pip install "laspy[lazrs,laszip]"==2.0.0
   ```
6. Install pyrpoj
    ```bash
   pip install pyproj
   ```
7. Install Scipy 
   ```bash
   pip install scipy
   ```
8.  Install QGIS 3.28 or update to QGIS 3.28 by following the instructions [here](https://www.qgis.org/en/site/forusers/download.html) since we need a QGIS version higher than 3.28 for open our laz files on it.
  
## Usage
**1**. Clone the repository and download test LiDAR data from [here](https://sigspatial2022.sigspatial.org/giscup/download.html) or use your own aerial LiDAR data.

**2**. Place the test data into the folder where you downloaded the Jupyter Notebook or dont forget the folder of your LiDAR data.

**3**. Open up JupyterLab or Jupyter Notebook from the command line or the Anaconda Navigator.

**4**. Select folder you placed the notebook file and open up the BuildingEXT.ipynb
 
**5**. In the first cell of the notebook change the line to:
   ```python
   las_file2 = laspy.read('Yourfile.laz')
   ```
   If your file is in different directory change it to: 

   ```python
   las_file2 = laspy.read("path/to/Yourfile.laz")
   ```
**6**. Once you run the first cell, you will encounter a prompt saying "Enter the EPSG for your laz file:" below it. Enter the EPSG code for your laz file and press enter. If you are unsure about the EPSG code of your Point Cloud data, you can learn it by opening your laz file in QGIS. Right-click on the layer and select "Properties." Then, copy the CRS name and paste it into Google to obtain the EPSG code for your laz file.

![image](https://user-images.githubusercontent.com/92017528/228278327-796ce627-6158-46ec-ba42-ce86f6adadcc.png)

    ![3](https://user-images.githubusercontent.com/92017528/228275978-d331043f-2311-4e65-9aa1-e2bb870c5d1f.png)

![image](https://user-images.githubusercontent.com/92017528/228278968-d982a95d-ce18-4d52-8c55-b8df193b017e.png)


**7**. Run the below 3 cells without changing anything unyil you see the All of our steps are done prompt ate the bottom.

**8**. Open up your QGIS and the folder where the BuildingEXT.ipynb is located.

**9**. Drag and Drop <Your las file>, if you have it you <Confiramtion Geojson>,"dsm.tif", "dtm.tif", "ndhm.tif", "buildings.tif" to QGIS and press zoom to layer in one of them.
   
**10**. Use QGIS vectorizer to vectorize our buildings.
   
![1](https://user-images.githubusercontent.com/92017528/228175495-6a880729-346d-4fc3-9cba-727233bc1032.png)


**11**. Select the vectorized layer and select properties tab after that go the the Symbology tab and  change the layer to catogirized.

![2](https://user-images.githubusercontent.com/92017528/228175908-c298e6c8-fcbd-41e6-95bb-b7922a242901.png)

**12**. Select DN for value and apply the Classification for our building features by pressing Classify

![2](https://user-images.githubusercontent.com/92017528/228278670-843d9434-2e9a-40c2-ba1b-bd4491378d02.png)


**13**. After that we get 3 tabs on our vectorized layer so select the 255 one who has our buildings on it and right click and use the select features tab.
   
![Ekran görüntüsü 2023-03-28 172810](https://user-images.githubusercontent.com/92017528/228278701-7338575e-97c1-451f-a464-4dadb35d4287.png)


**14**. After selecting the featurs right click on the vector layer and cllick on the Export>Save Selected Features tabs.

![5](https://user-images.githubusercontent.com/92017528/228176960-e4d7a83e-73d8-4a3c-ba51-5b36255677bf.png)

**15**. Fill out the neccesary tabs as seen on the bellow ficture and remove the check from id field.
![6](https://user-images.githubusercontent.com/92017528/228177163-76f47c0a-3eac-46c2-aeae-6846bb2661c1.png)

**16**. Now you got a layer with only the buildings we can compare it to our base layer by using overlap analysis in QGIS .
   ![image](https://user-images.githubusercontent.com/92017528/228202010-3d8f69eb-8650-49c0-a63d-a5959d0add54.png)

**17**. Enter your ground truth layer to the input Layer section and your building vector layer to Overlay layers section and press run.

   ![image](https://user-images.githubusercontent.com/92017528/228202320-e4993cd9-4bda-4e91-b0f1-2a8d8aa00ac2.png)

**18**. By looking at the attrubite table of the newly created overlap layer we can see the overlapped area in the left and percentage of the polygons in the right.
   
![image](https://user-images.githubusercontent.com/92017528/229314149-badc6a08-2c40-4475-bc83-4858077953c5.png)

**19**. The algorithm's strictness can be adjusted by changing the values of several parameters in the code by , including the adaptive thresholding parameters, minimum and maximum building sizes, squareness threshold, and morphological parameters.

![image](https://user-images.githubusercontent.com/92017528/229481746-cd365362-1437-42a5-81eb-7635127440f3.png)


Regarding the adaptive thresholding parameters, the block size determines the size of the neighborhood area used for thresholding, while the constant value subtracted from the mean or weighted sum of the neighborhood pixels. The current code uses a block size of 51 and a constant value of 4.6. Altering these values can influence the threshold's strictness, where a smaller block size can enhance the detail but may introduce more noise, while a larger block size can lead to a smoother image but may compromise some detail. Likewise, a higher constant value can result in a stricter threshold, while a lower value can make the threshold more lenient. The minimum and maximum sizes of buildings considered are 35 and 5000, respectively. You can change the minimum value according to the size of the buildings in your area. A contour squareness value less than 0.3 is used to filter out vegetation or noise which has low squareness compared to buildings. If you want the code to only take square buildings into account, use a higher squareness parameter. Morphological operations are used to remove noise and fill gaps in the binary mask with a (3, 3) kernel for opening. A smaller kernel can remove less noise, while a larger kernel can remove more noise but may also eliminate some valid structures (especially if you are using more than a 3x3 file in our sample point clouds). These values can be adjusted to modify the algorithm's strictness.



## Understanding The Project
We use  is using the laspy library to read in a LAS file which contains point cloud data. It then sets the CRS information to projection and writes the updated LAS file.

Next, we mannualy define the CRS (coordinate reference system) for the output raster and the code extracts the x, y, and z coordinates from the LAS file. It then sets the resolution of the digital elevation model (DEM) and calculates the grid bounds based on the x and y coordinates.

It generates a grid of points for the DEM and creates the DEM using linear interpolation of the point cloud. Finally, it saves the DEM to a GeoTIFF file using rasterio.

Afterwards we generate a digital terrain model (DTM) from a LiDAR point cloud dataset.

Here is a summary of what the code does:

- It uses the laspy package to read a LAS file (LiDAR point cloud data).
- It uses the point cloud data's own ground point classification categorization system to separate the points in the point cloud as either ground or non-ground points.
   
  ![image](https://user-images.githubusercontent.com/92017528/228279446-ba78e4e6-821c-4e09-8b67-f041e590d77e.png)

- It determines the bounds of the point cloud and calculates the size of the output raster based on the resolution specified.
- It creates an empty numpy array for the output DTM, and counts for each cell in the array.
- It creates a KDTree from the x, y coordinates of the ground points, and interpolates the z values of the ground points onto a mesh grid using linear interpolation.
- It loads the updated LAS file and interpolates the non-ground (aboveground) points onto the mesh grid using linear interpolation.
- It subtracts the interpolated non-ground values from the interpolated ground values to generate the DTM.

Finally, it writes the output DTM to a GeoTIFF file using the rasterio package and the resulting output file dtm.tif will contain elevation values for the ground surface at each point in the raster grid.

After generating the DTM we generate a nDHM by subtracting the DTM from the DSM.

In our filtering phase the code uses the Python libraries rasterio and cv2 (OpenCV) to process a raster image file of an nDHM (Normalized Digital Height Model) and extract a binary mask of the buildings in the image.

First, the code opens the nDHM image using rasterio and reads the raster data into a NumPy array. It also creates a copy of the metadata (profile) of the input file, which will be used later when writing the output file.

Next, the code performs image processing operations using cv2 to separate the buildings from the rest of the image. It first normalizes the nDHM image to 8-bit values using cv2.normalize(), and then applies adaptive thresholding using cv2.adaptiveThreshold() to produce a binary image with the buildings as white pixels and the background as black pixels.

The code then uses cv2.findContours() to find the contours (outlines) of the buildings in the binary image. It filters out contours with low "squareness" (aspect ratio) or small size, which are likely to correspond to vegetation or noise, using a minimum squareness threshold of 0.9 and a minimum size threshold that is computed based on the pixel size of a square meter in the image.

Finally, the code creates a binary mask image of the buildings using cv2.drawContours() and writes the output file as a GeoTIFF using rasterio.open() and dst.write(), with the metadata of the output file updated to reflect a binary output.

## Acknowledgements
  - This project was used data and idea from [this competition](https://sigspatial2022.sigspatial.org/giscup/index.html).
   
## Contact
| Team Members | Account | Link | 
| ------ | ------ | ------ | 
| Ümmü Sude YILDIRIM| LinkedIn |https://www.linkedin.com/in/sude-yildirim-6923591a3/ |
| Mertcan ERDEM | LinkedIn | https://www.linkedin.com/in/mertcan-erdem-54a500209/|
