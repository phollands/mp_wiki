# Magnetometer Connections

Magnetometer connections and mounting orientations for a "classic" UDB are described [here](http://gentlenav.googlecode.com/files/MagnetometerDemo.pdf).

For the UDB4, configure the magnetometer for 3.3 volts, and connect the ground, VCC, SCL, and SDA lines on the magnetometer to the GND, 3.3, SCL, and SDA pins on the UDB4.

In addition to the same mounting options supported by the classic UDBs, the UDB4 has an additional mounting option for connecting the magnetometer directly to the UDB4. Select the MAG\_DIRECT orientation option in the magnetometerOptions.h file, and connect the magnetometer as shown below.

**Note:** Presently, you can only use the MAG\_DIRECT orientation if you are using the ORIENTATION\_FORWARDS for the UDB.

http://gentlenav.googlecode.com/svn/wiki/images/MagDirectView1.JPG
http://gentlenav.googlecode.com/svn/wiki/images/MagDirectView2.JPG