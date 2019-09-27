# Geogrid

**Python module for precise mapping between (pixel index, pixel displacement) in imaging coordinates and (geolocation, motion velocity) in geographic coordinates**

**The current version can be installed with the ISCE (The InSAR Scientific Computing Environment; https://github.com/isce-framework/isce2) software (that supports both radar and optical images) or as a standalone Python module (only supports optical images)**

**In combination with the Python module, autoRIFT (https://github.com/leiyangleon/autoRIFT), this module can be used to create feature tracking imagery (e.g. land ice motion velocity) over arbitrary geographic-coordinate grid (e.g. Digital Elevation Model)**


Copyright (C) 2019 California Institute of Technology.  Government Sponsorship Acknowledged.

Link: https://github.com/leiyangleon/Geogrid



## 1. Authors


Piyush Agram (JPL/Caltech; piyush.agram@jpl.nasa.gov), Yang Lei (GPS/Caltech; ylei@caltech.edu)

## 2. Acknowledgement

This effort was funded by the NASA MEaSUREs program in contribution to the Inter-mission Time Series of Land Ice Velocity and Elevation (ITS_LIVE) project (https://its-live.jpl.nasa.gov/) and through Alex Gardner’s participation in the NASA NISAR Science Team
       
       
## 3. Features

* user can define a grid in geographic coordinates provided in the form of a Digital Elevation Model (DEM) with arbitrary EPSG code, 
* the program will extract the portion of the grid that overlaps with the given co-registered image pair, 
* for radar images, use radar orbit information plus DEM along with GDAL coordinate transformation to precisely map the geolocation and the motion velocity (in geographic coordinates) at each grid point to the corresponding pixel index and pixel displacement (in imaging coordinates) in the radar image pair, where the imaging along-track and line-of-sight unit vectors are precisely derived at each grid point
* for optical images, use map coordinate information of the optical image pair along with GDAL coordinate transformation to precisely map the geolocation and the motion velocity (in geographic coordinates) at each grid point to the corresponding pixel index and pixel displacement (in imaging coordinates) in the optical image pair, where the imaging horizontal- and vertical-direction unit vectors are precisely derived at each grid point
* the geographic z-direction motion velocity is estimated using the irrotational flow assumption as well as inputs from the geographic x- and y-direction motion velocity maps and the geographic x- and y-direction local surface slope maps
* return the pixel indices in the image pair for each grid point
* return the pixel displacement given the motion velocity maps and the local surface slope maps in the direction of both geographic x- and y-coordinates (they must be provided at the same grid as the DEM)
* return the matrix of conversion coefficients that can convert the fine pixel displacement between the two images (estimated with the Python module "autoRIFT" https://github.com/leiyangleon/autoRIFT) to motion velocity in geographic x- and y-coordinates
* the current version can be installed with the ISCE software (that supports both radar and optical images) or as a standalone Python module (only supports optical images)
* in combination with the Python module, autoRIFT (https://github.com/leiyangleon/autoRIFT), this module can be used to create feature tracking imagery (e.g. land ice motion velocity) over arbitrary geographic-coordinate grid (e.g. Digital Elevation Model)
* all outputs are in the format of GeoTIFF with the same EPSG code as input

## 4. Demo

_4.1 Radar Demo:_

<img src="figures/optical1.png" width="50%">

***Test area and dataset: optical image over the Jakobshavn glacier where the red rectangle marks boundary of the Sentinel-1A/B image pair (20170221-20170227). Input files in this test scenario consist of the Digital Elevation Model (DEM), local surface slope maps (in both x- and y-direction) and motion velocity maps (in both x- and y-direction) over the entire Greenland, where all maps share the same geographic-coordinate grid with 240-m spacing and spatial reference system with EPSG code 3413 (a.k.a WGS 84 / NSIDC Sea Ice Polar Stereographic North).***



<img src="figures/geogrid.png" width="100%">

***Output of "Geogrid" module: (a) range pixel index at each grid point, (b) azimuth pixel index at each grid point, (c) range pixel displacement at each grid point, (d) azimuth pixel displacement at each grid point. Note: only the portion of the grid overlapping with the radar image pair has been extracted and shown.***

This is obtained by implementing the following command line:

With ISCE:

       testGeogrid_ISCE.py -m master_image_folder -s slave_image_folder -d demname -sx dhdxname -sy dhdyname -vx vxname -vy vyname

where "master_image_folder" and "slave_image_folder" are the folders storing master and slave image information (e.g. radar parameters), and "demname", "dhdxname", "dhdyname", "vxname", "vyname" are defined below in the instructions.


Using the matrix of conversion coefficients, when fine pixel displacement are estimated from radar data, they can be immediately converted to motion velocity. See the final result below by using the matrix of conversion coefficients from the "Geogrid" module and the radar-estimated fine pixel displacement from the "autoRIFT" module (https://github.com/leiyangleon/autoRIFT).


<img src="figures/autorift2.png" width="100%">

***Final motion velocity results by combining outputs from "Geogrid" and "autoRIFT" modules: (a) estimated motion velocity from Sentinel-1 data (x-direction; in m/yr), (b) motion velocity from input data (x-direction; in m/yr), (c) estimated motion velocity from Sentinel-1 data (y-direction; in m/yr), (d) motion velocity from input data (y-direction; in m/yr). Notes: all maps are established exactly over the same geographic-coordinate grid from input.***



_4.2 Optical Demo:_

<img src="figures/optical_opt.png" width="50%">

***Test area and dataset: optical image over Greenland (to the north of the Jakobshavn glacier) where the red rectangle marks boundary of the Landsat-8 image pair (20170708-20170724). Input files in this test scenario consist of the Digital Elevation Model (DEM), local surface slope maps (in both x- and y-direction) and motion velocity maps (in both x- and y-direction) over the entire Greenland, where all maps share the same geographic-coordinate grid with 240-m spacing and spatial reference system with EPSG code 3413 (a.k.a WGS 84 / NSIDC Sea Ice Polar Stereographic North).***



<img src="figures/geogrid_opt.png" width="100%">

***Output of "Geogrid" module: (a) horizontal pixel index at each grid point, (b) vertical pixel index at each grid point, (c) horizontal pixel displacement at each grid point, (d) vertical pixel displacement at each grid point. Note: only the portion of the grid overlapping with the optical image pair has been extracted and shown.***

This is obtained by implementing the following command line:

With ISCE:

       testGeogrid_ISCE.py -m image1 -s image2 -d demname -sx dhdxname -sy dhdyname -vx vxname -vy vyname -fo 1

Standalone:

       testGeogridOptical.py -m image1 -s image2 -d demname -sx dhdxname -sy dhdyname -vx vxname -vy vyname

where "image1" and "image2" are the optical images with map coordinate information (e.g. projection, coordinates), and "demname", "dhdxname", "dhdyname", "vxname", "vyname" are defined below in the instructions. The "-fo" option of "testGeogrid_ISCE.py" indicates whether or not to read optical image data.


Using the matrix of conversion coefficients, when fine pixel displacement are estimated from optical data, they can be immediately converted to motion velocity. See the final result below by using the matrix of conversion coefficients from the "Geogrid" module and the optical data-estimated fine pixel displacement from the "autoRIFT" module (https://github.com/leiyangleon/autoRIFT).


<img src="figures/autorift2_opt.png" width="100%">

***Final motion velocity results by combining outputs from "Geogrid" and "autoRIFT" modules: (a) estimated motion velocity from Landsat-8 data (x-direction; in m/yr), (b) motion velocity from input data (x-direction; in m/yr), (c) estimated motion velocity from Landsat-8 data (y-direction; in m/yr), (d) motion velocity from input data (y-direction; in m/yr). Notes: all maps are established exactly over the same geographic-coordinate grid from input.***


## 6. Install

**With ISCE:**

* First install ISCE (https://github.com/isce-framework/isce2)
* Put the "geo_autoRIFT" folder and the "Sconscript" file under the "contrib" folder that is one level down ISCE's source directory (denoted as "isce-version"; where you started installing ISCE), i.e. "isce-version/contrib/" (see the snapshot below)

<img src="figures/install_ISCE.png" width="35%">

* Run "scons install" again from ISCE's source directory "isce-version" using command line
* This distribution automatically installs the "Geogrid" module as well as the "autoRIFT" module (https://github.com/leiyangleon/autoRIFT).


**Standalone:**

* Put the "geo_autoRIFT" folder and the "setup.py" file under some source directory (see the snapshot below)

<img src="figures/install_standalone.png" width="35%">

* Run "python3 setup.py install" or "sudo python3 setup.py install" (if the previous failed due to permission restriction) using command line
* This distribution automatically installs the "Geogrid" module as well as the "autoRIFT" module (https://github.com/leiyangleon/autoRIFT)
* The standalone version only supports optical images.
* If the modules cannot be imported in Python environment, please make sure the path where these modules are installed (see "setup.py") to be added to the environmental variable $PYTHONPATH.


## 5. Instructions


**Note:**

* For radar data, it is recommended to run ISCE up to the step where co-registered SLC's are done, e.g. "mergebursts" for using topsApp.
* For optical data, the optical images have to be co-registered with the same posting as well as the same x- and y-limits in map coordinates.

**For quick use:**

_Radar data:_
* Refer to the file "testGeogrid_ISCE.py" (with ISCE) for the usage of the module and modify it for your own purpose
* Input files include the master image folder (required), slave image folder (required), a DEM (required), local surface slope maps, velocity maps
* Output files include 1) the range and azimuth pixel indices, 2) the range and azimuth pixel displacement, 3) the conversion coefficients from radar range and azimuth displacement to motion velocity in geographic x-coordinate, and 4) the conversion coefficients from radar range and azimuth displacement to motion velocity in geographic y-coordinate. 

_Optical data:_
* Refer to the file "testGeogrid_ISCE.py" (with ISCE) and "testGeogridOptical.py" (standalone) for the usage of the module and modify it for your own purpose
* Input files include the image 1 (required), image 2 (required), a DEM (required), local surface slope maps, velocity maps
* Output files include 1) the horizontal and vertical pixel indices, 2) the horizontal and vertical pixel displacement, 3) the conversion coefficients from horizontal and vertical displacement to motion velocity in geographic x-coordinate, and 4) the conversion coefficients from horizontal and vertical displacement to motion velocity in geographic y-coordinate. 

_Note: among these, 1) will always be created, while 2-4) will be generated contingent upon that local surface slope and velocity maps are provided_

**For modular use:**

* In Python environment, type the following to import the "Geogrid" module and initialize the "Geogrid" object

_With ISCE:_

       import isce
       from contrib.geo_autoRIFT.geogrid import Geogrid, GeogridOptical
       obj = Geogrid() or obj = GeogridOptical()
       obj.configure()

_Standalone:_

       import GeogridOptical as GO
       obj = GO.GeogridOptical()

where "Geogrid()" is for radar data and "GeogridOptical()" for optical data.


* The "Geogrid" object has several parameters that have to be set up (listed below; can also be obtained by referring to "testGeogrid_ISCE.py"): 

       ------------------radar parameters (for radar only)------------------
       startingRange:       starting range
       rangePixelSize:      range pixel size
       sensingStart:        starting azimuth time
       prf:                 pulse repition frequency 
       lookSide:            look side, e.g. -1 for right looking 
       repeatTime:          time period between the acquisition of the two radar images
       numberOfLines:       number of lines (in azimuth)
       numberOfSamples:     number of samples (in range)
       orbit:               ISCE orbit data structure
       
       ------------------optical parameters (for optical only)------------------
       startingX:           starting coordinate in x direction
       startingY:           starting coordinate in y direction
       XSize:               resolution in x direction
       YSize:               resolution in y direction
       repeatTime:          time period between the acquisition of the two optical images
       numberOfLines:       number of lines (in y direction)
       numberOfSamples:     number of samples (in x direction)
       
       ------------------input file names------------------
       demname:             (input; required) file name of the DEM
       dhdxname:            (input; not required) file name of the local surface slope in geographic x-coodinate
       dhdyname:            (input; not required) file name of the local surface slope in geographic y-coodinate
       vxname:              (input; not required) file name of the motion velocity in geographic x-coodinate
       vyname:              (input; not required) file name of the motion velocity in geographic y-coodinate
       
       ------------------output file names------------------
       winlocname:          (output) file name for the pixel indices (at each grid point)
       winoffname:          (output) file name of the pixel displacement (at each grid point)
       winro2vxname:        (output) file name of the conversion coefficients from pixel displacement to motion velocity in geographic x-coordinate (at each grid point)
       winro2vyname:        (output) file name of the conversion coefficients from pixel displacement to motion velocity in geographic y-coordinate (at each grid point)
       Note: "winoffname", "winro2vxname" and "winro2vyname" will be created only when "dhdxname", "dhdyname", "vxname", and "vyname" are provided

* After the above parameters are set, run the module as below to create the output files

       obj.geogrid() or obj.runGeogrid()

where "obj.geogrid()" is for radar data, and "obj.runGeogrid()" for optical data.
