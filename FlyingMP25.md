# MatrixPilot: Operation

You power up with the plane level and motionless, and the throttle set for off for an ESC, or for low idle for a fueled engine. It does not matter what direction your plane points when it powers up, north, south, east or west. It will achieve yaw lock in a few seconds after takeoff.

After 10 seconds, the ailerons and rudder will waggle once. Then you can let go of the plane. If you have an ESC that needs for you to move the throttle stick to maximum and back, now is the time to do it.

Next, wait for GPS lock. When the GPS is locked, the aileron will waggle 4 times. When it stops waggling, the RTL point is set, and you are ready for flight.

Before you launch, it is best to check the deflection direction of your control surfaces one more time, for both manual and stabilized control. For stabilized control, the aileron of the higher wing should deflect upward, to push the wing back down.

If everything checks out, you are ready for flight. If you have a fueled engine, now is the time to start it. You are cleared for takeoff.

With respect to altitude hold, operation is as follows:
  * Altitude hold is turned on whenever you have selected either Stabilized mode or WayPoint mode.
  * In Stabilized mode, altitude hold treats the throttle setting as the desired altitude.
  * In WayPoint mode, altitude hold uses the Z position of the current goal waypoint as the desired altitude.
  * Typical operation is to turn stabilized mode on prior to launch and do your takeoff and climb out in the usual fashion. When you get to the altitude that you want, turn the throttle down to what you think is cruising power, just enough power to hold altitude. The altitude hold function will then adjust throttle and pitch to maintain that altitude.

The three things you will probably have to experiment with are the pitch trim setting on your radio, the parameter ALT\_HOLD\_THROTTLE\_MIN and the parameter ALT\_HOLD\_PITCH\_MAX. You will probably set both ALT\_HOLD\_PITCH\_MIN and ALT\_HOLD\_PITCH\_HIGH to zero.

Typical value for ALT\_HOLD\_THROTTLE\_MIN is 0.5. If you typically fly in windy conditions, and want to penetrate the wind, use a larger value of ALT\_HOLD\_THROTTLE\_MIN. If you want to control altitude with pitch only, with the motor running at full power, set ALT\_HOLD\_THROTTLE\_MIN to 1.0, or enable RACING\_MODE.

You may also have to experiment with the pitch trim on your radio to achieve a reasonable performance compromise between climb out and descent.

If everything checks out, you are ready for flight. Your first flight should be in manual control during takeoff/launch. After a few flights, if everything checks out, you can try stabilized control mode during takeoff and landing, it will actually make it easier.

When you are leveled off, and ready, switch to stabilized control. This will be the same as if your plane turned into a sailplane. When you use the ailerons, and then let go, the plane will quickly level itself back. In stabilized mode, you can use the ailerons by themselves, or the rudder by itself, or both together, to make a turn. The stabilization should maintain level pitch in either case.

One thing to note is that if you are using both roll and yaw stabilization, and have both ailerons and a rudder, then if you manually turn using ailerons while in stabilized mode, the rudder will fight against you.  The effects of this dynamic are especially noticeable while landing with the throttle at zero while in Stabilized mode.  In this specific situation, I suggest steering with rudder control.

If the gains are too high, your plane may flutter around the pitch or the roll axis in the stabilized mode. I have seen it during my test flights when the gains were too high. It is not dangerous, but it is annoying. Even with the flutter, my plane continued to be level in both pitch and roll. If you do have a flutter, you can stop it by using lower gains, using the instructions on the HowToConfigure wiki page.

Once you are comfortable with stabilized control, you can try waypoint mode.  By default, the only waypoint is located 50 meters above the location where the plane started up.  To test, you can fly away from yourself, and then switch to waypoint mode and watch the plane turn around and head straight back to the startup location.  Later, you can change the set of waypoints by editing the waypoints.h file.

But no matter how the waypoints are configured, if your plane loses transmitter signal, it will switch back to Return To Launch mode, in which it will ignore waypoints, and head straight back to the startup location.  For this to work, you'll have to make sure that your UAV Dev Board can recognize when your receiver has lost your transmitter signal.