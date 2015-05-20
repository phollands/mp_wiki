# MatrixPilot: Configuration

To configure MatrixPilot to work for your plane, you'll need to edit your options.h file.  This page describes all the settings in that file, as of MatrixPilot 3.0.

Also available are older versions for [MatrixPilot 2.0](HowToConfigureMP20.md), and for [MatrixPilot 2.5](HowToConfigureMP25.md).

To set up Waypoints, see the WayPoints wiki page.


## Hardware

First make sure to set which UAV Dev Board version you have.

```
// Set Up Board Type
// GREEN_BOARD - Board is green and includes 2 vertical gyro daugter-boards.
// RED_BOARD   - Board is red, and includes 2 vertical gyro daugter-boards.
// UDB3_BOARD  - Board is red, and includes a single, flat, multi-gyro daugter-board.
// See the MatrixPilot wiki for more details on different UDB boards.
// If building for UDB4, use the MatrixPilot-udb4.mcp project file.
#define BOARD_TYPE 						UDB3_BOARD
```


The clock speed should not have any noticeable effect for most people.  But if you're hacking on the code and need faster baud rates, change this to FRC8X\_CLOCK.

```
// Select Clock Configuration (Set to CRYSTAL_CLOCK or FRC8X_CLOCK)
// CRYSTAL_CLOCK is the 16 MHz crystal.  This is the speed used in the past, and may be
// more compatible with other add-ons. The CRYSTAL_CLOCK supports a maximum baud rate of 19200 bps.
// FRC8X_CLOCK runs the fast RC clock (7.3728 MHz) with 8X PLL multiplier, and supports much
// faster baud rates.
#define CLOCK_CONFIG 						CRYSTAL_CLOCK
```


You can mount the board into your plane in any of the below 6 orientations.  Then set this value to match.  This can be useful if you need to mount the board backwards or on the side of the plane for physical/space/layout reasons.

```
// Use board orientation to change the mounting direction of the board.
// The following 4 orientations have the board parallel with the ground.
// ORIENTATION_FORWARDS:  Component-side up,   GPS connector front
// ORIENTATION_BACKWARDS: Component-side up,   GPS connector back
// ORIENTATION_INVERTED:  Component-side down, GPS connector front
// ORIENTATION_FLIPPED:   Component-side down, GPS connector back
// The following 2 orientations are "knife edge" mountings
// ORIENTATION_ROLLCW: Rick's picture #9, board rolled 90 degrees clockwise,
//		from point of view of the pilot
// ORIENTATION_ROLLCW180: Rick's pitcure #11, board rolled 90 degrees clockwise,
//		from point of view of the pilot, then rotate the board 180 around the Z axis of the plane,
//		so that the GPS connector points toward the tail of the plane
#define BOARD_ORIENTATION					ORIENTATION_FORWARDS
```


Then choose your airframe type: Standard, V-Tail, or Delta Wing.

```
// Choose your airframe type:
//    AIRFRAME_STANDARD		 	Elevator, and Ailerons and/or Rudder control
//    AIRFRAME_VTAIL			Ailerons(optional), and Elevator and Rudder as V-tail controls
//    AIRFRAME_DELTA			Aileron and Elevator as Elevons, and Rudder(optional)
// (Note that although AIRFRAME_HELI is also recognized, the code for this airframe type is not ready.)
#define AIRFRAME_TYPE						AIRFRAME_STANDARD
```


And choose which GPS you're using.  Currently we support the [EM-406A](http://www.sparkfun.com/commerce/product_info.php?products_id=465), and the [UBlox GS407](http://store.diydrones.com/ProductDetails.asp?ProductCode=SPK-GPS-GS407).  Many of us are having better luck with the EM-406A.  (When using the EM-406A, choose GPS\_STD below.)

```
// Set this value to your GPS type.  (Set to GPS_STD, GPS_UBX_2HZ, or GPS_UBX_4HZ)
#define GPS_TYPE						GPS_STD
```


## Features

Next, you can choose which MatrixPilot features you'd like to use.

You can enable or disable each axis of stabilization.

```
// Roll, Pitch, and Yaw Stabilization
// Set any of these to 0 to disable the stabilization in that axis, using that control surface.
#define ROLL_STABILIZATION_AILERONS				1
#define ROLL_STABILIZATION_RUDDER				0
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


Wind gain adjustment allows turns in windy conditions to be smoother, relative to the ground.

```
// Wind Gain Adjustment
// This is an option for modulating the navigation gains in flight
// to maintain a constant turn radius in heavy winds in waypoint mode.
// Define WIND_GAIN_ADJUSTMENT as 1 to turn this feature on.
#define WIND_GAIN_ADJUSTMENT					0
```


Here you can enable altitude hold independently for stabilized mode and for waypoint mode.  Each mode can be set to maintain altitude either using elevator (AH\_PITCH\_ONLY), both elevator and throttle (AH\_FULL), or to disable altitude hold all together (AH\_NONE).  If altitude hold is enabled, you can configure it in more detail later in the file.

```
// Altitude Hold
// Use altitude hold in stabilized mode?  In waypoint mode?
// Each of these settings can be AH_NONE, AH_FULL, or AH_PITCH_ONLY
//  - In waypoint mode, the target altitude is defined by the waypoints or logo program.
//  - In stabilized mode, when ALTITUDEHOLD_STABILIZED is set to AH_PITCH_ONLY, the target
// altitude is whatever altitude the plane was at when switched into stabilized mode.
//  - In stabilized mode, when ALTITUDEHOLD_STABILIZED is set to AH_FULL, the target
// altitude is determined by the position of the throttle stick on the transmitter.
// NOTE: even when set to AH_NONE, MatrixPilot will still try to stabilize pitch as long
// as PITCH_STABILIZATION is set to 1 above, but will not aim for any specific altitude.
#define ALTITUDEHOLD_STABILIZED					AH_PITCH_ONLY
#define ALTITUDEHOLD_WAYPOINT					AH_FULL
```


Enabling Speed Control also improves the performance of altitude hold.  Be sure to set the desired speed fast enough to keep your plane stable, and at least as fast as the expected winds during your flights.

```
// Speed Control
// If you define SPEED_CONTROL to be 1, MatrixPilot will take air speed into account
// in the altitude controls, and will trim the throttle and pitch to maintain air speed.
// Define DESIRED_SPEED to be the air speed that you want, in meters/second.
#define SPEED_CONTROL						0
#define DESIRED_SPEED						10.00 // meters/second
```


You can enable stabilization of inverted flight, and of vertical, nose-up hovering.  These can be enabled separately for stabilized mode and for waypoint mode.  If enabled for stabilized mode, the UDB will try to stabilize the closest orientation to your current orientation.  For example, if you manually override stabilization to flip upside down, the UDB will then start stabilizing inverted.  In waypoint mode, the UDB will stabilize whichever orientation is defined for the current target waypoint in the waypoints.h file.

```
// Inverted flight
// Set these to 1 to enable stabilization of inverted flight in stabilized and/or waypoint modes.
#define INVERTED_FLIGHT_STABILIZED_MODE				0
#define INVERTED_FLIGHT_WAYPOINT_MODE				0

// Hovering
// Set these to 1 to enable stabilization of hovering in stabilized and/or waypoint modes.
#define HOVERING_STABILIZED_MODE				0
#define HOVERING_WAYPOINT_MODE					0
```


```
Note: As of MatrixPilot 3.0, Dead Reckoning and Wind Estimation are automatically enabled.
```


You can also enable camera stabilization if you have servo channels available.

```
// Camera Stabilization
// To enable, set this value to 1, and assign one or more of the CAMERA_*_OUTPUT_CHANNELS below.
#define USE_CAMERA_STABILIZATION				0
```

If you have a magnetometer attached to the UDB, set this to 1.

```
// Define MAG_YAW_DRIFT to be 1 to use magnetometer for yaw drift correction.
// Otherwise, if set to 0 the GPS will be used.
#define MAG_YAW_DRIFT                                           0
```

Racing mode can be useful when trying to keep up your speed, or when fighting stronger winds.

```
// Racing Mode
// Setting RACING_MODE to 1 will keep the plane at a set throttle value while in waypoint mode.
// RACING_MODE_WP_THROTTLE is the throttle value to use, and should be set between 0.0 and 1.0.
// Racing performance can be improved by disabling cross tracking for your waypoints.
#define RACING_MODE						0
#define RACING_MODE_WP_THROTTLE					1.0
```


You shouldn't use this NORADIO unless you need it for some esoteric ground testing of some otherwise untestable edge cases.

```
// Set this to 1 if you want the UAV Dev Board to fly your plane without a radio transmitter or
// receiver. (Totally autonomous.)  This is just meant for debugging.  It is not recommended that
// you actually use this since there is no automatic landing code yet, and you'd have no manual
// control to fall back on if things go wrong.  It may not even be legal in your area.
#define NORADIO							0
```


## Channel Setup

In this section you can configure how many input and output channels you're using, and what you're using each channel for.

Enable PPM input here.  This allows you to send all input RC channels into the UDB through one servo cable, coming from a special pin on your RC receiver, or from a PPM encoder board connected to your receiver.

```
// Use a single PPM input connection from the RC receiver to the UDB on RC input channel 4.
// This frees up RC inputs 3, 2, and 1 to act as RC outputs 4, 5, and 6.
// If you're not sure, leave USE_PPM_INPUT set to 0.
// PPM_NUMBER_OF_CHANNELS is the number of channels sent on the PWM signal.  This is
// often different from the NUM_INPUTS value below, and should usually be left at 8.
// If PPM_ALT_OUTPUT_PINS is set to 0, the 9 available RC outputs will be sent to the
// following pins, in this order: Out1, Out2, Out3, In3, In2, In1, RE0, RE2, RE4.
// With it set to 1, the RC outputs will be in this alternate configuration:
// Out1, Out2, Out3, RE0, RE2, RE4, In3, In2, In1.
#define USE_PPM_INPUT						0
#define PPM_NUMBER_OF_CHANNELS					8
#define PPM_SIGNAL_INVERTED					0
#define PPM_ALT_OUTPUT_PINS					0
```


If not using PPM input, then to enable a 5th input channel, and the 4th-6th output channels, see the ConnectingExtraChannels wiki page.

```
// NUM_INPUTS: Set to 1-5 (or 1-8 when using PPM input)
//   1-4 enables only the first 1-4 of the 4 standard input channels
//   5 also enables E8 as the 5th input channel
#define NUM_INPUTS						5

// Channel numbers for each input.
// Use as is, or edit to match your setup.
//   - If you're set up to use Rudder Navigation (like MatrixNav), then you may want to swap
//     the aileron and rudder channels so that rudder is CHANNEL_1, and aileron is 5.
#define THROTTLE_INPUT_CHANNEL					CHANNEL_3
#define AILERON_INPUT_CHANNEL					CHANNEL_1
#define ELEVATOR_INPUT_CHANNEL					CHANNEL_2
#define RUDDER_INPUT_CHANNEL					CHANNEL_5
#define MODE_SWITCH_INPUT_CHANNEL				CHANNEL_4
#define CAMERA_PITCH_INPUT_CHANNEL				CHANNEL_UNUSED
#define CAMERA_YAW_INPUT_CHANNEL				CHANNEL_UNUSED
#define OSD_MODE_SWITCH_INPUT_CHANNEL				CHANNEL_UNUSED
#define PASSTHROUGH_A_INPUT_CHANNEL				CHANNEL_UNUSED
#define PASSTHROUGH_B_INPUT_CHANNEL				CHANNEL_UNUSED
#define PASSTHROUGH_C_INPUT_CHANNEL				CHANNEL_UNUSED
#define PASSTHROUGH_D_INPUT_CHANNEL				CHANNEL_UNUSED


// NUM_OUTPUTS: Set to 3, 4, 5, or 6
//   3 enables only the standard 3 output channels
//   4 also enables E0 as the 4th output channel
//   5 also enables E2 as the 5th output channel
//   6 also enables E4 as the 6th output channel
//   NOTE: If USE_PPM_INPUT is enabled above, up to 9 outputs are available.)
#define NUM_OUTPUTS						4

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
#define THROTTLE_OUTPUT_CHANNEL					CHANNEL_3
#define AILERON_OUTPUT_CHANNEL					CHANNEL_1
#define ELEVATOR_OUTPUT_CHANNEL					CHANNEL_2
#define RUDDER_OUTPUT_CHANNEL					CHANNEL_4
#define AILERON_SECONDARY_OUTPUT_CHANNEL			CHANNEL_UNUSED
#define CAMERA_PITCH_OUTPUT_CHANNEL				CHANNEL_UNUSED
#define CAMERA_YAW_OUTPUT_CHANNEL				CHANNEL_UNUSED
#define TRIGGER_OUTPUT_CHANNEL					CHANNEL_UNUSED
#define PASSTHROUGH_A_OUTPUT_CHANNEL				CHANNEL_UNUSED
#define PASSTHROUGH_B_OUTPUT_CHANNEL				CHANNEL_UNUSED
#define PASSTHROUGH_C_OUTPUT_CHANNEL				CHANNEL_UNUSED
#define PASSTHROUGH_D_OUTPUT_CHANNEL				CHANNEL_UNUSED
```

Here you can choose which of your radio/servo channels are reversed.  If a servo requires a reversed signal, make sure you first have your transmitter set up to correctly control the plane manually, and then set these settings so that the automatic controls will match the direction of your manual controls.

```
// Servo Reversing Configuration
// Here you can choose which reversing switches use hardware switches, and hard code the rest.
// Note that your servo reversing settings here should match what you set on your transmitter.
// For any of these that evaluate to 1 (either hardcoded or by flipping a switch on the board,
// as you define below), that servo will be sent reversed controls.
#define AILERON_CHANNEL_REVERSED				HW_SWITCH_1
#define ELEVATOR_CHANNEL_REVERSED				HW_SWITCH_2
#define RUDDER_CHANNEL_REVERSED					HW_SWITCH_3
#define AILERON_SECONDARY_CHANNEL_REVERSED			0 // Hardcoded to be unreversed, since we have only 3 switches.
#define THROTTLE_CHANNEL_REVERSED				0 // Set to 1 to hardcode a channel to be reversed
#define CAMERA_ROLL_CHANNEL_REVERSED				0
#define CAMERA_PITCH_CHANNEL_REVERSED				0
#define CAMERA_YAW_CHANNEL_REVERSED				0

// Set this to 1 if you need to switch the left and right elevon or vtail surfaces
#define ELEVON_VTAIL_SURFACES_REVERSED				0
```

Here you can set up the thresholds for your mode switching channel.  A 3-way switch works best for this channel, but a knob can also work, as could a 2-way switch combined with a trim for this channel.  These values are the positions where we switch between the low and middle, and then the middle and high positions of the 3-way mode switch.

```
// Mode Switch is ideally controlled by a 3-position switch on your transmitter.
// Often the Flap channel will be controlled by a 3-position switch.
// These are the thresholds for the cutoffs between low and middle, and between middle and high.
// Normal signals should fall within about 2000 - 4000.
#define MODE_SWITCH_THRESHOLD_LOW				2600
#define MODE_SWITCH_THRESHOLD_HIGH				3400
```


Setting up the Failsafe Chanel allows your plane to know when it loses transmitter signal, and act accordingly.  You can set it up to either return to where it was launched via a set of Failsafe/RTL waypoints, or to just continue following the main set of waypoints uninterrupted.

```
// The Failsafe Channel is the RX channel that is monitored for loss of signal
// Make sure this is set to a channel you actually have plugged into the UAV Dev Board!
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
#define FAILSAFE_INPUT_CHANNEL					THROTTLE_INPUT_CHANNEL
#define FAILSAFE_INPUT_MIN					1500
#define FAILSAFE_INPUT_MAX					4500

// FAILSAFE_TYPE controls the UDB's behavior when in failsafe mode due to loss of transmitter
// signal.  (Set to FAILSAFE_RTL or FAILSAFE_MAIN_FLIGHTPLAN.)
// 
// When using FAILSAFE_RTL (Return To Launch), the UDB will begin following the RTL flight plan
// as defined near the bottom of the waypoints.h or flightplan-logo.h files.  By default, this
// is set to return to a point above the location where the UDB was powered up, and to loiter there.
// See the waypoints.h or flightplan-logo.h files for info on modifying this behavior.
// 
// When set to FAILSAFE_MAIN_FLIGHTPLAN, the UDB will instead follow the main flight plan as
// defined in either waypoints.h or flightplan-logo.h.  If the UDB was already in waypoint mode
// when it lost signal, the plane will just continue following the main flight plan without
// starting them over.  And if the transmitter is still in waypoint mode when the UDB sees it
// again, the UDB will still continue following the main flight plan without restarting.  If
// the UDB loses signal while not in waypoint mode, it will start the main flight plan from the
// beginning.
#define FAILSAFE_TYPE						FAILSAFE_RTL

// When FAILSAFE_HOLD is set to 1, then once Failsafe has engaged, and you have subsequently
// regained your RC TX-RX connection, you will need to manually change the Mode Switch in order
// to exit Failsafe mode.  This avoids the situation where your plane flies in and out of range,
// and keeps switching into and out of Failsafe mode, which depending on your configuration,
// could be confusing and/or dangerous.
#define FAILSAFE_HOLD						0
```

## Telemetry and Serial Output

```
// Serial Output Format (Can be SERIAL_NONE, SERIAL_DEBUG, SERIAL_ARDUSTATION, SERIAL_UDB,
// SERIAL_UDB_EXTRA, or SERIAL_OSD_REMZIBI)
// This determines the format of the output sent out the spare serial port.
// Note that SERIAL_OSD_REMZIBI only works with GPS_UBX.
// SERIAL_UDB_EXTRA will add additional telemetry fields to those of SERIAL_UDB.
// SERIAL_UDB_EXTRA can be used with the OpenLog without characters being dropped.
// SERIAL_UDB_EXTRA may result in dropped characters if used with the XBEE wireless transmitter.
#define SERIAL_OUTPUT_FORMAT					SERIAL_NONE
```


## On Screen Display

The native OSD is still experimental, but you can enable it here.  More info on the [On Screen Display page](OSD.md).

```
// On Screen Display
// OSD_VIDEO_FORMAT can be set to either OSD_NTSC, or OSD_PAL
// To hide the callsign, set OSD_CALL_SIGN to just {0xFF}
#define USE_OSD							0
#define OSD_VIDEO_FORMAT					OSD_NTSC
#define OSD_SHOW_HORIZON					0
#define OSD_CALL_SIGN						{0x95, 0x8B, 0x81, 0x8C, 0x8D, 0x8E, 0xFF} // KA1BCD
```


## Trigger Action

```
// Trigger Action
// Use the trigger to do things like drop an item at a certain waypoint, or take a photo every
// N seconds during certain waypoint legs.

// TRIGGER_TYPE can be set to TRIGGER_TYPE_NONE, TRIGGER_TYPE_SERVO, or TRIGGER_TYPE_DIGITAL.
// If using TRIGGER_TYPE_SERVO, set the TRIGGER_OUTPUT_CHANNEL above to choose which output channel
// receives trigger events, and set the TRIGGER_SERVO_LOW and TRIGGER_SERVO_HIGH values below.
// If using TRIGGER_TYPE_DIGITAL, the trigger will be on pin RE4.  In this case make sure to set
// NUM_OUTPUTS to be less than 6 to avoid a conflict between digital output and servo output on
// that pin.

// TRIGGER_ACTION can be: TRIGGER_PULSE_HIGH, TRIGGER_PULSE_LOW, TRIGGER_TOGGLE, or TRIGGER_REPEATING
// The trigger action output is always either low or high.  In servo mode, low and high are servo
// values set below.  In digital mode, low and high are 0V and 5V on pin RE4.
// The action is triggered when starting on a waypoint leg that includes the F_TRIGGER flag (see the
// waypoints.h file).
// If set to TRIGGER_PULSE_HIGH or TRIGGER_PULSE_LOW, then the output will pulse high or low for the
// number of milliseconds set by TRIGGER_PULSE_DURATION.
// If set to TRIGGER_TOGGLE, the output will just switch from high to low, or low to high each time
// the action is triggered.
// If set to TRIGGER_REPEATING, then during any waypoint leg with F_TRIGGER set, high pulses will be
// sent every TRIGGER_REPEAT_PERIOD milliseconds.

// Note, durations in milliseconds are rounded down to the nearest 25ms.

#define TRIGGER_TYPE						TRIGGER_TYPE_NONE
#define TRIGGER_ACTION						TRIGGER_PULSE_HIGH
#define TRIGGER_SERVO_LOW					2000
#define TRIGGER_SERVO_HIGH					4000
#define TRIGGER_PULSE_DURATION					250
#define TRIGGER_REPEAT_PERIOD					4000
```


## Gains

Servo Saturation controls the maximum throw of the servos.  Typical value is 1.0.  Maximum recommended value is 1.0, maximum valid value is 1.999.  Lower values will stop servos from moving too far.

```
// SERVOSAT limits servo throw by controlling pulse width saturation.
// set it to 1.0 if you want full servo throw, otherwise set it to the portion that you want
#define SERVOSAT						1.0
```


There are several gains that you can adjust. They should all be set to positive numbers.  If you think any gain needs to be reversed, you should instead set up reversals in the Reversal section above.  Use the same gains all board types -- the appropriate multipliers are now built into the firmware.

  * ROLLKP - This is the proportional feedback for the aileron control of roll. Setting it higher will improve precision of the bank leveling, but will reduce the bank angle and will make the turning radius get larger. Setting it too high may cause low frequency roll flutter that is annoying but not dangerous. Setting it lower will increase the bank angle and sharpen the turns, particularly during RTL. Typical value is 0.25.
  * ROLLKD - This is the derivative (gyro) feedback for the aileron control of roll. It is used to improve the damping of the roll control, to dampen any low frequency flutter. But if it is set too high, there may be a high frequency flutter that is annoying but not dangerous. Typical value is 0.125. This gain does not have to be greater than or equal to ROLLKP, you can use any value that you want.
  * YAWKP\_AILERON - This is the proportional turning gain used by navigation for controlling ailerons. Typical value is 0.1. Larger values will produce tighter turns. Using a value that is too large will produce a "dutch roll".  Maximum valid value is 1.999.
  * YAWKD\_AILERON - This is the derivative yaw gain used for yaw damping by yaw stabilization to reduce the impact of the wind, and to help stabilize the yaw control. Typical value is 0.2. Maximum valid value is 0.5.
  * AILERON\_BOOST - This is an amplification, or "boost" factor for manual control of the ailerons, used during stabilized and waypoint modes to restore control authority to the ailerons in the face of the damping effect of stabilization. Typical value is 1.0. This factor is in addition to the manual control, so a value of PITCHBOOST of 0 turns the boost off, and provides unmodified response to manual control. A PITCHBOOST of 1 makes the elevator response to manual control approximately twice as great. Maximum valid value is 1.999.

```
// Aileron/Roll Control Gains
// ROLLKP is the proportional gain
// ROLLKD is the derivative (gyro) gain
// YAWKP_AILERON is the proportional feedback gain for ailerons in response to yaw error
// YAWKD_AILERON is the derivative feedback gain for ailerons in response to yaw rotation
// AILERON_BOOST is the additional gain multiplier for the manually commanded aileron deflection
#define ROLLKP							0.20
#define ROLLKD							0.05
#define YAWKP_AILERON						0.10
#define YAWKD_AILERON						0.05
#define AILERON_BOOST						1.00
```


  * PITCHGAIN - This is the proportional feedback for the elevator control of pitch. Setting it higher will improve precision of the pitch leveling. If you set it too high, it may cause pitch flutter, but it is not dangerous, just annoying. Typical value is 0.250. Maximum valid value is 1.999.
  * PITCHKD - This is the pitch rate (measured in the earth coordinate system!) damping feedback for the elevator. Typical value is 0.25. Maximum valid value is (0.50\*SCALEGYRO).
  * RUDDER\_ELEV\_MIX - This is the amount of rudder-elevator mixing that you want. Typical value is 0.5. Set this parameter to 0 if you do not want to use this mixing. Maximum valid value is 1.999.
  * ROLL\_ELEV\_MIX - This is the amount of roll-elevator mixing that you want. Typical value is 0.1. Set this parameter to 0 if you do not want to use this mixing. Maximum valid value is 1.999.
  * ELEVATOR\_BOOST - This is an amplification, or "boost" factor for manual control of the elevator, used during stabilized mode to restore control authority to the elevator in the face of the damping effect of stabilization. Typical value is 0.5. This factor is in addition to the manual control, so a value of ELEVATOR\_BOOST of 0 turns the boost off, and provides unmodified response to manual control. A ELEVATOR\_BOOST of 1 makes the elevator response to manual control approximately twice as great. Maximum valid value is 1.999.
  * INVERTED\_NEUTRAL\_PITCH - This is the number of degrees that the plane should pitch the nose up while flying inverted, in order to maintain altitude.

```
// Elevator/Pitch Control Gains
// PITCHGAIN is the pitch stabilization gain, typically around 0.125
// PITCHKD feedback gain for pitch damping, around 0.0625
// RUDDER_ELEV_MIX is the degree of elevator adjustment for rudder and banking
// AILERON_ELEV_MIX is the degree of elevator adjustment for aileron
// ELEVATOR_BOOST is the additional gain multiplier for the manually commanded elevator deflection
#define PITCHGAIN						0.10
#define PITCHKD							0.04
#define RUDDER_ELEV_MIX						0.20
#define ROLL_ELEV_MIX						0.05
#define ELEVATOR_BOOST						0.50

// Neutral pitch angle of the plane (in degrees) when flying inverted
// Use this to add extra "up" elevator while the plane is inverted, to avoid losing altitude.
#define INVERTED_NEUTRAL_PITCH	 				8.0
```


  * YAWKP\_RUDDER - This is the navigation turning gain for using the rudder to make a turn. Typical value is 0.1. Larger values will produce tighter turns. Using a value that is too large will produce a "dutch roll". Maximum valid value is 1.999.
  * YAWKD\_RUDDER - This is a yaw damping term used both by stabilization and navigation to reduce the impact of the wind, and to help stabilize the yaw control. Maximum valid value is 0.5.
  * ROLLKP\_RUDDER - This is a stabilization gain that applies rudder in response to the plane's roll angle.  This is only helpful on planes that will roll in response to rudder.
  * MANUAL\_AILERON\_RUDDER\_MIX - This can be used to improve the responsiveness of manually commanded aimeron-based turns while in stabilized mode.  It's there to inhibit the rudder's tendency to fight a turn while in stabilized mode.
  * RUDDER\_BOOST - This is an amplification, or "boost" factor for manual control of the rudder, used during stabilized mode to restore control authority to the rudder in the face of the damping effect of stabilization. Typical value is 1.0. This factor is in addition to the manual control, so a value of RUDDER\_BOOST of 0 turns the boost off, and provides unmodified response to manual control. A RUDDER\_BOOST of 1 makes the rudder response to manual control approximately twice as great. Maximum valid value is 1.999.

```
// Rudder/Yaw Control Gains
// YAWKP_RUDDER is the proportional feedback gain for rudder navigation
// YAWKD_RUDDER is the yaw gyro feedback gain for the rudder in reponse to yaw rotation
// ROLLKP_RUDDER is the feedback gain for the rudder in response to the current roll angle
// MANUAL_AILERON_RUDDER_MIX is the fraction of manual aileron control to mix into the rudder when
// in stabilized or waypoint mode.  This mainly helps aileron-initiated turning while in stabilized.
// RUDDER_BOOST is the additional gain multiplier for the manually commanded rudder deflection
#define YAWKP_RUDDER						0.05
#define YAWKD_RUDDER						0.05
#define ROLLKP_RUDDER						0.06
#define MANUAL_AILERON_RUDDER_MIX				0.00
#define RUDDER_BOOST						1.00
```

Set up the gains for vertical, nose-up hovering here.  These gains are only used if hovering is enabled above.

```
// Gains for Hovering
// Gains are named based on plane's frame of reference (roll means ailerons)
// HOVER_ROLLKP is the roll-proportional feedback gain applied to the ailerons while navigating a hover
// HOVER_ROLLKD is the roll gyro feedback gain applied to ailerons while stabilizing a hover
// HOVER_PITCHGAIN is the pitch-proportional feedback gain applied to the elevator while stabilizing a hover
// HOVER_PITCHKD is the pitch gyro feedback gain applied to elevator while stabilizing a hover
// HOVER_PITCH_OFFSET is the neutral pitch angle for the plane (in degrees) while stabilizing a hover
// HOVER_YAWKP is the yaw-proportional feedback gain applied to the rudder while stabilizing a hover
// HOVER_YAWKD is the yaw gyro feedback gain applied to rudder while stabilizing a hover
// HOVER_YAW_OFFSET is the neutral yaw angle for the plane (in degrees) while stabilizing a hover
// HOVER_PITCH_TOWARDS_WP is the max angle in degrees to pitch the nose down towards the WP while navigating
// HOVER_NAV_MAX_PITCH_RADIUS is the radius around a waypoint in meters, within which the HOVER_PITCH_TOWARDS_WP
//                            value is proportionally scaled down.
#define HOVER_ROLLKP						0.05
#define HOVER_ROLLKD						0.05
#define HOVER_PITCHGAIN						0.2
#define HOVER_PITCHKD						0.25
#define HOVER_PITCH_OFFSET					0.0	// + leans towards top, - leans towards bottom
#define HOVER_YAWKP						0.2
#define HOVER_YAWKD						0.25
#define HOVER_YAW_OFFSET					0.0
#define HOVER_PITCH_TOWARDS_WP					30.0
#define HOVER_NAV_MAX_PITCH_RADIUS				20
```

One or two servo channels can be used to stabilize and target a camera mounted on a pan/tilt gimball.

```
// Camera Stabilization and Targeting
// 
// In Manual Mode the camera is fixed straight ahead.
// In Stabilized Mode, the camera stabilizes in the pitch axis but stabilizes a constant yaw
// relative to the plane's frame of reference.
// In Waypoint Mode, the direction of the camera is driven from a flight camera plan in waypoints.h
// In all three flight modes, if you set CAMERA_*_INPUT_CHANNEL then the transmitter camera controls
// will override the camera stabilisation. This allows a pilot to override the camera stabilization dynamically
// during flight and point the camera at a specific target of interest.
// 
// To save cpu cycles, you will need to pre-compute the tangent of the desired pitch of the camera
// when in stabilized mode. This should be expressed in 2:14 format. 
// Example: You require the camera to be pitched down by 15 degrees from the horizon in stabilized mode.
// Paste the following line into a google search box (without the //)
// tan((( 15 /180 )* 3.1416 ))* 16384
// The result, as an integer, will be 4390. Change the angle, 15, for whatever angle you would like.
// Note that CAM_TAN_PITCH_IN_STABILIZED_MODE should not exceed 32767 (integer overflows to negative).

#define CAM_TAN_PITCH_IN_STABILIZED_MODE  			1433	// 1443 is 5 degrees of pitch. Example: 15 degrees is 4389
#define CAM_YAW_IN_STABILIZED_MODE				0 // in degrees relative to the plane's yaw axis.    Example: 0

// Camera values to set at installation of camera servos
// All number should be integers
#define CAM_PITCH_SERVO_THROW					95	// Camera lens rotation at maximum PWM change (2000 to 4000), in degrees.          
#define CAM_PITCH_SERVO_MAX					85	// Max pitch up that plane can tilt and keep camera level, in degrees.  
#define CAM_PITCH_SERVO_MIN					-22 // Max pitch down that plane can tilt and keep camera level, in degrees. 
#define CAM_PITCH_OFFSET_CENTRED				38 // Offset in degrees of servo that results in a level camera.           
											    // Example: 30 would mean that a centered pitch servo points the camera
												// 30 degrees down from horizontal when looking to the front of the plane.

#define CAM_YAW_SERVO_THROW					350	// Camera yaw movement for maximum yaw PWM change (2000 to 4000) in Degrees. 
#define CAM_YAW_SERVO_MAX					130 // Max positive yaw of camera relative to front of plane in Degrees. 		     
#define CAM_YAW_SERVO_MIN					-130 // Min reverse  yaw of camera relative to front of plane in Degrees.   
#define CAM_YAW_OFFSET_CENTRED					11	// Yaw offset in degrees that results in camera pointing forward. 

// Camera test mode will move the yaw from + 90 degrees to + 90 degrees every 5 seconds. (180 degree turn around)
// That will show whether the CAM_PITCH_SERVO_THROW value is set correctly for your servo.
// Once the camera rotates correctly through 180 degrees, then you can adjust CAM_PITCH_OFFSET_CENTRED to center the camera.
// In Camera test mode, pitch angle changes permanently to 90 degrees down in stabilized mode, and  0 (level) in Manual Mode.

#define CAM_TESTING_OVERIDE					0 // Set to 1 for camera to move to test angles in stabilized mode.
#define CAM_TESTING_YAW_ANGLE			 		90 // e.g. 90 degrees. Will try to swing 90 degrees left, then 90 degrees right
#define CAM_TESTING_PITCH_ANGLE					90 // In degrees.
```



## Altitude Hold

These settings are only used when Altitude Hold is enabled above.

  * HEIGHT\_TARGET\_MIN - This is the minimum target height, in meters, used in stabilized mode.
  * HEIGHT\_TARGET\_MAX - This is the maximum target height, in meters, above the launch point. Typical value is 100. The commanded height for altitude hold is proportional to the throttle, up to this maximum height. Altitude hold will command full throttle until the plane is within 50 meters of the commanded height. It will gradually reduce throttle as it climbs higher. It will reduce to minimum throttle at the commanded height. If it continues to climb higher, the motor will be cut off completely.
  * HEIGHT\_MARGIN - The vertical range, in meters, to try to keep the plane within when altitude hold is enabled.
  * ALT\_HOLD\_THROTTLE\_MIN - This parameter sets a value for the minimum amount of throttle during altitude hold. Typical value is 0.55, define a value between 0.0 and 1.0
  * ALT\_HOLD\_THROTTLE\_MAX - This parameter sets a value for the maximum amount of throttle during altitude hold. Typical value is 1.0, define a value between 0.0 and 1.0
  * ALT\_HOLD\_PITCH\_MIN - This is the pitch angle, in degrees, that the control will attempt to hold the plane's pitch, at minimum throttle. The suggested value for this parameter for a sailplane is 0. If you want the altitude hold feature to maintain altitude without turning off the motor, select a slightly negative value for this parameter, such as -2. If you do not want to use this feature, set it to 0. Otherwise, set it to the pitch angle that you would normally control the plane at minimum throttle. Positive values means the nose points upward, negative values means the nose pitches downward.
  * ALT\_HOLD\_PITCH\_MAX - This is the pitch angle, in degrees, that the control will attempt to hold the plane's pitch, at MAXIMUM throttle. The suggested value for this parameter is 10. Otherwise, set the pitch angle that you would normally control the plane at maximum throttle. Positive values means the nose points upward, negative values means the nose pitches downward.
  * ALT\_HOLD\_PITCH\_HIGH - This is the pitch angle, in degrees, that the control will attempt to hold the plane's pitch, at ZERO throttle. In other words, this is the pitch angle that you want when your sailplane is gliding, or when your plane is flying "dead stick". The suggested value for this parameter is 0. If you do not want to use this feature, set it to 0. Otherwise, set it to the pitch angle that you would normally control the plane at zero throttle. Positive values means the nose points upward, negative values means the nose pitches downward.

```
// Min and Max target heights in meters.  These only apply to stabilized mode.
#define HEIGHT_TARGET_MIN					25.0
#define HEIGHT_TARGET_MAX					100.0

// The range of altitude within which to linearly vary the throttle
// and pitch to maintain altitude.  A bigger value makes altitude hold
// smoother, and is suggested for very fast planes.
#define HEIGHT_MARGIN						20

// Use ALT_HOLD_THROTTLE_MAX when below HEIGHT_MARGIN of the target height.
// Interpolate between ALT_HOLD_THROTTLE_MAX and ALT_HOLD_THROTTLE_MIN
// when within HEIGHT_MARGIN of the target height.
// Use ALT_HOLD_THROTTLE_MIN when above HEIGHT_MARGIN of the target height.
// Throttle values are from 0.0 - 1.0.
#define ALT_HOLD_THROTTLE_MIN					0.35
#define ALT_HOLD_THROTTLE_MAX					1.0

// Use ALT_HOLD_PITCH_MAX when below HEIGHT_MARGIN of the target height.
// Interpolate between ALT_HOLD_PITCH_MAX and ALT_HOLD_PITCH_MIN when
// within HEIGHT_MARGIN of the target height.
// Use ALT_HOLD_PITCH_HIGH when above HEIGHT_MARGIN of the target height.
// Pitch values are in degrees.  Negative values pitch the plane down.
#define ALT_HOLD_PITCH_MIN					-15.0
#define ALT_HOLD_PITCH_MAX					15.0
#define ALT_HOLD_PITCH_HIGH					-15.0

// The throttle will be turned off for an F_LAND waypoint.
// The plane will use the line between the F_LAND waypoint and the previous
// waypoint to define a target glide slope that serves as a ceiling during landing.
```


RTL\_PITCH\_DOWN is used if you want the nose to pitch down during return to launch. It is the same as in previous versions of AileronAssist. If you use this feature, enter a positive value for this parameter, which is the angle, in degrees, that you want the nose to pitch down during RTL. The idea is that you may have a dead motor and a head wind during a loss of signal RTL, so you might be willing to sacrifice some altitude to get your plane back to you. Some pilots use this feature, some do not. Typical value if you use it is 2. If you do not use it, set it to zero. Maximum recommended value is 10.0.

```
// Return To Launch Pitch Down in degrees, a real number.
// this is the real angle in degrees that the nose of the plane will pitch downward during a return to launch.
// it is used to increase speed (and wind penetration) during a return to launch.
// set it to zero if you do not want to use this feature.
// This only takes effect when entering RTL mode, which only happens when the plane loses the transmitter signal.
#define RTL_PITCH_DOWN						0.0
```


## Hardware In the Loop Simulation

Set this when using the UDB to control the X-Plane simulator instead of controlling a real plane.  See the [wiki page](HardwareInLoop.md).

```
// Hardware In the Loop Simulation
// Only set this to 1 for testing in the simulator.  Do not try to fly with this set to 1!
// See the MatrixPilot wiki for more info on using HILSIM.
// HILSIM_BAUD is the serial speed for communications with the X-Plane plugin.  Default is
// 19200, but 230400 is a good speedy option.  Make sure the X-Plane plugin's Setup file has
// its speed set to match.
#define HILSIM 								0
#define HILSIM_BAUD							19200
```


## Flight Plan Handling

```
// Flight Plan handling
//
// You can define your flightplan either using the UDB Waypoints format, or using UDB Logo
// Set this to either FP_WAYPOINTS or FP_LOGO
// The Waypoint definitions and options are located in the waypoints.h file.
// The Logo flight plan definitions and options are located in the flightplan-logo.h file.
#define FLIGHT_PLAN_TYPE						FP_WAYPOINTS
```


The waypoint handling options live at the top of both the waypoints.h file, and the flightplan-logo.h file.  Note that as of MatrixPilot 3.0, the fixed origin option now includes altitude, and that it is expressed as a floating point value in meters above sea level.

```
// Move on to the next waypoint when getting within this distance of the current goal (in meters)
#define WAYPOINT_RADIUS 						25

// Origin Location
// When using relative waypoints, the default is to interpret those waypoints as relative to the
// plane's power-up location.  Here you can choose to use any specific, fixed 3D location as the
// origin point for your relative waypoints.
//
// USE_FIXED_ORIGIN should be 0 to use the power-up location as the origin for relative waypoints.
// Set it to 1 to use a fixed location as the origin, no matter where you power up.
// FIXED_ORIGIN_LOCATION is the location to use as the origin for relative waypoints.  It uses the
// format { X, Y, Z } where:
// X is Logitude in degrees * 10^7
// Y is Latitude in degrees * 10^7
// Z is altitude above sea level, in meters, as a floating point value.
// 
// If you are using waypoints for an autonomous landing, it is a good idea to set the altitude value
// to be the altitude of the landing point, and then express the heights of all of the waypoints with
// respect to the landing point.
// If you are using OpenLog, an easy way to determine the altitude of your landing point is to
// examine the telemetry after a flight, take a look in the .csv file, it will be easy to spot the
// altitude, expressed in meters.

#define USE_FIXED_ORIGIN						0
#define FIXED_ORIGIN_LOCATION						{ -1219950467, 374124664, 30.0 }
									// A point in Baylands Park in Sunnyvale, CA
```