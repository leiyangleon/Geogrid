# Geogrid

### Update Notes:

```diff
+ Note from now on, the two testGeogrid scripts (testGeogridOptical.py and testGeogrid_ISCE.py) are only 
+  hosted on the sister module autoRIFT's GitHub page (https://github.com/leiyangleon/autoRIFT). 
+  Thus, they have been removed from this website.
```

**A Python module for precise mapping between (pixel index, pixel displacement) in imaging coordinates and (geolocation, motion velocity) in geographic Cartesian (northing/easting) coordinates**

**Geogrid can be installed as a standalone Python module (only supports Cartesian coordinates) either manually or as a conda install (https://github.com/conda-forge/autorift-feedstock). To allow support for both Cartesian and radar coordinates, Geogrid must be installed with the InSAR Scientific Computing Environment (ISCE: https://github.com/isce-framework/isce2)**

**Geogrid can be used for dense feature tracking between two images over a grid defined in an arbitrary geographic Cartesian (northing/easting) coordinate projection when used in combination with the sister autoRIFT Python module (https://github.com/leiyangleon/autoRIFT). Example applications include searching radar-coordinate imagery on a polar stereographic grid and searching Universal Transverse Mercator (UTM) imagery at a specified geographic Cartesian (northing/easting) coordinate grid**



Copyright (C) 2019 California Institute of Technology.  Government Sponsorship Acknowledged.

Link: https://github.com/leiyangleon/Geogrid



## 1. Authors


Piyush Agram (JPL/Caltech; piyush.agram@jpl.nasa.gov), Yang Lei (GPS/Caltech; ylei@caltech.edu)

## 2. Acknowledgement

This effort was funded by the NASA MEaSUREs program in contribution to the Inter-mission Time Series of Land Ice Velocity and Elevation (ITS_LIVE) project (https://its-live.jpl.nasa.gov/) and through Alex Gardner’s participation in the NASA NISAR Science Team
       
       
## 3. [Features](/docs/features.md)



## 4. Demo

[_4.1 Radar Demo_](/docs/rdr_demo.md)





[_4.2 Optical Demo_](/docs/opt_demo.md)



## 6. Install

Please refer to the installation guide of autoRIFT repository (https://github.com/leiyangleon/autoRIFT) for installing the Geogrid module.



## 5. Instructions


**Note:**

* For radar-coordinate imagery, it is recommended to run ISCE up to the step where co-registered SLC's are done, e.g. "mergebursts" for using topsApp.
* For Cartesian-coordinate imagery, the images have to be co-registered with the same posting as well as the same x- and y-limits in map coordinates.
* The input DEM grid along with the auxilliary files have to be in the geographic Cartesian coordinate projection (with geographic x, y coordinates being easting, northing defined as distances in units of m); the geographic coordinate system (with latitude and longitude) is not supported. Users are recommended to do the GDAL coordinate transformation themselves before using the current version of the Geogrid module.



**For quick use:**

_Radar-coordinate Imagery:_
* Refer to the file "testGeogrid_ISCE.py" (with ISCE) for the usage of the module and modify it for your own purpose
* Input files include the master image folder (required), slave image folder (required), a DEM (required; in units of m), local surface slope maps (unitless), velocity maps (in units of m/yr), velocity search range maps (in units of m/yr), chip size minimum/maximum maps (in units of m), stable surface mask.
* Output files include 1) the range and azimuth pixel indices (in units of integer image pixels), 2) the range and azimuth pixel displacement (in units of integer image pixels), 3) the range and azimuth search range (in units of integer image pixels), 4) the range and azimuth chip size minimum (in units of integer image pixels), 5) the range and azimuth chip size maximum (in units of integer image pixels), 6) the stable surface mask (boolean), 7) the conversion coefficients from radar range and azimuth displacement to motion velocity in geographic x-coordinate, and 8) the conversion coefficients from radar range and azimuth displacement to motion velocity in geographic y-coordinate. 

_Cartesian-coordinate Imagery:_
* Refer to the file "testGeogrid_ISCE.py" (with ISCE) and "testGeogridOptical.py" (standalone) for the usage of the module and modify it for your own purpose
* Input files include the image 1 (required), image 2 (required), a DEM (required; in units of m), local surface slope maps (unitless), velocity maps (in units of m/yr)
* Output files include 1) the horizontal and vertical pixel indices (in units of integer image pixels), 2) the horizontal and vertical pixel displacement (in units of integer image pixels), 3) the horizontal and vertical search range (in units of integer image pixels), 4) the horizontal and vertical chip size minimum (in units of integer image pixels), 5) the horizontal and vertical chip size maximum (in units of integer image pixels), 6) the stable surface mask (boolean), 3) the conversion coefficients from horizontal and vertical displacement to motion velocity in geographic x-coordinate, and 4) the conversion coefficients from horizontal and vertical displacement to motion velocity in geographic y-coordinate. 

_Note: among these, 1) will always be created, while 2) and 7-8) will be generated contingent upon that local surface slope and velocity maps are provided. The rest, i.e. 3-6) will be created only when the corresponding inputs are provided._

**For modular use:**

* In Python environment, type the following to import the "Geogrid" module and initialize the "Geogrid" object

_With ISCE:_

       import isce
       from contrib.geo_autoRIFT.geogrid import Geogrid, GeogridOptical
       obj = Geogrid() or obj = GeogridOptical()
       obj.configure()

_Standalone:_

       from geogrid import GeogridOptical
       obj = GeogridOptical()

where "Geogrid()" is for radar-coordinate imagery and "GeogridOptical()" for Cartesian-coordinate imagery.


* The "Geogrid" object has several parameters that have to be set up (listed below; can also be obtained by referring to "testGeogrid_ISCE.py"): 

       ------------------radar-coordinate imagery parameters (for radar only)------------------
       startingRange:       starting range
       rangePixelSize:      range pixel size
       sensingStart:        starting azimuth time
       prf:                 pulse repition frequency 
       lookSide:            look side, e.g. -1 for right looking 
       repeatTime:          time period between the acquisition of the two radar images
       numberOfLines:       number of lines (in azimuth)
       numberOfSamples:     number of samples (in range)
       orbit:               ISCE orbit data structure
       
       ------------------Cartesian-coordinate imagery parameters (for Cartesian only)------------------
       startingX:           starting coordinate in x direction
       startingY:           starting coordinate in y direction
       XSize:               resolution in x direction
       YSize:               resolution in y direction
       repeatTime:          time period between the acquisition of the two optical images
       numberOfLines:       number of lines (in y direction)
       numberOfSamples:     number of samples (in x direction)
       
       ------------------MISC------------------
       nodata_out:          nodata value in the output
       chipSizeX0:          Smallest chip size allowed (in m), which only gets used when the chip size minimum and maximum are provided.
       
       ------------------input file names------------------
       demname:             (input; required) file path/name of the DEM
       dhdxname:            (input; not required) file path/name of the local surface slope in geographic x- (easting) coodinate
       dhdyname:            (input; not required) file path/name of the local surface slope in geographic y- (northing) coodinate
       vxname:              (input; not required) file path/name of the motion velocity in geographic x- (easting) coodinate
       vyname:              (input; not required) file path/name of the motion velocity in geographic y- (northing) coodinate
       srxname:             (input; not required) file path/name of the velocity search range in geographic x- (easting) coodinate
       sryname:             (input; not required) file path/name of the velocity search range in geographic y- (northing) coodinate
       csminxname:          (input; not required) file path/name of the chip size minimum (in m) in geographic x- (easting) coodinate
       csminyname:          (input; not required) file path/name of the chip size minimum (in m) in geographic y- (northing) coodinate
       csmaxxname:          (input; not required) file path/name of the chip size maximum (in m) in geographic x- (easting) coodinate
       csmaxyname:          (input; not required) file path/name of the chip size maximum (in m) in geographic y- (northing) coodinate
       ssmname:             (input; not required) file path/name of the stable surface mask
       
       
       ------------------output file names------------------
       winlocname:          (output) file name for the two-band (in x and y-direction) pixel indices (at each grid point)
       winoffname:          (output) file name of the two-band (in x and y-direction) pixel displacement (at each grid point)
       winsrname:           (output) file name of the two-band (in x and y-direction) pixel search range (at each grid point)
       wincsminname:        (output) file name of the two-band (in x and y-direction) chip size minimum in pixels (at each grid point)
       wincsmaxname:        (output) file name of the two-band (in x and y-direction) chip size maximum in pixels (at each grid point)
       winssmname:          (output) file name of the stable surface mask (at each grid point)
       winro2vxname:        (output) file name of the two-band (in x and y-direction) conversion coefficients from pixel displacement to motion velocity in geographic x-coordinate (at each grid point)
       winro2vyname:        (output) file name of the two-band (in x and y-direction) conversion coefficients from pixel displacement to motion velocity in geographic y-coordinate (at each grid point)
       
       
       Note: all the above outputs will be created. However, when "dhdxname" and "dhdyname" are not provided, "winoffname", "winro2vxname" and "winro2vyname" will be nodata everywhere; when "vxname", and "vyname" are not provided, "winoffname" will be nodata everywhere; only when all of "dhdxname", "dhdyname", "vxname", and "vyname" are provided, these four outputs ("winlocname", "winoffname", "winro2vxname" and "winro2vyname") have meaningful values. In addition, "winsrname" is meaningful when "srname" is provided; "wincsminname" is meaningul when "csminxname" and "csminyname" are provided; "wincsmaxname" is meaningul when "csmaxxname" and "csmaxyname" are provided; "winssmname" is meaningful when "ssmname" is provided. Otherwise, there will be nodata values as assigned by "nodata_out" everywhere in the outputs.

* After the above parameters are set, run the module as below to create the output files

       obj.geogrid() or obj.runGeogrid()

where "obj.geogrid()" is for radar-coordinate imagery, and "obj.runGeogrid()" for Cartesian-coordinate imagery.
