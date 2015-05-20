![http://gentlenav.googlecode.com/svn/wiki/images/mavlink.png](http://gentlenav.googlecode.com/svn/wiki/images/mavlink.png)

# Introduction

MAVLink is a standard UAV/MAV (micro air vehicle) communication protocol.  It is the way that the common GCS communicate.  It is bi-directional so communication to and from the mav is possible.  It carries information about mav position, orientation and condition.  It also carries commands and command responses.

MAVLink is invented and maintained by Lorenz Meyer, James Goppert and Tridge.  The main website is here:

http://www.qgroundcontrol.org/mavlink/start


MAVLink is only supported by UDB4+´.  UDB3- will not work.



# How to use MAVLink with UDB4+

In [options.h](http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/options.h#335) set the following line:

`#define SERIAL_OUTPUT_FORMAT 	SERIAL_MAVLINK`

if you have several aircraft, you may wish to change the identifier for each one one this line:

`#define MAVLINK_SYSID	55`

There are acceptable ranges of system ID which are defined in the mavlink website

Recompile with these options then connect to either a groundstation or a binary compatible datalogger.

# MAVLink options

Some extra options for mavlink can be setup in mavlink\_options.h

### Serial baud rate

The serial baud rate can be changed here:

#define MAVLINK\_BAUD		57600
MAVLink is tested at 57600 baud and so this is the default.
There is a range of choices as shown in [mavlink\_options.h ](http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/mavlink_options.h#22)

### Data streams
MAVLink automatically transmits different data at different data rates.  The default data rates are set in mavlink\_options.h.  These data rates can be overridden a mavlink message from the gcs.


# MAVLink and SERIAL\_UDB\_EXTRA
The MatrixPilot implementation of mavlink incorporates all of the SERIAL\_UDB\_EXTRA messages within MAVLink. The idea is to allow an easy transition path for users of SERIAL\_UDB\_EXTRA to MAVLink. If MAVLink is stored on an OpenLog device, then when the LOG.txt file is presented to Flight Analyzer it will a) process the mavlink binary LOG.txt in the normal way to produce a CSV file and a KMZ file for Google Earth. It will then also write out a human readable ascii version of the SERIAL\_UDB\_EXTRA data for complete backwards compatibility. Users of an OpenLog with MAVLink need tobe sure the openlog firmware is upgraded to support binary files. OpenLog storage in the plane, and Xbee bi-directional communications with a mavlink GCS can be used simultaneously at the same time.


# MAVLink parameters
Parameters are variables that you wish to change through the gcs.  These variables can be gains, calibrations, rc trims and more.


# Groundstations
At this time, MatrixPilot supports MAVLink V1.0. QgroundControl and MAVproxy both work. HKGCS will not work as it only supports the earlier version of MAVLink V0.9.


### qgroundcontrol
The most complex and feature rich GCS.  This is the best GCS to start and test your mavlink connection.

Get it here:
http://www.qgroundcontrol.org/downloads


QGC will edit parameters and also let you store/recall parameters to/from non volatile memory (eeprom).  MatrixPilot has some specific QGC widget to do this.  You will find them in Tools->pyparam->QGC Widgets.


### mavproxy

This is a command line based gcs. A feature of mavproxy is that it can connect your mav with many gcs over ip or udp connections.  Another feature is voice generation.

The latest version of mavproxy has some excellent add-on modules including graphical interfaces. MAVProxy works with both Linux and Windows (and shoule work with MacOS) You will find the latest mavproxy here:

https://github.com/tridge

and mavlink here:
https://github.com/mavlink/mavlink

A tested version of MAVProxy is included with MatrixPilot in the Tools/mavlink/MAVProxy directory.

### pyparameditor
A simple parameter viewer and editor specific to UDB.  It gives simple access to store/recall/clear data files/areas.  The source code can be found here: http://code.google.com/p/pyparameditor/


### pyFEdit - FlexiFunction mixer editor

pyFEdit can do a real time MAVLink connection to mixer settings.  It is available here:
http://code.google.com/p/pyfedit/wiki/Home?tm=6




# Acknowledgements
MAVlink implementation on UDB and the supporting flan toolset was the  work of Pete Hollands and Matt Coleman.