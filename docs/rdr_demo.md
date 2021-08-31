### 4.1 Radar Demo

<img src="../figures/optical1.png" width="50%">

***Test area and dataset: optical image over the Jakobshavn glacier where the red rectangle marks boundary of the Sentinel-1A/B image pair (20170221-20170227). Input files in this test scenario consist of the Digital Elevation Model (DEM), local surface slope maps (in both x- and y-direction) and motion velocity maps (in both x- and y-direction) over the entire Greenland, where all maps share the same map-projected geographic Cartesian (northing/easting) coordinate grid with 240-m spacing and spatial reference system of EPSG code 3413 (a.k.a WGS 84 / NSIDC Sea Ice Polar Stereographic North).***



<img src="../figures/geogrid.png" width="100%">

***Output of "Geogrid" module: (a) range pixel index at each grid point, (b) azimuth pixel index at each grid point, (c) expected range pixel displacement at each grid point, (d) expected azimuth pixel displacement at each grid point. Note: only the portion of the grid overlapping with the radar image pair has been extracted and shown.***

This is obtained by implementing the following command line:

With ISCE:

       testGeogrid_ISCE.py -m reference_image_folder -s secondary_image_folder -d demname -sx dhdxname -sy dhdyname -vx vxname -vy vyname -srx srxname -sry sryname -csminx csminxname -csminy csminyname -csmaxx csmaxxname -csmaxy csmaxyname -ssm ssmname

where "reference_image_folder" and "secondary_image_folder" are the folders storing coregistered reference and secondary image information (e.g. radar parameters), and "demname", "dhdxname", "dhdyname", "vxname", "vyname", "srxname", "sryname", "csminxname", "csminyname", "csmaxxname", "csmaxyname" and "ssmname" are optional inputs that are defined below in the section of instructions.


Using the matrix of conversion coefficients from the Geogrid outputs, when range/azimuth fine pixel displacement are estimated from radar data using autoRIFT, they can be immediately converted to motion velocity in northing/easting-direction. See the final result below by using the matrix of conversion coefficients from the "Geogrid" module and the radar-estimated fine pixel displacement from the "autoRIFT" module (https://github.com/nasa-jpl/autoRIFT).


<img src="../figures/autorift2.png" width="100%">

***Final motion velocity results by combining outputs from "Geogrid" (i.e. matrix of conversion coefficients) and "autoRIFT" modules (i.e. estimated range/azimuth pixel displacement from the Demo at https://github.com/nasa-jpl/autoRIFT): (a) estimated motion velocity from Sentinel-1 data (x-direction; in m/yr), (b) reference motion velocity from input data (x-direction; in m/yr), (c) estimated motion velocity from Sentinel-1 data (y-direction; in m/yr), (d) reference motion velocity from input data (y-direction; in m/yr). Notes: all maps are established exactly over the same map-projected Cartesian (northing/easting) coordinate grid from input.***
