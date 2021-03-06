# MatrixPilot: Connect a Serial Cable from the UAV Dev Board to a Computer

This document attempts to describe in detail how to establish a wired serial link from the UAV Devboard to a PC, for any of the "classic" UDBs. Wiring for a UDB4 is shown at the end.

Many of us want to have bi-directional communications with our UAV Devboard in a plane / helicopter. That is best established in two steps. Establish a link through a serial link to a PC using wires. Then, once that is working, attempt a more complex link using a wireless connection with a device such as an Xbee.

If you follow this route - then a number of us on in the discussion group can support you, as we have exactly the same equipment and setup which is:-

**One [USB Serial board](http://www.sparkfun.com/commerce/product_info.php?products_id=718) from sparkfun. Product Number:- BOB-00718 at Sparkfun.**

**a [USB cable](http://www.sparkfun.com/commerce/product_info.php?products_id=598) from that board to your PC. Product Number:- CAB-00598 at Sparkfun. These cables have been used with quite a few digital cameras in the past, so you may already have one.**

**Pins and Connectors. You must decide for yourself what connectors that you want to use and solder onto the boards.**

Then follow the wiring instructions for the 4 wires on the enclosed jpeg picture. Please note that you will have to de-solder a solder blob on the board - as shown in the picture (and shown on the Sparkfun [schematic](http://www.sparkfun.com/datasheets/BreakoutBoards/FT232RL-Breakout-Schematic.pdf) at location SJ2.)

![http://gentlenav.googlecode.com/svn/wiki/images/wiring_serial_link_v3.jpg](http://gentlenav.googlecode.com/svn/wiki/images/wiring_serial_link_v3.jpg)

You will then need the FTDI windows driver for the USB serial card. There are several types. I used the "Virtual Comms Port" drivers which present the USB serial card as an old fashioned Windows Comms Port - usually Comm 6 or 7 under Windows XP depending on what other devices you have installed. That driver can be found here on [this page at FTDI](http://www.ftdichip.com/Drivers/VCP.htm). I used the setup executable for Windows XP from that page. I've not linked to it directly as it is a .exe file.

After loading the drivers, doing the usual Windows reboot, I usually use "Start / Accessories / Communications / Hyper Terminal / udb.ht" under Windows to open a terminal session to the USB board and to the plane - remembering in my case, to set the correct COMMS port. Setting the port is achived under "File / Properties" "Connect Using: Comm 6" or what ever Comms port you think the USB serial port is on. If you are not sure, look at the list with the USB serial port plugged in. Then look at it again, with the USB serial port unplugged. Which ever COMMS port is now not listed in the "Connect Using" drop down menu, is the COMMS port of your USB serial port.

Finally, Once you've configured the COMM port name, configure also the baud rate by pressing "CONFIGURE". Set the following parameters: Baud Rate 19200, Data Bits 8, Parity None, Flow Control None.

Then in the UAV Devboard Matrix Pilot software you will need to select a serial ouput from within the option.h file.
i.e. one of

SERIAL\_DEBUG, SERIAL\_ARDUSTATION, SERIAL\_UDB, or SERIAL\_OSD\_REMZIBI

re-compile, re-load your firmware, boot the plane. Serial Output begins once messages are being received from the GPS unit. So you will want to make sure that your plane is receiving a clear signal from the GPS satellites.

The process for a serial connection to a UDB4 is identical, except for the pins that you will use on the UDB4. There are 4 pins in a row that you can use for the connection: GND, 3.3, RX2, and TX2. The main difference in labeling is that on the UDB4, the pin label is 3.3 instead of VCC.

Here are the connections for an example in which the UDB4 is powered from the FTDI:

http://gentlenav.googlecode.com/svn/wiki/images/UDB4FTDI.JPG