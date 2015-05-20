# Telemetry Formats

MatrixPilot supports two core formats of telemetry

  * SERIAL\_UDB\_EXTRA - which is a home grown ASCII human readable format which is supported on all current versions of MatrixPilot and all devices (UDB2, UDB3, UDB4, UDB5, AUAV3). Please see [this document](https://docs.google.com/document/pub?id=1bPovneDV1UXBBEQE6yC3Ms9CCI_wmecLGtVzUkJeoIw) for a detailed explanation of the SERIAL\_UDB\_EXTRA format.

  * SERIAL\_MAVLINK - provides support for the [MAVLink](http://www.qgroundcontrol.org/mavlink/start) protocol for telemetry in small UAV's.
    * [Initial introduction](UsingMAVLINK.md) to using MAVLink with MatrixPilot
    * [Oveview of MatrixPilot MAVLink Code structure](MavlinkCodeStructure.md)
    * [Auto-generation of C and python support functions from MAVLink definition XML files](MavlinkGeneratingHeaders.md)
    * [How to specify that a MatrixPilot variable can be changed by a MAVLink compatible Ground Control Station (GCS)](MavlinkAddingParametersToReadOrWrite.md)