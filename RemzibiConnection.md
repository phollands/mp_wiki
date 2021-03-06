# MatrixPilot: Connect a Remzibi OSD to the UAV Dev Board

The Remzibi OSD overlays an information-rich heads-up-display on top of a video stream from the plane.  People often use this to give them virtual instruments while flyinga plane by watching a live video feed from the plane.

The Remzibi OSD usually needs a GPS plugged in to it, but since the UAV Dev Board has its own GPS, it seems wasteful of battery and weight to run a 2nd GPS for the OSD.

So MatrixPilot offers the ability to output NMEA formatted GPS data that can be read by the Remzibi OSD.

To enable this, just set the SERIAL\_OUTPUT\_FORMAT to SERIAL\_OSD\_REMZIBI in the options.h file.  Then connect the TX pin from the serial header on the UAVDevBoard to the RX pin on the GPS header on the OSD, and make sure the OSD gets power as usual, and shares a GND connection with the UAV Dev Board.

Also note that currently this only works using the [uBlox 5H GPS](http://store.diydrones.com/ProductDetails.asp?ProductCode=SPK-GPS-GS407).