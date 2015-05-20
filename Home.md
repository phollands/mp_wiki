# UAV Dev Board
![https://dlnmh9ip6v2uc.cloudfront.net//images/products/1/1/7/0/3/11703-01a.jpg](https://dlnmh9ip6v2uc.cloudfront.net//images/products/1/1/7/0/3/11703-01a.jpg)

The UAV Dev Board, originally designed by Bill Premerlani, is an IMU-based development platform for building controllers for Unmanned Aerial Vehicles.  This site hosts the development of UAV code that runs on this board, allowing it to stabilize an airplane, maintain altitude, and navigate to specific locations, or back to the launch site.

You can [buy a UAV Dev Board from Spark Fun](https://www.sparkfun.com/products/11703).

You can also [buy an AUAV3 Dev Board from Nick Arsov](http://www.arsovtech.com).

If you buy a UAV Dev Board, and are interested in hearing about upcoming changes, or want to participate in or influence future development, we strongly urge you to join the [UAV Dev Board Discussion List](http://groups.google.com/group/uavdevboard).

There is also a lively community that discusses this platform on the [DIY Drones site](http://diydrones.com/page/uav-devboard).


## News!

**MatrixPilot 4.0 - Thursday, July 18, 2013**

This release includes support for SparkFun's UDB4 and UDB5 boards, and Nick Arsov's AUAV3.  Other improvements include cleaner navigation, better cross-tracking, and lower power consumption.

Note that starting with MatrixPilot 4.0, we no longer includes support for older UDB boards.  But fear not, there will continue to be MatrixPilot 3.x updates to backport important improvements to the older boards.


**DIY Drones T3-2 Competition Success ! - Saturday, March 10th, 2012**

Two entries using the UDB and MatrixPilot were in the winning 4 which were "tied" for the [the automatic takeoff and landing competition](http://diydrones.com/profiles/blogs/announcing-t3-2-round-two-the-autotakeoff-and-landing-round?id=705844%3ABlogPost%3A752867&page=2#comments). Congratulations to Phil Giacalone for a super automatic flight. In the previous competition for racing around a "figure of 8", Riccardo Kuebler came 2nd, and Phil Giacalone came 3rd. UDB MatrixPIlot took 5 of the top 12 places.


**UDB4 available from SparkFun - Friday, October 21, 2011**

The UAV Dev Board team are proud to announce availability of the next major revision of hardware from Sparkfun ([See Sparkfun site for details](http://www.sparkfun.com/products/10582)). Sparkfun also reviewed [some of the history](http://www.sparkfun.com/news/698) of the project.



**[More News...](News.md)**


## Hardware Specifications
The UDB5 (UAV Development Board version 5) comes populated with a dsPIC33FJ256 CPU, and the MPU-6000, a MEMS 3-axis gyroscope and 3-axis accelerometer. The on-board Invensense gyro has enough vibration tolerance to be used in RC heli applications.

It is intended for the do-it-yourselfer. By itself, it can be used to develop a three axis IMU controller. With the addition of a GPS receiver, it can be used to develop a UAV controller for an RC car, plane, or boat.  All firmware versions are compatible with the [EM406 GPS](http://www.sparkfun.com/commerce/product_info.php?products_id=465).  The new MatrixPilot firmware can also use the [uBlox 5H GPS](http://store.diydrones.com/ProductDetails.asp?ProductCode=SPK-GPS-GS407), and the [MediaTek GPS](http://store.diydrones.com/MediaTek_MT3329_GPS_10Hz_Adapter_Basic_p/mt3329-02.htm), but the developers currently recommend using either the [EM406](http://www.sparkfun.com/commerce/product_info.php?products_id=465) or the [MediaTek](http://store.diydrones.com/MediaTek_MT3329_GPS_10Hz_Adapter_Basic_p/mt3329-02.htm).


## Already Have a UAV Dev Board?
  * [Join the uavdevboard google group to follow the latest development](http://groups.google.com/group/uavdevboard)
  * [Get set up to program your UAV Dev Board with the latest firmware (PDF)](http://gentlenav.googlecode.com/files/PICProgrammingGettingStarted.pdf)
  * [How to get your UAV Dev Board set up, installed, and connected to the rest of your hardware](HowToInstall.md)
  * [How to configure the firmware for your plane](HowToConfigure.md)
  * [Troubleshooting problems with your UAV Dev Board](Troubleshooting.md)
  * [Contribute to the project](HowToContribute.md)


## Related Projects and Sites

  * [Buy a UAV Dev Board from SparkFun](https://www.sparkfun.com/)
  * [MediaTek GPS](http://store.diydrones.com/MediaTek_MT3329_GPS_10Hz_Adapter_Basic_p/mt3329-02.htm)
  * [EM406 GPS](http://www.sparkfun.com/commerce/product_info.php?products_id=465)
  * [DIY Drones Community](http://diydrones.com/)