# MatrixPilot: Configuration

To configure MatrixPilot to work for your plane, you'll probably need to edit your options.h file.  This page describes all the settings in that file.

To set up Waypoints, see the WayPoints wiki page.


## Hardware

First make sure to set which UAV Dev Board version you have.  The older green board or the newer red board.

```
////////////////////////////////////////////////////////////////////////////////
// Set Up Board Type (Set to RED_BOARD or GREEN_BOARD)
#define BOARD_TYPE 							RED_BOARD
```


Then choose your airframe type: Standard, V-Tail, or Delta Wing.

```
////////////////////////////////////////////////////////////////////////////////
// Choose your airframe type:
//    AIRFRAME_STANDARD		 	Elevator, and Ailerons and/or Rudder control
//    AIRFRAME_VTAIL			Ailerons(optional), and Elevator and Rudder as V-tail controls
//    AIRFRAME_DELTA			Aileron and Elevator as Elevons, and Rudder(optional)
// (Note that although AIRFRAME_HELI is also recognized, the code for this airframe type is not ready.)
#define AIRFRAME_TYPE						AIRFRAME_STANDARD
```


And choose which GPS you're using.  Currently we support the [EM-406A](http://www.sparkfun.com/commerce/product_info.php?products_id=465), and the [UBlox GS407](http://store.diydrones.com/ProductDetails.asp?ProductCode=SPK-GPS-GS407).

```
////////////////////////////////////////////////////////////////////////////////
// Set this value to your GPS type.  (Set to GPS_STD or GPS_UBX)
#define GPS_TYPE							GPS_STD
```


## Features

Next, you can choose which MatrixPilot features you'd like to use.

You can enable or disable each axis of stabilization.

```
// Roll, Pitch, and Yaw Stabilization
// Set any of these to 0 to disable the stabilization in that axis.
#define ROLL_STABILIZATION					1
#define PITCH_STABILIZATION					1
#define YAW_STABILIZATION_RUDDER				1
#define YAW_STABILIZATION_AILERON				1
```

and navigation using either rudder, ailerons, or both.

```
// Aileron and Rudder Navigation
// Set either of these to 0 to disable use of that control surface for navigation.
#define AILERON_NAVIGATION					1
#define RUDDER_NAVIGATION					1
```

If altitude hold is enabled, you can configure it further later in the file.

```
// Altitude Hold
// Set this to 0 to disable altitude hold.
#define USE_ALTITUDEHOLD					1
```

You can also enable camera stabilization if you have servo chanels available.

```
// Camera Stabilization
// To enable, set this value to 1, and assign one or more of the CAMERA_*_OUTPUT_CHANNELS below.
#define USE_CAMERA_STABILIZATION				0
```

Racing mode can be useful when trying to keep up your speed, or when fighting stronger winds.

```
// Racing Mode
// Setting this to 1 will keep the plane at full throttle while in waypoint mode
// Racing performance can be improved by disabling CROSSTRACKING in waypoints.h
#define RACING_MODE						0
```

You shouldn't use this NORADIO unless you need it for some esoteric ground testing of some otherwise untestable edge cases.

```
// Set this to 1 if you want the UAV Dev Board to fly your plane without a radio transmitter or
// receiver. (Totally autonomous.)  This is just meant for debugging.  It is not recommended that
// you acually use this since there is no automatic landing code yet, and you'd have no manual
// control to fall back on if things go wrong.  It may not even be legal in your area.
#define NORADIO							0
```


## Channel Setup

In this section you can configure how many input and output chanels you're using, and what you're using each channel for.  To eable the 5th input chanel, and the 4th-6th output channels, see the ConnectingExtraChannels wiki page.

```
// NUM_INPUTS: Set to 4 or 5 
//   4 enables only the standard 4 input channels
//   5 also enables E8 as the 5th input channel
#define NUM_INPUTS	5

// Channel numbers for each input.
// Use as is, or edit to match your setup.
//   - If you're set up to use Rudder Navigation (like MatrixNav), then you may want to swap
//     the aileron and rudder channels so that rudder is CHANNEL_1, and aileron is 5.
#define THROTTLE_INPUT_CHANNEL				CHANNEL_3
#define AILERON_INPUT_CHANNEL				CHANNEL_1
#define ELEVATOR_INPUT_CHANNEL				CHANNEL_2
#define RUDDER_INPUT_CHANNEL				CHANNEL_5
#define MODE_SWITCH_INPUT_CHANNEL			CHANNEL_4
#define CAMERA_ROLL_INPUT_CHANNEL			CHANNEL_UNUSED
#define CAMERA_PITCH_INPUT_CHANNEL			CHANNEL_UNUSED
#define CAMERA_YAW_INPUT_CHANNEL			CHANNEL_UNUSED


// NUM_OUTPUTS: Set to 3, 4, 5, or 6
//   3 enables only the standard 3 output channels
//   4 also enables E0 as the 4th output channel
//   5 also enables E2 as the 5th output channel
//   6 also enables E4 as the 6th output channel
#define NUM_OUTPUTS	5

// Channel numbers for each output
// Use as is, or edit to match your setup.
//   - Only assign each channel to one output purpose
//   - If you don't want to use an output channel, set it to CHANNEL_UNUSED
//   - If you're set up to use Rudder Navigation (like MatrixNav), then you may want to swap
//     the aileron and runner channels so that rudder is CHANNEL_1, and aileron is 5.
// 
// NOTE: If your board is powered from your ESC through the throttle cable, make sure to
// connect THROTTLE_OUTPUT_CHANNEL to one of the built-in Outputs (1, 2, or 3) to make
// sure your board gets power.
// 
#define THROTTLE_OUTPUT_CHANNEL				CHANNEL_3
#define AILERON_OUTPUT_CHANNEL				CHANNEL_1
#define ELEVATOR_OUTPUT_CHANNEL				CHANNEL_2
#define RUDDER_OUTPUT_CHANNEL				CHANNEL_4
#define AILERON_SECONDARY_OUTPUT_CHANNEL		CHANNEL_5
#define CAMERA_ROLL_OUTPUT_CHANNEL			CHANNEL_UNUSED
#define CAMERA_PITCH_OUTPUT_CHANNEL			CHANNEL_UNUSED
#define CAMERA_YAW_OUTPUT_CHANNEL			CHANNEL_UNUSED
```

Here you can choose which of your radio/servo channels are reversed.  If a servo requires a reversed signal, make sure you first have your transmitter set up to correctly control the plane manualy, and then set these settings to match your radio.

```
// Servo Reversing Configuration
// Here you can choose which reversing switches use hardware switches, and hard code the rest.
// Note that your servo reversing settings here should match what you set on your transmitter.
// For any of these that evaluate to 1 (either hardcoded or by flipping a switch on the board,
// as you define below), that servo will be sent reversed controls.
#define AILERON_CHANNEL_REVERSED			HW_SWITCH_1
#define ELEVATOR_CHANNEL_REVERSED			HW_SWITCH_2
#define RUDDER_CHANNEL_REVERSED				HW_SWITCH_3
#define AILERON_SECONDARY_CHANNEL_REVERSED		0 // Hardcoded to be unreversed, since we have only 3 switches.
#define THROTTLE_CHANNEL_REVERSED			0 // Set to 1 to hardcode a channel to be reversed
#define CAMERA_ROLL_CHANNEL_REVERSED			0
#define CAMERA_PITCH_CHANNEL_REVERSED			0
#define CAMERA_YAW_CHANNEL_REVERSED			0

// Set this to 1 if you need to switch the left and right elevon or vtail surfaces
#define ELEVON_VTAIL_SURFACES_REVERSED			0
```

Here you can set up the thresholds for your mode switching channel.  A 3-way switch works best for this channel, but a knob can also work, as could a 2-way switch combined with a trim for this channel.  These values are the positions where we switch between the low and middle, and then the middle and high positions of the 3-way mode switch.

```
// Mode Switch is ideally controlled by a 3-position switch on your transmitter.
// Often the Flap channel will be controlled by a 3-position switch.
// These are the thresholds for the cutoffs between low and middle, and between middle and high.
// Normal signals should fall within about 2000 - 4000, with 3000 being the center.
#define MODE_SWITCH_THRESHOLD_LOW			2600
#define MODE_SWITCH_THRESHOLD_HIGH			3400
```

Setting up the Failsafe Chanel allows your plane to know when it loses transmitter signal, and return to where it was launched.

```
// The Failsafe Channel is the RX channel that is monitored for loss of signal
// Make sure this is set to a channel you actually have plugged into the UAV Dev Board!
// 
// No matter which input you use as your failsafe, make sure you are also sending
// valid servo pulses to Input channel 4 every ~20ms, or the failsafe will not work properly.
// Just making sure you have Input 4 connected to your receiver will accomplish this.
// 
// For a receiver that remembers a failsafe value for when it loses the transmitter signal,
// like the Spektrum AR6100, you can program the receiver's failsafe value to a value below
// the normal low value for that channel.  Then set the FAILSAFE_INPUT_MIN value to a value
// between the receiver's programmed failsafe value and the transmitter's normal lowest
// value for that channel.  This way the firmware can detect the difference between a normal
// signal, and a lost transmitter.
//
// FAILSAFE_INPUT_MIN and _MAX define the range within which we consider the radio on.
// Normal signals should fall within about 2000 - 4000.
#define FAILSAFE_INPUT_CHANNEL				THROTTLE_INPUT_CHANNEL
#define FAILSAFE_INPUT_MIN				1500
#define FAILSAFE_INPUT_MAX				4500
```

## Telemetry and Serial Output

```
// Serial Output Format (Can be SERIAL_NONE, SERIAL_DEBUG, SERIAL_ARDUSTATION, SERIAL_UDB, or SERIAL_OSD_REMZIBI)
// This determines the format of the output sent out the spare serial port.
// Note that SERIAL_OSD_REMZIBI only works with GPS_UBX.
#define SERIAL_OUTPUT_FORMAT				SERIAL_NONE
```


## Gains

Servo Saturation controls the maximum throw of the servos.  Typical value is 1.0.  Maximum recommended value is 1.0, maximum valid value is 1.999.  Lower values will stop servos from moving too far.

```
// SERVOSAT limits servo throw by controlling pulse width saturation.
// set it to 1.0 if you want full servo throw, otherwise set it to the portion that you want
#define SERVOSAT					1.0
```


There are several gains that you can adjust. They should all be set to positive numbers.  If you think any gain needs to be reversed, you should instead reversals in the Reversal section above, and with the board reversing switches SR1, SR2 and SR3. Use the same gains on either the red board or the green board, the appropriate multipliers are now built into the firmware.

  * ROLLKP – This is the proportional feedback for the aileron control of roll. Setting it higher will improve precision of the bank leveling, but will reduce the bank angle and will make the turning radius get larger. Setting it too high may cause low frequency roll flutter that is annoying but not dangerous. Setting it lower will increase the bank angle and sharpen the turns, particularly during RTL. Typical value is 0.25.
  * ROLLKD – This is the derivative (gyro) feedback for the aileron control of roll. It is used to improve the damping of the roll control, to dampen any low frequency flutter. But if it is set too high, there may be a high frequency flutter that is annoying but not dangerous. Typical value is 0.125. This gain does not have to be greater than or equal to ROLLKP, you can use any value that you want.
  * YAWKP\_AILERON – This is the proportional turning gain used by navigation for controlling ailerons. Typical value is 0.1. Larger values will produce tighter turns. Using a value that is too large will produce a “dutch roll”.  Maximum valid value is 1.999.
  * YAWKD\_AILERON – This is the derivative yaw gain used for yaw damping by yaw stabilization to reduce the impact of the wind, and to help stabilize the yaw control. Typical value is 0.2. Maximum valid value is 0.5.
  * AILERON\_BOOST – This is an amplification, or “boost” factor for manual control of the ailerons, used during stabilized and waypoint modes to restore control authority to the ailerons in the face of the damping effect of stabilization. Typical value is 1.0. This factor is in addition to the manual control, so a value of PITCHBOOST of 0 turns the boost off, and provides unmodified response to manual control. A PITCHBOOST of 1 makes the elevator response to manual control approximately twice as great. Maximum valid value is 1.999.

```
// Aileron/Roll Control Gains
// ROLLKP is the proportional gain, approximately 0.25
// ROLLKD is the deriviate (gyro) gain, approximately 0.125
// YAWKP_AILERON is the proportional feedback gain for ailerons in response to yaw error
// YAWKD_AILERON is the derivative feedback gain for ailerons in reponse to yaw rotation
// AILERON_BOOST is the additional gain multiplier for the manually commanded aileron deflection
#define ROLLKP					0.25
#define ROLLKD					0.125
#define YAWKP_AILERON				0.100
#define YAWKD_AILERON				0.2
#define AILERON_BOOST				1.0
```


  * PITCHGAIN – This is the proportional feedback for the elevator control of pitch. Setting it higher will improve precision of the pitch leveling. If you set it too high, it may cause pitch flutter, but it is not dangerous, just annoying. Typical value is 0.250. Maximum valid value is 1.999.
  * PITCHKD – This is the pitch rate (measured in the earth coordinate system!) damping feedback for the elevator. Typical value is 0.25. Maximum valid value is (0.50\*SCALEGYRO).
  * RUDDER\_ELEV\_MIX – This is the amount of rudder-elevator mixing that you want. Typical value is 0.5. Set this parameter to 0 if you do not want to use this mixing. Maximum valid value is 1.999.
  * ROLL\_ELEV\_MIX – This is the amount of roll-elevator mixing that you want. Typical value is 0.1. Set this parameter to 0 if you do not want to use this mixing. Maximum valid value is 1.999.
  * ELEVATOR\_BOOST – This is an amplification, or “boost” factor for manual control of the elevator, used during stabilized mode to restore control authority to the elevator in the face of the damping effect of stabilization. Typical value is 0.5. This factor is in addition to the manual control, so a value of ELEVATOR\_BOOST of 0 turns the boost off, and provides unmodified response to manual control. A ELEVATOR\_BOOST of 1 makes the elevator response to manual control approximately twice as great. Maximum valid value is 1.999.


```
// Elevator/Pitch Control Gains
// PITCHGAIN is the pitch stabilization gain, typically around 0.125
// PITCHKD feedback gain for pitch damping, around 0.0625
// RUDDER_ELEV_MIX is the degree of elevator adjustment for rudder and banking
// AILERON_ELEV_MIX is the degree of elevator adjustment for aileron
// ELEVATOR_BOOST is the additional gain multiplier for the manually commanded elevator deflection
#define PITCHGAIN				0.150
#define PITCHKD					0.0625
#define RUDDER_ELEV_MIX				0.5
#define ROLL_ELEV_MIX				0.1
#define ELEVATOR_BOOST				0.5
```


  * YAWKP\_RUDDER – This is the turning gain used by RTL for using the rudder to make a turn. Typical value is 0.1. Larger values will produce tighter turns. Using a value that is too large will produce a “dutch roll”. Maximum valid value is 1.999.
  * YAWKD\_RUDDER – This is a yaw damping term used both by stabilization and RTL to reduce the impact of the wind, and to help stabilize the yaw control. Typical value is 0.2. Maximum valid value is 0.5.
  * RUDDER\_BOOST – This is an amplification, or “boost” factor for manual control of the rudder, used during stabilized mode to restore control authority to the rudder in the face of the damping effect of stabilization. Typical value is 1.0. This factor is in addition to the manual control, so a value of RUDDER\_BOOST of 0 turns the boost off, and provides unmodified response to manual control. A RUDDER\_BOOST of 1 makes the rudder response to manual control approximately twice as great. Maximum valid value is 1.999.

```
// Rudder/Yaw Control Gains
// YAWKP_RUDDER is the proportional feedback gain for rudder navigation
// YAWKD_RUDDER is the yaw gyro feedback gain for the rudder in reponse to yaw rotation
// RUDDER_BOOST is the additional gain multiplier for the manually commanded rudder deflection
#define YAWKP_RUDDER				0.0625
#define YAWKD_RUDDER				0.5
#define RUDDER_BOOST				1.0
```


These camera gains are currently proportional to the roll, pitch, and yaw of the plane.  This is untested, and these gains will likely need to be adjusted to work well.

```
// The following section is for camera stabilization
// These values are untested and will need adjustment
#define CAMERA_ROLLKP				0.5
#define CAMERA_PITCHKP				0.5
#define CAMERA_YAWKP				0.5
```



## Altitude Hold

  * HEIGHT\_TARGET\_MIN - This is the minimum target height, in meters, used in stabilized mode.
  * HEIGHT\_TARGET\_MAX – This is the maximum target height, in meters, above the launch point. Typical value is 100. The commanded height for altitude hold is proportional to the throttle, up to this maximum height. Altitude hold will command full throttle until the plane is within 50 meters of the commanded height. It will gradually reduce throttle as it climbs higher. It will reduce to minimum throttle at the commanded height. If it continues to climb higher, the motor will be cut off completely.
  * HEIGHT\_MARGIN - The vertical range, in meters, to try to keep the plane within when altitude hold is enabled.
  * ALT\_HOLD\_THROTTLE\_MIN – This parameter sets a value for the minimum amount of throttle during altitude hold. Typical value is 0.55, define a value between 0.0 and 1.0
  * ALT\_HOLD\_THROTTLE\_MAX – This parameter sets a value for the maximum amount of throttle during altitude hold. Typical value is 1.0, define a value between 0.0 and 1.0
  * ALT\_HOLD\_PITCH\_MIN – This is the pitch angle, in degrees, that the control will attempt to hold the plane’s pitch, at minimum throttle. The suggested value for this parameter for a sailplane is 0. If you want the altitude hold feature to maintain altitude without turning off the motor, select a slightly negative value for this parameter, such as -2. If you do not want to use this feature, set it to 0. Otherwise, set it the pitch angle that you would normally control the plane at minimum throttle. Positive values means the nose points upward, negative values means the nose pitches downward.
  * ALT\_HOLD\_PITCH\_MAX – This is the pitch angle, in degrees, that the control will attempt to hold the plane’s pitch, at MAXIMUM throttle. The suggested value for this parameter is 10. Otherwise, set the pitch angle that you would normally control the plane at maximum throttle. Positive values means the nose points upward, negative values means the nose pitches downward.
  * ALT\_HOLD\_PITCH\_HIGH – This is the pitch angle, in degrees, that the control will attempt to hold the plane’s pitch, at ZERO throttle. In other words, this is the pitch angle that you want when your sailplane is gliding, or when your plane is flying “dead stick”. The suggested value for this parameter is 0. If you do not want to use this feature, set it to 0. Otherwise, set it to the pitch angle that you would normally control the plane at zero throttle. Positive values means the nose points upward, negative values means the nose pitches downward.

```
// Min and Max target heights in meters.  These only apply to stabilized mode.
#define HEIGHT_TARGET_MIN		25.0
#define HEIGHT_TARGET_MAX		100.0

// The range of altitude within which to linearly vary the throttle
// and pitch to maintain altitude.  A bigger value makes altitude hold
// smoother, and is suggested for very fast planes.
#define HEIGHT_MARGIN			 10

// Use ALT_HOLD_THROTTLE_MAX when below HEIGHT_MARGIN of the target height.
// Interpolate between ALT_HOLD_THROTTLE_MAX and ALT_HOLD_THROTTLE_MIN
// when within HEIGHT_MARGIN of the target height.
// Use ALT_HOLD_THROTTLE_MIN when above HEIGHT_MARGIN of the target height.
// Throttle values are from 0.0 - 1.0.
#define ALT_HOLD_THROTTLE_MIN		0.35
#define ALT_HOLD_THROTTLE_MAX		1.0

// Use ALT_HOLD_PITCH_MAX when below HEIGHT_MARGIN of the target height.
// Interpolate between ALT_HOLD_PITCH_MAX and ALT_HOLD_PITCH_MIN when
// within HEIGHT_MARGIN of the target height.
// Use ALT_HOLD_PITCH_HIGH when above HEIGHT_MARGIN of the target height.
// Pitch values are in degrees.  Negative values pitch the plane down.
#define ALT_HOLD_PITCH_MIN		 0.0
#define ALT_HOLD_PITCH_MAX		15.0
#define ALT_HOLD_PITCH_HIGH		 0.0
```


RTL\_PITCH\_DOWN is used if you want the nose to pitch down during return to launch. It is the same as in previous versions of AileronAssist. If you use this feature, enter a positive value for this parameter, which is the angle, in degrees, that you want the nose to pitch down during RTL. The idea is that you may have a dead motor and a head wind during a loss of signal RTL, so you might be willing to sacrifice some altitude to get your plane back to you. Some pilots use this feature, some do not. Typical value if you use it is 2. If you do not use it, set it to zero. Maximum recommended value is 10.0.

```
// Return To Launch Pitch Down in degrees, a real number.
// this is the real angle in degrees that the nose of the plane will pitch downward during a return to launch.
// it is used to increase speed (and wind penetration) during a return to launch.
// set it to zero if you do not want to use this feature.
// This only takes effect when entering RTL mode, which only happens when the plane loses the transmitter signal.
#define RTL_PITCH_DOWN			  0.0
```


## Waypoint Handling

The waypoint handling options live at the top of the waypoints.h file.

```
////////////////////////////////////////////////////////////////////////////////
// Waypoint handling

// You have the option of using either cross tracking,
// in which navigation is based on the distance of the plane
// to the line between the waypoints.
// Or you can use navigation directly toward the goal point.
// If you want to use cross tracking, set USE_CROSSTRACKING to 1,
// otherwise, to use navigation directly toward the goal,
// set USE_CROSSTRACKING to 0.
#define USE_CROSSTRACKING		0

// Move on to the next waypoint when getting within this distance of the current goal (in meters)
// Only applies if not using cross tracking.
#define WAYPOINT_RADIUS 		25
```