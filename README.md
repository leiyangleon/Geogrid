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



## 4. [Demo](/docs/demo.md)





## 5. Install

Please refer to the installation guide of autoRIFT repository (https://github.com/leiyangleon/autoRIFT) for installing the Geogrid module.



## 6. [Instructions](/docs/instruction.md)



