# Tuning Gains to make the Autopilot turn more easily

This note  was written for MatrixPilot version 3.2.1.
To keep things simple in this note, I am going to assume that the rudder is not being used at all to turn the plane. The plane is turning using ailerons to bank.

If you have a good understanding of tuning the gains in stabilization mode, it's then easier to understand tuning gains in waypoint / autonomous mode.

When in stabilized mode, the autopilot tries to fly straight and level. When the real pilot mixes in some aileron to bank, the autopilot still tries to fly straight and level, but does not completely correct the error introduced by the pillots manual adjustment of ailerons. (There is no integral term in the PID loop.) The result is that  the plane ends up stabilized in a small bank angle.  If ROLLKP is high, then the pilot will find it difficult to turn a stabilized plane, because a high ROLLKP will cause the autopilot to be agressive in stabilizing for straight and level flight. if ROLLKP is low, the plane is less stabilized, but the pilot will find that the roll control from the  transmitter (aileron control) is much more sensitive.

So to make the plane turn more in stabilized mode either a) you the pilot has to use more agressive controls, or b) the stabilization gains have to be turned down.

It very common for new users of MatrixPilot to have their stabilization gains set too high. The plane is often over stabilized, and so hard to turn. An experienced pilot will know immediately if the plane is over stabilized, because it's rather hard to get the plane to bank and turn.

Now when the plane flies in Waypoint mode it flies in the same way, as when in stabilisation mode.

In Waypoint mode, the stabilization software still runs. And the role of the real pilot, is replaced by software .... let's call it the "auto navigation pilot."  YAWKP and YAWKD control the gains for the "auto navigator". YAWKP causes the plane to turn in proportional to the difference between the desired and actual heading. YAWKD resists turning by generating negative feedback proportional to yaw rate. Turning up YAWKP is just like the real pilot becoming more agressive in using the transmitter controls to turn the plane.

ROLLKP and ROLLKD are still working in autonomous mode to stabilize theplane. So if you turn those down. you reduce stabilization, but make it easier for the YAWKP and YAWKD terms to turn the plane.
So in general you have two choices to make turns more agressive.

1) Turn down ROLLKP and ROLLKD stabilization or

2) Turn up YAWKP and turn down YAWKD that control the "auto navigator".

Note: In general for a normal trainer type plane, the ROLLKD terms and the YAWKD  terms (rate terms) can be set to zero. These derivative terms are designed to stop "overshoot" of the controls. But the design of a traditional aircraft (particularly trainers) will automatically provide additional stability, and stop the plane from overshooting.

As ROLLKD and YAWKD are using "the rate of change of roll" and "rate of change of yaw" as input signals, they add quite a bit of complexity to tuning the aircraft. That is why, if your plane is a conventional intermediate or easy to fly plane, it may be better to set ROLLKD and YAWKD to zero initially.

Planes that may need the ROLLKD and YAWKD terms to be non zero are aerobatic planes and flying wings.