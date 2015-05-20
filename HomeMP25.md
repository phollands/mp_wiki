# UAV Dev Board
![http://static.sparkfun.com/images/products/09980-01b_i_ma.jpg](http://static.sparkfun.com/images/products/09980-01b_i_ma.jpg)

The UAV Dev Board, designed by Bill Premerlani, is an IMU-based development platform for building controllers for Unmanned Aerial Vehicles.  This site hosts the development of UAV code that runs on this board, allowing it to stabilize an airplane, maintain altitude, and optionally navigate back to the launch site.

You can [buy a UAV Dev Board from Spark Fun](http://www.sparkfun.com/commerce/product_info.php?products_id=9038).

If you buy a UAV Dev Board, and are interested in hearing about upcoming changes, or want to participate in or influence future development, we strongly urge you to join the [UAV Dev Board Discussion List](http://groups.google.com/group/uavdevboard).

There is also a lively community that discusses this platform on the [DIY Drones site](http://diydrones.com/page/uav-devboard).


## News!

**Team Lawn Dart places at the 2011 SparkFun Autonomous Vehicle Competition - Saturday April 23, 2011**

Congratulations to Team Lawn Dart on their great entry and performance.  They used a modified UDB, running a modified version of MatrixPilot to win 3rd place in the air competition.


**MatrixPilot wins DIY Drones T3 HILSim Contest - February 2011**

Congratulations to Marcus Fahlen of Norway on winning 1st place in the DIY Drones contest to replicate a flight around Manhattan using an autopilot (in this case the UDB3) connected to a flight simulator. Thanks to all of the wider team that have made Marc's success possible. See [here for the competition thread ](http://diydrones.com/profiles/blogs/t3-contest-season-2-round-1?id=705844%3ABlogPost%3A238107&page=5#comments), and the results were published [here](http://diydrones.com/profiles/blogs/winners-of-t3-contest).


**MatrixPilot 2.5 - Thursday, August 19, 2010**

After many months of work, the UDB development team is excited to release version 2.5 of MatrixPilot!  This version adds tons of new features, including inverted stabilization, camera targeting, more configurable waypoints, wind speed estimation, dead reckoning, and hardware-in-the-loop simulation.  See the full list on the [status page](ProjectStatus.md).


**[More News...](News.md)**


## Hardware Specifications
This board comes with a dsPIC30F4011 CPU, an MMA7260 three axis accelerometer and two dual-axis Inversense IXZ500 gyroscopes already soldered as shown in the picture. It is intended for the do-it-yourselfer. By itself, it can be used to develop a three axis IMU controller. With the addition of a GPS receiver, it can be used to develop a UAV controller for an RC car, plane, or boat.  All firmware versions are compatible with the [EM406 GPS](http://www.sparkfun.com/commerce/product_info.php?products_id=465).  The new MatrixPilot firmware can also use the [uBlox 5H GPS](http://store.diydrones.com/ProductDetails.asp?ProductCode=SPK-GPS-GS407).


## Already Have a UAV Dev Board?
  * [Join the uavdevboard google group to follow the latest development](http://groups.google.com/group/uavdevboard)
  * [Get set up to program your UAV Dev Board with the latest firmware (PDF)](http://gentlenav.googlecode.com/files/PICProgrammingGettingStarted.pdf)
  * [How to get your UAV Dev Board set up, installed, and connected to the rest of your hardware](HowToInstall.md)
  * [How to configure the firmware for your plane](HowToConfigure.md)
  * [Troubleshooting problems with your UAV Dev Board](Troubleshooting.md)
  * [Contribute to the project](HowToContribute.md)


## UAV Dev Board Firmwares

For info on the latest developments and future plans, check out the ProjectStatus page.

| **MatrixPilot** | **MatrixPilot is the latest branch of the firmware for the UAV Dev Board.  It is a recommended upgrade from both MatrixNav and AileronAssist.  It offers up to 5 input channels and 6 output channels, control of Delta Wing and V-Tail airframes, allows much more configuration, plus a bunch of other cool new features too.** |
|:----------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AileronAssist   | Aileron Assist controls a plane's elevator and ailerons to stabilize a plane's pitch and roll, and to navigate back to the launch location on command.  It is recommended that AileronAssist users upgrade to MatrixPilot for better support.                                                                                     |
| MatrixNav       | Matrix Nav controls a plane's elevator and rudder to stabilize its pitch, and navigate back to the launch location on command.  It is recommended that MatrixNav users upgrade to MatrixPilot for better support.                                                                                                                 |
| RollPitchYaw    | The Roll Pitch Yaw firmware is not meant for flying. It's a test program that drives three servos proportionally to the current roll, pitch, and yaw of the UAV Dev Board. It also sends serial data about it's orientation out the TX pin, which can be useful for visualizations on a computer.                                 |
| [UAVSelfTest](UAVSelfTest.md) | The UAVSelfTest firmware lets you test that your dev board hardware is functioning properly. It helps you test the receiver channels, the servo channels, the gyros, accelerometers, LEDs, and switches.                                                                                                                          |


## Related Projects and Sites

  * [Buy a UAV Dev Board from SparkFun](http://www.sparkfun.com/commerce/product_info.php?products_id=9038)
  * [uBlox 5H GPS with Adapter](http://store.diydrones.com/ProductDetails.asp?ProductCode=SPK-GPS-GS407)
  * [DIY Drones Community](http://diydrones.com/)
  * ["Oil Pan" UDB Expansion Board](http://code.google.com/p/uavexpansionboard/)