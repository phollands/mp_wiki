# MatrixPilot: Power up and manual checkout

Power everything up. Turn on your transmitter first, and then your plane. If you are using an electronic speed control, make very sure that the throttle stick is set for minimum throttle and the mode control is set for manual. Otherwise your motor could possibly come on during the initialization sequence. It might be a good idea to prepare for the possibility that the motor will come on during initialization, until you go through the setup process once.

Hold the plane level and steady during the first 10 seconds of power up.

At the end of 10 seconds, the gyro and accelerometer offsets are recorded, and the ailerons and rudder will waggle once.  At this point you are free to move the plane.  If you have an ESC that requires you to cycle the throttle to full and then back to off, now is the time to do that.

When manual control comes on, check the directions of elevator and aileron under manual control. If either of them is in the wrong make a note that they are wrong, but don't fix them yet. It is best to get the stabilisation response of servos correct first, and then fix the manual movement of the servos later.

Now you need to wait for GPS lock, it may take a minute or two. After the GPS locks, the ailerons and rudder should "waggle" 4 times. When they stop waggling, the controls are ready for further setup.


## Table of LED Behavior

The green LED shows whether or not the UDB is receiving a signal from the radio receiver.  The red LED's meaning depends on whether we already have GPS lock or not.  The signal that the UDB has a GPS lock is 4 wags of the control surfaces.

| **Green LED** | **Red LED** | **Description** |
|:--------------|:------------|:----------------|
| Off           | Off         | Not powered, or not programmed properly |
| Off           | On          | If before GPS lock: Has power, but isn't yet receiving pulses from the receiver.  If After GPS lock: Return-to-Launch mode  |
| Off           | Blinking    | _(Not currently used)_ |
| On            | Off         |  If before GPS lock: Done with Calibration, but waiting for GPS.  If after GPS lock: Manual mode |
| On            | On          | Stabilized mode |
| On            | Blinking    | If before GPS lock: Calibrating the sensors.  If after GPS lock: Waypoint mode |


## Stabilization checkout

Test the stabilization functions, including altitude hold. Make sure the throttle is in the off position. Put the flight mode switch on your transmitter in "stabilize mode", but be prepared for the possibility that the motor will come on. Now is the time to check that the throttle reversing switch (SR3 by default), is in the right position. Make sure the controls are in the stabilize mode, and advance the throttle. If you have the throttle reversing switch in the correct position, the throttle will “snap” on to the minimum throttle setting. If you have the throttle reversing switch in the wrong position, the altitude hold will not engage, and the motor will stay off. So, if the motor does not come on, then this switch is in the wrong position, flip it the other way. Once you have it in the correct position, test out the altitude hold function by slowly increasing the throttle. What you are doing is changing the commanded altitude. At some point, the motor will turn on rather abruptly. Switch back and forth between manual and stabilized mode and notice the different throttle response in the two modes.

Next, set the throttle back to off, still in the stabilized mode, and then check pitch, roll, and yaw response. When you pitch, roll, and yaw the plane, the elevator, ailerons, and rudder should respond (for a standard airframe). The elevator should respond to pitch, the ailerons to roll and yaw, and the rudder to yaw.

Check that the directions of responses are correct. When the nose pitches down, the elevator should deflect up. The pitch control is proportional to the pitch error and pitch rate.

When you roll the plane, the aileron on the wing that is higher should deflect up, so that it would push the wing back down. There are two terms in the roll feedback, both a proportional term, and a roll gyro term. So, if you roll the plane slowly, the ailerons should respond in proportion to the bank angle. If you roll quickly, there should be a greater response.

When yawing the plane to the right, the ailerons should try to turn the plane left (the right aileron should deflect down), and the rudder should deflect left, trying to turn the plane left.

If either the elevator or the aileron control feedback is in the wrong direction, fix it by changing the position of the control servo reversing switches on the board. By default, SR1 controls reversing of ailerons, and SR2 is for the elevator.  By default, there is no reversing switch for rudder, so if you need to reverse the rudder channel, you'll have to edit the REVERSING section in options.h to set the rudder reversing setting to 1 or 0 (on or off), or set it to use the value of one of the 3 hardware switches. Once you get the correct feedback control directions for "stabilize mode", they will also be correct for return to launch.

If when moving the dev board between planes, you change SR1, SR2, or SR3, remember to change them back when you go back to using a different plane!

Once all the channels are correctly set for stabilize mode, then make sure that the manual inputs from our transmitter are correctly moving the ailerons, rudder, elevator etc. If the manual inputs have the wrong direction, then reverse them using the transmitter's reversing channel reversing function.


## Waypoint checkout

Once you have stabilize mode checked out, test the Waypoint function. You can do that by moving your 3-way mode switch to the 3rd position. Keep in mind that, depending on your settings, the motor may come on.  Walk around and see what the ailerons elevator, and rudder do. The elevator should continue pitch control. The ailerons should stabilize the roll, and also try to turn the plane back to the RTL point, as should the rudder.

Here are a few tips for doing that:
  * Temporarily set any rate feedback gains to zero, so that you do not have to hold the plane steady.
  * Keep the throttle stick off so the motor does not come on. Better yet, disconnect the motor, if that is possible.
  * Use short course legs so that you do not have to walk too far.

There is one more important part of Waypoint mode that you will want to test: what will be the bank angle when you switch to Waypoint mode when the plane is going 180 degrees away from the next waypoint.

To test that, walk away from the first waypoint, with your plane facing in the direction that you are walking. The ailerons should deflect in an attempt by the controls to turn around. This is how much the ailerons will initially deflect in an attempt to turn around. During flight, the plane will roll into a bank, and the ailerons will return to neutral. You will be interested in what the bank angle will be. To find out while you are walking, turn the plane a little bit in the direction of the roll, and roll the plane as if it were turning, until the ailerons go back to their neutral (center) position. You are now looking at the max bank angle that the plane will have at the start of its turn to each waypoint.


## Return to launch / Failsafe checkout

How you configure the UAV Dev Board to recognize signal loss depends on what your receiver outputs when it loses signal.

  * If your receiver just stops outputting pulses, you don't need to do any more configuration.
  * If your receiver just continues outputting the last known values for all channels, you actually can not make use of RTL.
  * If your receiver has a programmable loss-of-signal fail-safe, even just for one channel (usually throttle), program the fail safe to set this channel outside the range of valid pulse widths, and set your FAILSAFE\_INPUT\_CHANNEL to this channel.

In order to make the best use of the capabilities of the waypoints function, it is suggested that you set up the radio fail safe as follows:
  * Set the rudder, ailerons, and elevator for center position.
  * Set the control mode selection channel to select the waypoints
function.
  * Set the throttle however you want. If you are not using altitude hold, the motor will respond accordingly. If you are using altitude hold, If you set the throttle for full off, the aircraft will follow the waypoints with the throttle off. If you set the throttle for anything else, the aircraft will continue to follow the waypoints, including the height coordinates.

You can test the RTL function by setting your UDB to manual mode, and then shutting off the transmitter. Keep in mind that, depending on your settings, the motor may come on.  If your plane starts stabilizing and try to turn, you have your configuration successfully set up to enter RTL mode on signal loss.  It's a good idea to also make sure that the plane returns to manual mode when you turn your transmitter back on.

If everything looks good to you, then you are ready for flight.