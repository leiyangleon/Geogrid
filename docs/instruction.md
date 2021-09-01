## 6. Instructions


**Note:**

* For radar-coordinate imagery, it is recommended to run ISCE up to the step where co-registered SLC's are done, e.g. "mergebursts" for using topsApp.
* For map-projected geographic Cartesian-coordinate (e.g. optical) imagery, the images have to be co-registered with the same posting as well as the same x- and y-limits in map coordinates.
* The input DEM grid along with the auxilliary files have to be in the same map-projected geographic Cartesian coordinate projection (with x/y coordinates being easting/northing defined as distances in units of m); the geographic coordinate system (with latitude and longitude) is not supported. Users are recommended to do the GDAL coordinate transformation themselves before using Geogrid module.



**For quick use:**

_1. Radar-coordinate Imagery:_
* Refer to the file "testGeogrid_ISCE.py" (with ISCE) for the usage of the module and modify it for your own purpose
* Mandatory input files include: 

       "reference_image_folder" (-m option):     the coregistered reference image folder (required)
       "secondary_image_folder" (-s option):     the coregistered secondary image folder (required)
       "demname" (-d option):                    a DEM (required; in units of m)
  Optional inputs include:
       
       "dhdxname"/"dhdyname" (-sx/-sy option):                 x/y local surface slope maps (unitless)
       "vxname"/"vyname" (-vx/-vy option):                     x/y reference velocity maps (in units of m/yr)
       "srxname"/"sryname" (-srx/-sry option):                 x/y velocity search range maps (in units of m/yr)
       "csminxname"/"csminyname" (-csminx/-csminy option):     x/y chip size minimum maps (in units of m; constant ratio between x and y)
       "csmaxxname"/"csmaxyname" (-csmaxx/-csmaxy option):     x/y chip size maximum maps (in units of m; constant ratio between x and y)
       "ssmname" (-ssm option):                                stable surface mask (boolean)
* Output files may include all or some of the following (depending on the input fed in): 

       "winlocname":        the range/azimuth pixel indices (2-band; in units of integer image pixels), 
       "winoffname":        the downstream search (expected) range/azimuth pixel displacement (2-band; in units of integer image pixels), 
       "winsrname":         the range/azimuth search range (2-band; in units of integer image pixels), 
       "wincsminname":      the range/azimuth chip size minimum (2-band; in units of integer image pixels), 
       "wincsmaxname":      the range/azimuth chip size maximum (2-band; in units of integer image pixels), 
       "winssmname":        the stable surface mask (boolean), 
       "winro2vxname":      the 2-by-1 conversion coefficients from radar range/azimuth displacement to x-direction motion velocity (3-band; 3rd band is conversion coefficient from range pixel displacement to range motion velocity), 
       "winro2vyname":      the 2-by-1 conversion coefficients from radar range/azimuth displacement to y-direction motion velocity (3-band; 3rd band is conversion coefficient from azimuth pixel displacement to azimuth motion velocity). 
* For using Geogrid, a grid must be specified, which can be a real DEM for processing radar imagery and a dummy DEM (with all zero values) for optical imagery. Each of the rest optional input can be either used or omitted.

       input "demname"                                         -> output "winlocname"
* For full/combinative use of the optional inputs, please see below, where some ouputs may depend on multiple inputs and others may only depend on single input
                 
       input "dhdxname"/"dhdyname"                             -> output "winro2vxname"/"winro2vyname"
       input "dhdxname"/"dhdyname" and "vxname"/"vyname"       -> output "winro2vxname"/"winro2vyname" and "winoffname" 
       input "dhdxname"/"dhdyname" and "srxname"/"sryname"     -> output "winro2vxname"/"winro2vyname" and "winsrname"
       input "csminxname"/"csminyname"                         -> output "wincsminname"
       input "csmaxxname"/"csmaxyname"                         -> output "wincsmaxname"
       input "ssmname"                                         -> output "winssmname"
  _Note: "winlocname" will always be created, while the others will be generated contingent upon that the corresponding optional inputs are provided as above._       


_2. Map-projected Cartesian-coordinate Imagery:_
* Refer to the file "testGeogrid_ISCE.py" (with ISCE) and "testGeogridOptical.py" (standalone) for the usage of the module and modify it for your own purpose
* Mandatory input files include: 

       "reference_image_folder" (-m option):     the coregistered reference image (required)
       "secondary_image_folder" (-s option):     the coregistered secondary image (required)
       "demname" (-d option):                    a real or dummy (all-zero-value) DEM (required; in units of m)
  Optional inputs include:
       
       "dhdxname"/"dhdyname" (-sx/-sy option):                 x/y local surface slope maps (unitless; can be all-zero-value)
       "vxname"/"vyname" (-vx/-vy option):                     x/y reference velocity maps (in units of m/yr)
       "srxname"/"sryname" (-srx/-sry option):                 x/y velocity search range maps (in units of m/yr)
       "csminxname"/"csminyname" (-csminx/-csminy option):     x/y chip size minimum maps (in units of m; constant ratio between x and y)
       "csmaxxname"/"csmaxyname" (-csmaxx/-csmaxy option):     x/y chip size maximum maps (in units of m; constant ratio between x and y)
       "ssmname" (-ssm option):                                stable surface mask (boolean)
* Output files may include all or some of the following (depending on the input fed in): 

       "winlocname":        the horizontal/vertical pixel indices (2-band; in units of integer image pixels), 
       "winoffname":        the downstream search (expected) horizontal/vertical pixel displacement (2-band; in units of integer image pixels), 
       "winsrname":         the horizontal/vertical search range (2-band; in units of integer image pixels), 
       "wincsminname":      the horizontal/vertical chip size minimum (2-band; in units of integer image pixels), 
       "wincsmaxname":      the horizontal/vertical chip size maximum (2-band; in units of integer image pixels), 
       "winssmname":        the stable surface mask (boolean), 
       "winro2vxname":      the 2-by-1 conversion coefficients from horizontal/vertical displacement to x-direction motion velocity (2-band), 
       "winro2vyname":      the 2-by-1 conversion coefficients from horizontal/vertical displacement to y-direction motion velocity (2-band). 
* For using Geogrid, a grid must be specified, which can be a real DEM for processing radar imagery and a dummy DEM (with all zero values) for optical imagery. Each of the rest optional input can be either used or omitted.

       input "demname"                                         -> output "winlocname"
* For full/combinative use of the optional inputs, please see below, where some ouputs may depend on multiple inputs and others may only depend on single input
                 
       input "dhdxname"/"dhdyname"                             -> output "winro2vxname"/"winro2vyname"
       input "dhdxname"/"dhdyname" and "vxname"/"vyname"       -> output "winro2vxname"/"winro2vyname" and "winoffname" 
       input "dhdxname"/"dhdyname" and "srxname"/"sryname"     -> output "winro2vxname"/"winro2vyname" and "winsrname"
       input "csminxname"/"csminyname"                         -> output "wincsminname"
       input "csmaxxname"/"csmaxyname"                         -> output "wincsmaxname"
       input "ssmname"                                         -> output "winssmname"
  _Note: "winlocname" will always be created, while the others will be generated contingent upon that the corresponding optional inputs are provided as above._


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
       incidenceAngle:      local incidence angle of radar wave
       epsg:                EPSG code for DEM map projection
       
       ------------------Map-projected Cartesian-coordinate imagery parameters (for optical only)------------------
       startingX:           starting coordinate in x direction
       startingY:           starting coordinate in y direction
       XSize:               resolution in x direction
       YSize:               resolution in y direction
       repeatTime:          time period between the acquisition of the two optical images
       numberOfLines:       number of lines (in y direction)
       numberOfSamples:     number of samples (in x direction)
       epsgDem:             EPSG code for DEM map projection
       epsgDat:             EPSG code for image data map projection
       
       ------------------MISC parameters (preparation for autoRIFT)------------------
       nodata_out:          nodata value in the output
       chipSizeX0:          Smallest chip size allowed (in m)
       gridSpacingX:        Grid spacing in X direction (in m)
       _xlim:               x coordinate limits (in m)
       _ylim:               y coordinate limits (in m)
       pOff:                Grid starting pixel index in horizontal direction (in integer pixels)
       lOff:                Grid starting line index in vertical direction (in integer pixels)
       pCount:              number of grid pixels in horizontal direction (in integer pixels)
       lCount:              number of grid lines in vertical direction (in integer pixels)
       X_res:               grid posting in horizontal direction (in m)
       Y_res:               grid posting in vertical direction (in m)
       
       ------------------input file names------------------
       demname:             (input; required) file path/name of the DEM
       dhdxname:            (input; not required) file path/name of the local surface slope in x-coodinate (easting)
       dhdyname:            (input; not required) file path/name of the local surface slope in y-coodinate (northing)
       vxname:              (input; not required) file path/name of the motion velocity (in m/yr) in x-coodinate (easting)
       vyname:              (input; not required) file path/name of the motion velocity (in m/yr) in y-coodinate (northing)
       srxname:             (input; not required) file path/name of the velocity search range (in m/yr) in x-coodinate (easting)
       sryname:             (input; not required) file path/name of the velocity search range (in m/yr) in y-coodinate (northing)
       csminxname:          (input; not required) file path/name of the chip size minimum (in m) in horizontal direction
       csminyname:          (input; not required) file path/name of the chip size minimum (in m) in vertical direction
       csmaxxname:          (input; not required) file path/name of the chip size maximum (in m) in horizontal direction
       csmaxyname:          (input; not required) file path/name of the chip size maximum (in m) in vertical direction
       ssmname:             (input; not required) file path/name of the stable surface mask
       
       
       ------------------output file names------------------
       winlocname:          (output) file path/name of the 2-band (in image horizontal and vertical direction) pixel indices (at each grid point)
       winoffname:          (output) file path/name of the 2-band (in image horizontal and vertical direction) downstream search (expected) pixel displacement (at each grid point)
       winsrname:           (output) file path/name of the 2-band (in image horizontal and vertical direction) pixel search range (at each grid point)
       wincsminname:        (output) file path/name of the 2-band (in image horizontal and vertical direction) chip size minimum in pixels (at each grid point)
       wincsmaxname:        (output) file path/name of the 2-band (in image horizontal and vertical direction) chip size maximum in pixels (at each grid point)
       winssmname:          (output) file path/name of the stable surface mask (at each grid point)
       winro2vxname:        (output) file path/name of the 2-band (in image horizontal and vertical direction) or 3-band (for radar only) conversion coefficients from pixel displacement to x-direction (easting) motion velocity (at each grid point)
       winro2vyname:        (output) file path/name of the 2-band (in image horizontal and vertical direction) or 3-band (for radar only) conversion coefficients from pixel displacement to y-direction (northing) motion velocity (at each grid point)
       
       
* For using Geogrid, a grid must be specified, which can be a real DEM for processing radar imagery and a dummy DEM (with all zero values) for optical imagery. Each of the rest optional input can be either used or omitted.

       input "demname"                                         -> output "winlocname"
* For full/combinative use of the optional inputs, please see below, where some ouputs may depend on multiple inputs and others may only depend on single input
                 
       input "dhdxname"/"dhdyname"                             -> output "winro2vxname"/"winro2vyname"
       input "dhdxname"/"dhdyname" and "vxname"/"vyname"       -> output "winro2vxname"/"winro2vyname" and "winoffname" 
       input "dhdxname"/"dhdyname" and "srxname"/"sryname"     -> output "winro2vxname"/"winro2vyname" and "winsrname"
       input "csminxname"/"csminyname"                         -> output "wincsminname"
       input "csmaxxname"/"csmaxyname"                         -> output "wincsmaxname"
       input "ssmname"                                         -> output "winssmname"
  _Note: "winlocname" will always be created, while the others will be generated contingent upon that the corresponding optional inputs are provided as above._  

* After the above parameters are set, run the module as below to create the output files

       obj.geogrid() or obj.runGeogrid()

  where "obj.geogrid()" is for radar-coordinate imagery, and "obj.runGeogrid()" for map-projected Cartesian-coordinate (optical) imagery.
