## 6. Instructions


**Note:**

* For radar-coordinate imagery, it is recommended to run ISCE up to the step where co-registered SLC's are done, e.g. "mergebursts" for using topsApp.
* For map-projected geographic Cartesian-coordinate (e.g. optical) imagery, the images have to be co-registered with the same posting as well as the same x- and y-limits in map coordinates.
* The input DEM grid along with the auxilliary files have to be in the same map-projected geographic Cartesian coordinate projection (with x/y coordinates being easting/northing defined as distances in units of m); the geographic coordinate system (with latitude and longitude) is not supported. Users are recommended to do the GDAL coordinate transformation themselves before using Geogrid module.



**For quick use:**

_Radar-coordinate Imagery:_
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

       "winlocname":        the range and azimuth pixel indices (2-band; in units of integer image pixels), 
       "winoffname":        the downstream search (expected) range and azimuth pixel displacement (2-band; in units of integer image pixels), 
       "winsrname":         the range and azimuth search range (2-band; in units of integer image pixels), 
       "wincsminname":      the range and azimuth chip size minimum (2-band; in units of integer image pixels), 
       "wincsmaxname":      the range and azimuth chip size maximum (2-band; in units of integer image pixels), 
       "winssmname":        the stable surface mask (boolean), 
       "winro2vxname":      the 2-by-1 conversion coefficients from radar range and azimuth displacement to x-direction motion velocity (3-band; 3rd band is conversion coefficient from range pixel displacement to range motion velocity), 
       "winro2vyname":      the 2-by-1 conversion coefficients from radar range and azimuth displacement to y-direction motion velocity (3-band; 3rd band is conversion coefficient from azimuth pixel displacement to azimuth motion velocity). 
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


_Map-projected Cartesian-coordinate Imagery:_
* Refer to the file "testGeogrid_ISCE.py" (with ISCE) and "testGeogridOptical.py" (standalone) for the usage of the module and modify it for your own purpose
* Input files include the image 1 (required), image 2 (required), a DEM (required; in units of m), local surface slope maps (unitless), velocity maps (in units of m/yr)
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

       "winlocname":        the horizontal and vertical pixel indices (2-band; in units of integer image pixels), 
       "winoffname":        the downstream search (expected) horizontal and vertical pixel displacement (2-band; in units of integer image pixels), 
       "winsrname":         the horizontal and vertical search range (2-band; in units of integer image pixels), 
       "wincsminname":      the horizontal and vertical chip size minimum (2-band; in units of integer image pixels), 
       "wincsmaxname":      the horizontal and vertical chip size maximum (2-band; in units of integer image pixels), 
       "winssmname":        the stable surface mask (boolean), 
       "winro2vxname":      the 2-by-1 conversion coefficients from horizontal and vertical displacement to x-direction motion velocity (2-band), 
       "winro2vyname":      the 2-by-1 conversion coefficients from horizontal and vertical displacement to y-direction motion velocity (2-band). 
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
