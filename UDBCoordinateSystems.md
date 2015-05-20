# UAV DevBoard Coordinate Systems
Peter Hollands, December 2009
with thanks to William Premerlani for detailed review and corrections.

The software inside of the UAV DevBoard uses 4 coordinate systems.

These are:-
  * Aviation Convention - Airplane
  * Aviation Convention - Earth
  * UAV DevBoard – Earth
  * GPS Convention – Lat / Lon

Why 4? In summary, it is because UAV DevBoard has been a learning journey for the team. If we started again, we could make this slightly simpler, but at this stage, it is easier to leave the software as it is, and to provide you with this document.

## AVIATION CONVENTION – AIRPLANE

![http://gentlenav.googlecode.com/svn/wiki/images/udb_coords_fig1.jpg](http://gentlenav.googlecode.com/svn/wiki/images/udb_coords_fig1.jpg)

We use a variation of the aviation convention for the coordinates for vectors measured in the frame of reference of the plane, such as accelerometer and gyro signals. It is not exactly the same as the standard aviation convention. It came out the way simply because the design of the board included X and Y arrow labels that agreed with the mounting of the three-axis accelerometer on the board. Since then, we discovered that the industry standard aviation convention is rotated 90 degrees from the aviation convention used in the UAV DevBoard firmware.

There is a supporting document to this document which explains the mathematics that is used by the UAV DevBoard firmware.  That document is called the Direction Cosine Matrix IMU: Theory. See http://gentlenav.googlecode.com/files/DCMDraft2.pdf . The X and Y axis in the Theory document are the reverse of this document.

In the system used for UAV DevBoard, angles are defined using the mathematical convention. Positive angles are measured counter-clockwise when looking along an axis toward the origin. For example, a positive pitch angle change corresponds to the plane pitching downward.

Some of the Z angles are shown in the previous figure, which are measured from the X axis. X and Y angles are not shown. Another way of describing how angles are measured is that angles follow the “right hand rule”. That is, if you take your right hand, and place your thumb in the direction of any axis, then your remaining fingers, when slightly clenched will point in the direction of a positive angle around that axis.

<img src='http://gentlenav.googlecode.com/svn/wiki/images/righ_hand_small.jpg' width='100' height='100'>

This hand would need to be rotated down so that the thumb points downwards in line with the positive direction of the Z axis.<br>
<br>
<h2>AVIATION CONVENTION – EARTH</h2>


<img src='http://gentlenav.googlecode.com/svn/wiki/images/udb_coords_fig2.jpg' />

<b>Aviation Convention – Earth</b> is exactly the same as <b>Aviation Convention – Plane</b>. It is just that the two coordinate systems move independently as the plane banks, turns, dives and “loops the loop” around the sky. The two coordinate systems can be related to each other by a mathematical “translation” of the origins of each coordinate system and by a rotation. The latter rotation is specified by the Direction Cosine Matrix, also known as “rmat” (rotation matrix) in the firmware of the UAV DevBoard. The <b>Aviation Convention – Plane</b> coordinate system coincides with the <b>Aviation Convention – Earth</b> coordinate system when the plane is located at the origin of the <b>Aviation Convention – Earth</b> coordinate system, pointing north, with the plane level with respect to both pitch and roll.<br>
<br>
The mathematics of this approach is described in a document called Direction Cosine Matrix IMU: Theory. (see <a href='http://gentlenav.googlecode.com/files/DCMDraft2.pdf'>http://gentlenav.googlecode.com/files/DCMDraft2.pdf</a> ) . Note, as also mentioned above, that this document was written before the axis labels were printed on the UAV DevBoard manufactured by Spark Fun, and so the X and Y axis in that document are the reverse of this document.<br>
<br>
<h2>UAV DEVBOARD - EARTH</h2>

This coordinate system is used by software within the UAV DevBoard to represent relative distances from where the plane’s computer was started up.  Using relative distances, allows the computer to save on memory and CPU cycles as the variables can then be sized as 16 bit variables, rather than 32 bit variables. (Note that 16 bit values mean that the computer can travel at most 2 to the power 15 <a href='.md'>the 16th bit  is the sign bit </a> i.e. 32768 meters or 20.36 miles from it’s origin before navigation variables will overflow. )<br>
<br>
<img src='http://gentlenav.googlecode.com/svn/wiki/images/udb_coords_fig3.jpg' />

Distances along the X axis and Y axis are measured in meters from the location where the plane first acquired a GPS location. This is usually the place where the plane’s UAV DevBoard is started up on the airfield. Note that the X Axis of <b>UAV DevBoard – Earth</b> is reversed when compared to <b>Aviation Convention – Earth</b>.<br>
<br>
Positive Z is the height of the plane above where it first was started up. The Z axis of <b>UAV DevBoard – Earth</b> is the reverse of that which is used for <b>Aviation Convention – Earth</b>.<br>
<br>
Angles in the UAV DevBoard system are measured in the mathematical convention of counter clockwise, starting from the east. So this convention is also using the right hand rule.<br>
<br>
<h2>GPS CONVENTION – LAT/LON</h2>

<img src='http://gentlenav.googlecode.com/svn/wiki/images/udb_coords_fig4.jpg' />

This coordinate system is the normal navigational convention used by shipping and aircraft since the seventeenth century. We use it because we receive positional information from the Global Positioning System (GPS) in this format.<br>
<br>
<h2>CONVERTING BETWEEN COORDINATE SYSTEMS</h2>

We will now explore how the UAV DevBoard firmware converts between coordinate systems. In this section all links to actual code are links to revision 202 (on Google Code) of MatrixPilot.<br>
<br>
As an example, we will take the most extreme case of conversion that is required. That is the case when the UAV DevBoard is controlling servos on the plane to point a camera at a known GPS location on the ground. The plane might be upside down, undertaking aerobatics as it is taking pictures, but the UAV DevBoard software can keep the camera pointing in the right direction.(subject to the mechanical abilities of the servos).<br>
<br>
<h3>From GPS Convention – Lat / Long to UAV DevBoard – Earth</h3>
Consider that we want a camera to point continuously at a ground location which is 40 meters north and 20 meters east of where we start up the plane. This type of definition of a waypoint or place mark is usually defined in the file waypoint.h using the conventions of <b>UAV DevBoard – Earth</b> . For example: ( 20, 40, 0 )<br>
<br>
The firmware receives the position of the plane as Lat, Long and Altitude information from a GPS receiver. For example: +524744707 latitude -10948199 longitude, 7800 altitude.  Lat and Long are delivered to the code by a routine called commit_gps_data. Lat and Long are delivered multiplied by 10 to the power 7. Altitude is delivered as meters multiplied by 100. So the above locations are in fact +52.4744707 latitude, -1.0948199 longitude and78.00 meters. This data is framed within the <b>GPS Convention – Lat / Lon</b> discussed above.<br>
<br>
A new aircraft position is received from the GPS. The code in gpsParseCommon.c calls a routine called navigate() which converts this data into the coordinate system called <b>UAV DevBoard – Earth</b>.  Here is a fragment of that code:-<br>
<br>
<pre><code>// in meters, range is about 20 miles<br>
accum_nav.WW = ((lat_gps.WW - lat_origin.WW)/90) ; vector_to_origin.y =  \<br>
                              - accum_nav._.W0 ;<br>
GPSlocation.y = accum_nav._.W0 ;<br>
        <br>
heightlong.WW = ( alt_sl_gps.WW - alt_origin.WW)/100 ; // height in meters<br>
height = heightlong._.W0 ;<br>
GPSlocation.z = height ;<br>
        <br>
//      multiply the longitude delta by the cosine of the latitude<br>
accum_nav.WW = ((long_gps.WW - long_origin.WW)/90) ; // in meters<br>
accum_nav.WW = ((__builtin_mulss ( cos_lat , accum_nav._.W0 )&lt;&lt;2)) ;<br>
vector_to_origin.x = - accum_nav._.W1 ;<br>
GPSlocation.x = accum_nav._.W1<br>
</code></pre>

The code uses a 32 bit temporary variable called accum_nav.WW to subtract the latitude at start-up (lat_origin) from its current location (lat_gps). The result when divide by 90 converts the difference in latitude into meters. Keeping the difference as a 32 bit quantity is no longer required as the amount will be relatively small.<br>
<pre><code>GPSlocation.y = accum_nav._.W0 ;<br>
</code></pre>
will take the bottom 16 bits of the distance from the origin and store it as the variable GPSlocation.y . So latitude in the <b>GPS Convention Lat / Lon</b> converts easily to the Y axis of the <b>UAV DevBoard - Earth</b>. No change of sign is required.<br>
<br>
A similar process is done for the longitude. However the distance associated with a change in longitude depends at what latitude the distance is measured. If you think about it for a moment, one can move through 180 degrees of longitude at the North Pole and travel no distance. You can simply turn around. While 180 degrees of longitude traveled at the equator is more than 12000 miles. So in the code above, the code first finds the difference in longitude:<br>
<pre><code> accum_nav.WW = ((long_gps.WW - long_origin.WW)/90) ; <br>
</code></pre>
and then multiplies that answer by the cosine of the latitude. So at the North Pole, where the latitude is 90 degrees, the cosine would be 0, and so the distance moved would be correctly computed as 0. While at the equator, where the latitude is 0, the cosine would be 1, and the distance would remain exactly as computed on the previous line.<br>
<br>
So to summarize: Coordinate conversion from <b>GPS Convention – Lat / Lon</b> to <b>UAV DevBoard – Earth</b> is easy. The axis of both conventions are aligned and in the same direction. The only small complication it that the distance moved by a change in longitude depends partly on what latitude the plane is at.<br>
<br>
For completeness, I’m going to also discuss the conversion of angles. This is not required for the camera example above, but it is required for correcting yaw gyro drift using the Course Over the Ground (COG) from the GPS. COG is received from the GPS as a value like 32679 in the routine commit_gps_data() . That means a course of 326.79 degrees. It is converted from <b>GPS Convention – Lat / Lon</b> to <b>UAV DevBoard - Earth</b> in the routine called estYawDrift(). Two lines achieve the conversion:-<br>
<pre><code>accum.WW = __builtin_muluu ( COURSEDEG_2_BYTECIR , cog_gps.BB ) ;<br>
actual_dir = -accum.__.B2 + 64 ;<br>
</code></pre>
The first line converts the angle from degrees to ByteCircular.  Degrees represents angle in a circle from 0 -360 degrees. ByteCircular represents angles in a circle with values from 0-255.  One ByteCircular is equal to around 1.4 degrees.<br>
<br>
The second line reverse the direction of the angle (negates the angle), and then adds on 90 degrees which is 64 in ByteCircular. If you look carefully at the convention diagrams above, you can confirm that due north is 000 in the <b>GPS Convention – Lat / Lon</b> and is 90 Degrees (or more specifically + 64 ByteCircular) in the <b>UAV DevBoard – Earth</b> convention.<br>
<br>
<h3>From UAV DevBoard – Earth to Aviation Convention – Earth</h3>
Picking up again on the camera view example: The plane now knows its position within the <b>UAV DevBoard - Earth</b> convention as:  GPSlocation.y, GPSlocation.x, GPSlocation.z  which are distances in meters from the origin of the plane a start-up.<br>
<br>
The desired location to view with the camera was 40 meters north and 20 meters east of the origin at start-up. The code in waypoints.c could look something like:-<br>
<pre><code>void compute_camera_view (void)<br>
{<br>
// For now camera view_location set here and not in waypoints.h<br>
// The following are in meters and are relative to the origin.<br>
	<br>
	<br>
// view location is 20 meters to the east of where the plane starts up.<br>
	view_location.x = 20  ;<br>
	view_location.y = 40<br>
	view_location.z = 0;<br>
<br>
	camera_view.x = (view_location.x - GPSlocation.x);<br>
<br>
	camera_view.y =  (view_location.y - GPSlocation.y) ;<br>
	camera_view.z =  (view_location.z - GPSlocation.z) ;<br>
}<br>
</code></pre>
That means that camera_view is simply a vector within the <b>UAV DevBoard – Earth</b> coordinate system.<br>
<br>
That vector, in order to be useful for pointing the camera on the plane, needs to be converted first to the Aviation Convention – Ground reference, and then rotated into the <b>Aviation Convention – Airplane</b> reference.<br>
<br>
For a vector, that simply means swapping the signs of the X axis and the Z axis. The figure below shows this quite clearly.<br>
<br>
<img src='http://gentlenav.googlecode.com/svn/wiki/images/udb_coords_fig5.jpg' />

To convert from the <b>Aviation Convention - Earth</b> convention to the <b>UAV DevBoard - Earth</b>, values of Y remain the same. Values of X must be negated (The X axis of the plane is opposite to that of the ground). Values for Z must also be negated.<br>
<br>
With regard to yaw angles measured in each system: basically, the two coordinates are flipped 180 degrees with respect to each other, so they rotate in opposite directions, starting 180 degrees apart.<br>
<br>
In the code for pointing the camera, this could be done in cameraCntrl.c as follows:-<br>
<pre><code>// Convert from "UAV DevBoard - Earth" convention to "Aviation Convention - Earth"<br>
cam_vector_ground[0] = - camera_view.x ;<br>
cam_vector_ground[1] =   camera_view.y ;<br>
cam_vector_ground[2] = - camera_view.z ;<br>
</code></pre>

<h3>From Aviation Convention – Earth to Aviation Convention – Plane</h3>

The servos in the plane are aligned with the axis in the <b>Aviation Convention – Airlane</b>. It is easiest to set the servos, if the cam_vector_ground is now converted into that coordinate system. This is easily achieved by using the Direction Cosines Matrix which is constantly tracking the relative rotation between the <b>Aviation Convention – Airlane</b> Axes and the <b>Aviation Convention – Ground</b> Axes.  We simply need to take the inverse (which is also the transpose) of the rotation matrix, rmat, and multiply that inverse rotation onto the cam_vector_ground. Here is some code that will do that:-<br>
<pre><code>MatrixTranspose(3, 3, rmat_transpose, rmat ) ;<br>
// It does not matter that the result of this operation is not the expected magnitude<br>
// The code only uses the ratios of X,Y,Z relative to each other to calculate angles.<br>
MatrixMultiply( 3 , 3 , 1 , cam_vector_plane , \<br>
       rmat_transpose ,cam_vector_ground ) ;<br>
</code></pre>
So we now have the vector that points at the camera in the plane’s coordinate reference system. It is called cam_vector_plane.<br>
<br>
The two Aviation Conventions are completely similar apart from rotations of their axis. So no further conversion is necessary.<br>
<br>
To summarize: Conversion from <b>Aviation Convention – Earth</b> to <b>Aviation Convention – Airplane</b> of vectors is simply a question of multiplying the inverse of the rotation matrix, rmat .<br>
<br>
However, to point the turret servo in the right direction, there is yet still another conversion that must be done. <b>Aviation Convention – Airlane</b>, would have a yaw angle as 0 as pointing out down the left wing (the X axis). But normal usage of the word yaw would have a yaw angle of 0 with reference to the plane as pointing directly forward of the plane in line with the fuselage. This the natural yaw angle to use for a turret servo that is going to point in proportion to the cam_vector plane. So we now have to subtract 90 degrees onto any heading angle calculated from <b>Aviation Convention – Airlane</b>, in order to make it line up with the fuselage.  This 90 degree rotation is achieved in the code below, by subtracting 64 units of ByteCircular (90 degrees) onto the yaw.<br>
<br>
<pre><code>// Yaw <br>
	matrix_accum.x =   cam_vector_plane[0] ;<br>
	matrix_accum.y =   cam_vector_plane[1] ; <br>
	yaw = rect_to_polar(&amp;matrix_accum) - 64; // subtract 90 degrees<br>
	// Pitch<br>
	matrix_accum.y = cam_vector_plane[2] ;<br>
	pitch = rect_to_polar(&amp;matrix_accum) ;<br>
</code></pre>

The yaw and pitch in this calculation relate entirely to the plane’s of reference. They determine the angle of pitch and yaw relative to the plane to achieve the to desired camera view.<br>
<br>
The Rmat rotational matrix has converted a desired camera vector based on the earth’s reference, into a desired vector with reference to the plane’s axis, and finally converted that into yaw and pitch angles.(The software does this 40 times / second). These values of yaw and pitch, will always point at the right location on the earth. So now it is now a fairly simple exercise to rotate the camera servos, which are also operating relative to the plane.<br>
<br>
For pitch the key line of code is:-<br>
<pre><code>cam.WW = __builtin_mulss((pitchServoLimit(pitch - pitch_offset_centred)), pitch_servo_ratio) ;<br>
</code></pre>
and for yaw the key line of code is:-<br>
<pre><code>cam.WW = __builtin_mulss((yawServoLimit(yaw - yaw_offset_centred)), yaw_servo_ratio) ;<br>
</code></pre>

<h2>Conclusion</h2>

So there are primarily 4 coordinate systems in use in the UAV DevBoard:<br>
<br>
<ul><li>Aviation Convention - Airplane<br>
</li><li>Aviation Convention - Earth<br>
</li><li>UAV DevBoard – Earth<br>
</li><li>GPS Convention – Lat / Lon</li></ul>

With the camera example, we have traversed those systems in reverse order to show how each coordinate system relates to the previous one.