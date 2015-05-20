# MatrixPilot-SIL

Software In the Loop Simulation, or SILSIM, allows running MatrixPilot on your computer instead of on a dsPic processor on a UAV Dev Board, and allows connecting to XPlane for simulated flights in the same way that [HILSIM](HardwareInLoop.md) does.

There is now a MatrixPilot-SIL project in the Tools directory.  This project references most of the MatrixPilot files in trunk/MatrixPilot, trunk/libDCM, and some headers from trunk/libUDB, and adds its own implementation of libUDB meant to run on a computer.  It runs a virtual UDB, complete with LEDs, a virtual file-based EEPROM, UDP sockets for the UARTs, C implementations of the DSP library and builtin() multiplication/division calls, etc.  Instead of the physical sensors, it uses the existing HILSIM code to receive virtual sensor data from XPlane.


## Configuring MatrixPilot-SIL

SILSIM support can be found in the trunk/Tools/MatrixPilot directory and includes makefile scripts for building with gcc on Windows or Linux/MAC platforms. Additionally Windows users can benefit from the user friendly debugging enviroment offered by Microsoft(r)Visual Studio with the included Visual C++ 2010 project.

To build for SILSIM, you'll just need to edit trunk/MatrixPilot/options.h to match your simulated plane.  (Or just copy trunk/MatrixPilot/example-options-files/options.HILSIM-Cessna-ben.h to trunk/MatrixPilot/options.h, and use the built-in Cessna in XPlane.)

Advanced SILSIM settings can be found in trunk/Tools/MatrixPilot-SIL/SIL-config.h  The default settings are depicted below:

```
////////////////////////////////////////////////////////////////////////////////
// Software In the Loop Simulation
//
// See the MatrixPilot wiki for more info on using SILSIM.
// Below are settings to configure the simulated UDB UARTs.
// The SERIAL_RC_INPUT settings allow optionally talking over a serial port to a UDB
// passing RC inputs through to the simulated UDB.
#define SILSIM_GPS_RUN_AS_SERVER		0
#define SILSIM_GPS_PORT				14551		// default port to connect to QGroundControl
#define SILSIM_GPS_HOST				"127.0.0.1"
#define SILSIM_TELEMETRY_RUN_AS_SERVER		0
#define SILSIM_TELEMETRY_PORT			14550		// default port to connect to XPlane HILSIM plugin
#define SILSIM_TELEMETRY_HOST			"127.0.0.1"
#define SILSIM_SERIAL_RC_INPUT_DEVICE		""		// i.e. "COM4" or "/dev/cu.usbserial-A600dP4v", or "" to disable
#define SILSIM_SERIAL_RC_INPUT_BAUD		38400
```

The SILSIM\_GPS options control the simulated GPS UART, SILSIM\_TELEMETRY configures the telemetry UART, and the SILSIM\_SERIAL lines control an optional serial connection to a physical UDB running HILSIM, just to get the RC inputs sent in to SILSIM from a real RC controller.

The RUN\_AS\_SERVER lines set whether matrixpilot should run a server on the given port, or connect to the given port as a client.  The HOST lines let you connect to a UDP port on any IPv4 host on the network.  No DNS is performed so you'll need to enter the host in the A.B.C.D format.

The SILSIM\_SERIAL\_RC\_INPUT\_DEVICE option lets you set the serial port to use, and if left as the empty string, will disable serial input.


## Building MatrixPilot-SIL

After configuring options.h, open trunk/Tools/MatrixPilot-SIL.  If using windows download a free copy of Visual C++ Express Edition and open the MatrixPilot-SIL.vcxproj project. If using gcc on windows or unix (mac, linux, etc) you can just invoke 'make' from the command line while in the MatrixPilot-SIL directory.

Using gcc and make will build 2 binaries: matrixpilot and silcat.

**matrixpilot** is the application that runs the MatrixPilot code on a simulated UDB.  It uses UDP sockets to connect the GPS port to XPlane, and to connect the telemetry port to your choice of QGroundControl, a terminal using silcat, or anything else that can listen on a UDP port.

**silcat** is a tiny command-line app that lets you read and write from/to matrixpilot's telemetry port.  Or to any other UDP port as a client or server, or to a serial port.  It can be used as a nice, simple, generic way to interact with a serial port or UDP port.

NOTE a Visual C++ silcat project has notas yet been created. Please post to the MatrixPilot mailing list if interested in this being supported.

## Beginners Guide to Compiling MatrixPilot-SIL in Windows

Download Visual C++ Express or a current installer from the http://www.mingw.org/ MinGW] project.

Run the respective setup program and accept the default settings.  After installing MinGW you may need to add the 'bin' folder to your system path.  You may also need to install [GNU make](http://www.gnu.org/software/make/), and possibly also include it in your system path.

Open your MatrixPilot options.h file and change the following values:

```
#define GPS_TYPE			GPS_UBX_4HZ
#define AILERON_CHANNEL_REVERSED	1
#define ELEVATOR_CHANNEL_REVERSED	1
#define RUDDER_CHANNEL_REVERSED		0
#define SERIAL_OUTPUT_FORMAT 		SERIAL_UDB or SERIAL_UDB_EXTRA
```

Set any other options as you desire

Save and Close options.h

Open a command prompt (Usually a black screen with white text)

Change directory to the MatrixPilot-SIL folder. The command will be similar to:

cd C:\ <path to my MatrixPilot source code> \Tools\MatrixPilot-SIL

for example:

cd C:\UDBCode\gentlenav\Tools\MatrixPilot-SIL

Enter the following command: mingw32-make all clean

Once that is complete, congratulations you have created the SILSIM executables!

Using File Explorer, browse to the MatrixPilot-SIL folder in your MatrixPilot files. You should now see MatrixPilot.exe and silcat.exe .

Setup X-Plane as you would using the [HILSIM](HardwareInLoop.md) documentation, confirm the line Port,14551 is not commented out.

Open X-Plane and silcat
Wait for X-Plane to fully load and the aircraft is on the runway.

Open MatrixPilot and wait for it to initialize. You should see logging data displayed on the silcat application.

Once is shows INIT: Ready., time to fly.

## Using MatrixPilot-SIL

See the [HILSIM](HardwareInLoop.md) page for info on getting XPlane set up for MatrixPilot simulation.

Then start up X-Plane and run matrixpilot in a terminal window.  You should be greeted with something like this:

```
MatrixPilot SIL

1/2/3 = mode manual/stabilized/waypoint
w/s   = throttle up/down
a/d   = rudder left/right
i/k   = elevator forward/back
j/l   = aileron left/right

z     = zero the sticks
L     = toggle LEDs
0     = toggle RC Radio connection on/off
xN    = execute LOGO subroutine N(0-9)
r     = reset
?     = show this help message
```

and then:

```
INIT: Calibrating...
INIT: sensors calibrated and trims recorded.
INIT: waiting for GPS...
INIT: GPS link acquired.
INIT: Ready.
```

At this point you're ready to fly!  You can use your RC controller if you set up SILSIM\_SERIAL\_RC\_INPUT\_DEVICE, or just use the indicated keys to control throttle, ailerons, elevator, rudder, and mode switch.  The mode will likely default to stabilized, since that's what you get with a centered-by-default 3 position switch.  Note that you'll be entering these keystrokes into the terminal window, not into the X-Plane window.

Once you have the plane in the air, try enabling WP mode.  If you're using LLGO, and already in WP mode, you can also switch over to executing other LOGO subroutines, by number, directly from the terminal by typing x followed by a number 0-9, where 0 runs your logo program from the top, and 1-9 are subroutines 1-9.  Currently only the first 9 subroutines can be accessed this way.

In another terminal window, you can run silcat to connect to the simulated UDB's telemetry port.  If you set the SERIAL\_FORMAT to SERIAL\_UDB\_EXTRA, for example, you'd see the telemetry streaming to stdout.  Since it streams to stdout, you could also pipe it into a file to save a log of the flight for analysis in flan, as though it were being recorded to an OpenLog.

Alternately, you could set SERIAL\_FORMAT to SERIAL\_MAVLINK, and run [QGroundControl](http://www.qgroundcontrol.org/) to see a live view of the plane, live-update gains, etc.