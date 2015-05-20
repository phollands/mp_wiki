# MatrixPilot: Connecting Extra Channels

The existing Red and Green boards only include built in connections for 4 Radio Input channels, and 3 Servo Output channels.

Here's how to connect the 5th Input channel, and the 4th, 5th, and 6th Output channels.


## Simple Method: One Extra Input and One Extra Output

The new Input runs on pin RE8.  The new outputs on RE0, RE2, and RE4.  This pins are made available along the bottom-left edge of the board.

If you just need one more input and one more output, here's a simple way to do it:

  * Find, re-purpose, or buy a male-to-female servo extension cable
  * Near the middle of the cable, separate the Orange (signal) wire from the other two for a distance of a couple inches
  * Cut only the orange wire, near the middle of the cable
  * Strip the two new ends of the orange wire
  * Either by soldering directly to the board, or through a header and connector, connect the side of the orange wire coming from the radio receiver to RE8, and the other end of the orange wire to RE0.  This will act as Input Channel 5 and Output Channel 4 on the UDB.

![http://gentlenav.googlecode.com/svn/wiki/images/simple_extra_channels.jpg](http://gentlenav.googlecode.com/svn/wiki/images/simple_extra_channels.jpg)


## All Extra Channels

To use all of these pins as a radio input and servo outputs, here's what I suggest doing:

  * Find, re-purpose, or buy 3 male-to-female servo extension cables
  * Near the middle of each cable, separate the Orange (signal) wire from the other two for a distance of a couple inches
  * Cut only the orange wire near the middle of the cable
  * Strip the two new ends of the orange wire
  * Either by soldering directly to the board, or through a 4-pin header and connector, make the following connections:
  * Take one cable and connect the orange wire that is connected to the female end(1) to RE8.  This female end will be Input 5.
  * Connect the other orange wire from that cable (connected to the male end) to RE0.  This male end will be Output 4.
  * Connect the orange wire that's connected to the male end of the next cable to RE2.  This male end will be Output 5.
  * Connect the orange wire that's connected to the male end of the last cable to RE4.  This male end will be Output 6.
  * Separately tape up each of the 2 unused ends that connect to the female ends of the 2nd and 3rd cables so they won't short on anything.  If you read through all the directions before starting, good job!  Your reward is that you can skip stripping these two remaining ends a few steps ago.

(1) Note: I'm calling the below pictured connector female.  This is the end that usually comes attached to servos, and that usually plugs into a receiver.

![http://www.pololu.com/picture/0J652.250.jpg](http://www.pololu.com/picture/0J652.250.jpg)


An easier method is to use these cables from pololu.com:

http://www.pololu.com/catalog/product/1800

http://www.pololu.com/catalog/product/1801

http://www.pololu.com/catalog/product/1902

http://www.pololu.com/catalog/product/1903

and arrange them as seen in these photos showing the parts needed to add one extra input and two extra outputs, both before:

![http://gentlenav.googlecode.com/svn/wiki/images/connectors1.jpg](http://gentlenav.googlecode.com/svn/wiki/images/connectors1.jpg)

and after connecting them together:

![http://gentlenav.googlecode.com/svn/wiki/images/connectors2.jpg](http://gentlenav.googlecode.com/svn/wiki/images/connectors2.jpg)

The 4 black connectors at the bottom of the photo just above are, left to right, servo output 5, servo output 4, a connection to a spare receiver output for power, and radio input 5.

No soldering is required to build these cables, although you'll still have to solder the header pins onto the UDB.


## Using the Extra Channels

Now to use these cables, connect the new Input 5 to a channel you want to use on your receiver.  And attach the other two female ends to 2 unused channels on your receiver.  (You can connect one of them to the BATT connection on the receiver, if that is unused in your setup.)  Note that these two channels are only being used for power, and the values from the receiver on these channels is being ignored.  Now you can connect servos to the other ends of these cables, and they will be driven as output channels 4, 5, and 6.

Here's a diagram put together by Peter Hollands:

![http://lh4.ggpht.com/_FuqmgzUKqZs/SqvWRKCYnCI/AAAAAAAAAac/iRT22B9njv0/image002.gif](http://lh4.ggpht.com/_FuqmgzUKqZs/SqvWRKCYnCI/AAAAAAAAAac/iRT22B9njv0/image002.gif)