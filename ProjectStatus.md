# Project Status

### What's changed in MatrixPilot 4.1
  * New makefile build system
  * New project generator python script
  * New X-Plane plugin keyboard capture for flight controls
  * Support for external flash memory devices
  * Improved internal logfile support
  * Updated MAVLink release
  * Numerous bugfixes...

### What's New in MatrixPilot 4.0
  * New features:
    * Added support for SparkFun's UDB5 board
    * Added support for Nick Arsov's AUAV3 board
    * Improved cross-tracking performance
    * Improved navigation
    * Lower power consumption by the dsPIC, due to sleeping while idle
    * More details to come...

### What's New in MatrixPilot 3.3
  * New features:
    * Added support for Nick Arsov's UDB3 clone
    * Improved LOGO interpreter to allow using conditionals
    * Improvedd LOGO interpreter by adding a simple interrupt facility
    * Added bi-directional telemetry protocol - SERIAL\_MAVLINK.
    * Added glide airspeed control with MavLink parameters.
    * Added variable altitude and airspeed options with MavLink parameters and non-volatile memory storage.
    * Improved UDB4 timing to allow faster baud rates.
    * Allow stabilization mode of camera to be determined from a three position switch on the transmitter by defining CAMERA\_MODE\_INPUT\_CHANNEL .
    * Provide an option to initialize the aircraft in the vertical position. This is for VTOL.
    * Added ROLLKD\_RUDDER feedback parameter to dampen Dutch-Roll. Rudder deflection term proportional to roll rate
    * Added a new orientation option for the magnetometer : direct connection to a UDB4
    * Update the Waypoint Editor to more closely match the UDB Logo Editor, including cross-browser compatibility fixes.
    * Added non volatile memory services, used by MavLink
    * Flight Analyzer Improvements:
      * Added Logo Waypoints processing and display to Flight Analyzer.
      * Improve robustness against corrupted lines.
      * Implemented a Graphical Elevation Profile for each flight in Google Earth.
      * Will decode and write legacy ascii SERIAL\_UDB\_EXTRA files from MatrixPilot MavLink telemetry.
      * Added XML time column to CSV file
      * Update flan serial\_udb\_extra parser to understand new F2: format for in-line waypoint data.
      * Racing: re-base time automatically so all race flights pass the race start pylon at midday exactly on 11/11/2011
  * Bug Fixes:
    * Improved the failsafe code to make it less sensitive to an occasional invalid pulse.
    * Magnetometer : fixed a nasty bug in the UDB4 mag driver that would change the accelerometer range
    * Fix a bug in REL\_ANGLE\_TO\_HOME/REL\_ANGLE\_TO\_GOAL that returned values in the range of -360-359 instead of -180-179
    * Fix NORTH, SOUTH, EAST, WEST commands in Logo Editor
    * Fix flight analyser to correctly work with older format of SERIAL\_UDB
    * Correct CPU\_LOAD\_PERCENT, for the UDB4, which was under-reading cpu load by 5 percent.
    * Fix bugs in NORADIO option to allow GPS acquisition to occur and allow red LED to flash during gyro calibration


### What's New in MatrixPilot 3.2.1
  * Fixed a bug that could cause the rudder channel to pull to one side while in RTL mode when using an RC receiver without failsafe.
  * Added magnetometer support for the UDB4.


### What's New in MatrixPilot 3.2
  * Fixed support for new DIYDrones magnetometer.
  * Allow using spare analog inputs for voltage, current, rssi for use with the OSD.
  * New Ubuntu 10.04 HILSIM plugin; Updated the prebuilt HILSIM windows plugin.
  * Variometer improvements and total-energy calculation.
  * Improvements to flan, the Flight Analyzer.
  * Disabled wind gain adjustment when flying into the wind.
  * IMU velocity improvements (noticeably better in the OSD).
  * OSD responsiveness improvements (USE IMU instead of GPS course info).
  * Improved altitude hold's use of Min/Max alt.
  * Added compile-time testing of options.h settings to reduce config mistakes.
  * Cleaned up options.h a bit (removed CLOCK\_CONFIG is the big one).
  * New options for board mounting orientation: YAWCW, YAWCCW.


### What's New in MatrixPilot 3.1
  * Major IMU improvements, including "dizzy-proofing", automatic inflight gyro calibration, and compensation for magnetometer latency.
  * Live camera tracking of one UDB from another.
  * User-configurable OSD layouts.
  * Full support for the DIYDrones MediaTek GPS.
  * Maintain minimum ground speed. The speed control loop is now based on the smaller of ground speed and air speed.
  * Improvements to inverted stabilization.
  * Improvements to the Flight Analyzer tool.
  * More bug fixes.


### What's New in MatrixPilot 3.0
  * Support for up to 8-channel PPM RC Input on a single pin, freeing up other pins for other features, including up to 9 RC outputs.  Compatible with [this board from the DIYDrones store](http://store.diydrones.com/product_p/br-ppme.htm).
  * Added a new mission description language based on LOGO.
  * Allow sending waypoints (or UDB Logo instructions) mid flight over the telemetry channel (but no ground-station support yet).
  * Much improved Navigation, due to improvements in Dead Reckoning and wind estimation.
  * Altitude control now accounts for energy due to airspeed, and adjusts throttle and pitch to maintain the desired total energy.
  * Improved camera-targeting logic and precision.
  * Added an RTL Hold feature that keeps you in RTL mode once it's been triggered, until you flip your mode switch.  This helps avoid chaos if you're flying at the edge of your RC range.
  * Support for a still-experimental native On-Screen Display using [this board from SparkFun](http://www.sparkfun.com/products/9168).
  * Improved autonomous landing control.
  * Improved flight analysis tools.
  * Code restructuring into shared libraries makes building your own UDB-based projects much easier!


### What's New in MatrixPilot 2.5

  * IMU based "dead" reckoning.
  * Support for waypoints at absolute positions, relative to startup, or relative to a fixed location.
  * More configurable waypoints / flight patterns.
  * Stabilize inverted flight.
  * Stabilize vertical / hovering flight.
  * Beginnings of automatic landing.
  * Camera stabilization and targeting.
  * Support for automatic detection of, and adjustment for average wind speed and direction.
  * Support for using a magnetometer for yaw stabilization (using the I2C interface)
  * Improve robustness of waypoint following when losing the transmitter signal.
  * Hardware-In-the-Loop simulation using a UDB and X-Plane.


## What's New in MatrixPilot 2.0

  * Combines the functionality of MatrixNav and AileronAssist into a single firmware.
  * Supports navigation using both ailerons and rudder.
  * Increases the number of Radio Input channels (from 4 to 5), and Servo Output channels (from 3 to 6).
  * Supports controlling Delta wing and VTail airframes, in addition to Standard airframes with or without ailerons.
  * More responsive to switching between Manual, Stabilized, and Waypoint modes.
  * Supports using the new uBlox GPS for faster navigation responses.
  * Super-basic camera stabilization code.
  * Add serial telemetry output in a variety of formats.
    * Supports telemetry output in our own UAV Dev Board format.
    * Supports telemetry output using an Ardupilot Ground Station compatible format.
    * Supports NMEA output to a Remzibi OSD.


## Future Roadmap

As with all long term plans, these could change at any time, but here's where we're thinking we'll go in the near future.


### Future Ideas

  * Fully rotational based controls, using the complete theory proposed by Robert Mahoney.
  * Real Helicopter and Quadcopter support.
  * Barometric pressure sensor for more precise altitude control.
  * ...