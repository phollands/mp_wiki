# Introduction

Many new features are developed in the trunk code.  They are normally defaulted off to keep compatibility with the resource limited UDB3.  This document describes the features and how to activate them.


## ACTIVATING A FEATURE
Most features have a defined value in an xx\_options.h file.  This is our way of isolating code changes from the main track.  To switch the feature on, you need to find the right options file and modify the defined value.


---


## Non volatile memory - The EEPROM
This enables UDB to remember settings without needing to recompile and download.  You MUST be using SERIAL\_MAVLINK to use this feature.
  1. Set to mavlink telemetry here: http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/options.h?r=1616#324
  1. Switch on the I2C driver support that communicates with the eeprom. http://code.google.com/p/gentlenav/source/browse/trunk/libUDB/nv_memory_options.h?r=1616#9
  1. Switch on the non volatile memory here: http://code.google.com/p/gentlenav/source/browse/trunk/libUDB/nv_memory_options.h?r=1616#23

Description of I2C and EEPROM drivers:
https://docs.google.com/open?id=0B5SF4VCMDng5YUU5UDVkSjFlWms

Description of parameter storage in nv memory:
https://docs.google.com/open?id=0B5SF4VCMDng5QWtYY3RMWnpHN0U




---



## Parameters, pyparam and the parameter database
MAVlink can be used to modify parameters on UDB.  Parameters are settings that can be always be read and written.  Parameters can also be stored in nv memory.  For a full description of how they work, read the mavlink documentation here:
http://qgroundcontrol.org/mavlink/start

The parameter database is an XML file describing all of the parameters, how they connect to variables in MatrixPilot and how they are organised in the eeprom.

pyparam is a python script that turns the database into the c code and data tables required for MatrixPilot.


---


## Variable gains
To save space, many values are constants instead of ram based variables.
Some constants automatically become variables when mavlink is used.  Others need to be switched on.

**Height gains**

http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/gain_variables.h?r=1616#12
Note that this also includes airspeed gains.  The aircraft will behave in a different way to non variable.  Take a look at the airspeed options here:

http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/airspeed_options.h?r=1616#22


**declination angle**

http://code.google.com/p/gentlenav/source/browse/trunk/libUDB/magnetometerOptions.h?r=1616#35

**magnetometer calibration**

This is an option but not part of the defines.  If you store the magnetometer calibration, this calibration will be recalled from nv memory at startup.



---


## Variable HILSIM channels
for those needing more than the standard 8 channels, there is a HILSIM option here:
http://code.google.com/p/gentlenav/source/browse/trunk/libUDB/nv_memory_options.h?r=1616#45
Please note that you will need a HILSIM plugin build to support this feature.


---


## Flexifunction mixing
This is an option for anything you want mixing.

http://code.google.com/p/gentlenav/source/browse/trunk/libUDB/nv_memory_options.h?r=1616#39

It has a library associated with it.  At this time, the library is located int the MatrixPilot\_FlexiFunctions branch here:

http://code.google.com/p/gentlenav/source/browse/#svn%2Fbranches%2FMatrixPilot_FlexiFunctions%2FlibFlexiFunctions


There is an editor that goes with this.  Read the wiki on the project page here:

http://code.google.com/p/pyfedit/wiki/Home?tm=6