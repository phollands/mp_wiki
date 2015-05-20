# Generating Mavlink C and Python Functions

The C header files for mavlink in matrixpilot can be automatically generated from source XML files which define each of the mavlink messages. These source files are kept in [Tools/MAVLink/mavlink/message\_definitions](http://code.google.com/p/gentlenav/source/browse/trunk/#trunk%2FTools%2FMAVLink%2Fmavlink%2Fmessage_definitions%2Fv1.0). There is a [source xml file  for "common" mavlink messages](http://code.google.com/p/gentlenav/source/browse/trunk/Tools/MAVLink/mavlink/message_definitions/v1.0/matrixpilot.xml), meaning that these messages are "common" to all autopilots that supports mavlink. And there is an xml file for [messages](http://code.google.com/p/gentlenav/source/browse/trunk/Tools/MAVLink/mavlink/message_definitions/v1.0/matrixpilot.xml) that are unique to matrixpilot. For example special messages to support backwards compatibility with our previous telemetry format called SERIAL\_UDB\_EXTRA.

Generation of new messages is not something that most developers will want to do. There are well over a hundred existing potential messages that are already defined. But should it be necessary to add a new mavlink message, then the process is  well stuctured.

  1. Edit the source XML definition file (probably matrixpilot.xml)
  1. Run gen\_matrixpilot.py
  1. Compile and run the test suite to check all messages function correctly

# Source XML Definition Example

Here is an example of the source XML for the mavlink Heartbeat message from within common.xml :-
```
<message id="0" name="HEARTBEAT">
               <description>The heartbeat message shows that a system is present and responding. The type of the MAV and Autopilot hardware allow the receiving system to treat further messages from this system appropriate (e.g. by laying out the user interface based on the autopilot).</description>
               <field type="uint8_t" name="type">Type of the MAV (quadrotor, helicopter, etc., up to 15 types, defined in MAV_TYPE ENUM)</field>
               <field type="uint8_t" name="autopilot">Autopilot type / class. defined in MAV_AUTOPILOT ENUM</field>
               <field type="uint8_t" name="base_mode">System mode bitfield, see MAV_MODE_FLAGS ENUM in mavlink/include/mavlink_types.h</field>
               <field type="uint32_t" name="custom_mode">A bitfield for use for autopilot-specific flags.</field>
               <field type="uint8_t" name="system_status">System status flag, see MAV_STATE ENUM</field>
               <field type="uint8_t_mavlink_version" name="mavlink_version">MAVLink version, not writable by user, gets added by protocol because of magic data type: uint8_t_mavlink_version</field>
          </message>
```

# Automatic Generation

gen\_MatrixPilot.py (found in [Tool/MAVLink/mavlink/pymavlink/generator](http://code.google.com/p/gentlenav/source/browse/trunk/#trunk%2FTools%2FMAVLink%2Fmavlink%2Fpymavlink%2Fgenerator) )will generate all of the C headers within the mavlink generation directories, before copying over the new header files to the main matrixpilot MAVLink headers directory.

One of the benefits of the autogeneration system, is that all the parsers for the messages are created automatically both in C and in Python. This makes it easy to add new messages (like SERIAL\_UDB\_EXTRA messages), and then receive them in a python program, like flan.pyw (Flight Analyzer) or in MAVProxy.

After running gen\_MatrixPilot.py, the above xml heartbeat message will be:-

  1. [Created in C code](http://code.google.com/p/gentlenav/source/browse/trunk/MAVLink/include/common/mavlink_msg_heartbeat.h) in the MAVlink headers file for MatrixPilot
  1. [automatically be called from the auto generated test sutie](http://code.google.com/p/gentlenav/source/browse/trunk/MAVLink/include/common/testsuite.h#27)
  1. [have a python class generated for the heartbeat message](http://code.google.com/p/gentlenav/source/browse/trunk/Tools/MAVLink/mavlink/pymavlink/mavlinkv10.py#950) along with parsing and [encoding functions](http://code.google.com/p/gentlenav/source/browse/trunk/Tools/MAVLink/mavlink/pymavlink/mavlinkv10.py#3548).

# Running the Test Suite

Compiling the test suite suite, ends up including about 500 subroutines for testing purposes in the UDB. Unlike the day to day usage of MAVLink, the auto test program is now large and only just fits in the UDB's firmware (UDB4 supports 256K bytes). It may be necessary in the future to split the auto testsuite into multiple tests that are loaded and tested in section in the UDB.

The automatic test suite for enncoding and decoding and testing all messages is [enabled in MAVLink.c](http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/MAVLink.c#54) by setting the following variable to 1:
```
#define MAVLINK_TEST_ENCODE_DECODE      0
```

Please note the instructions in MAVLink.c with regard to setting the options in the compiler for the "Large memory code model".

The test firmware will output the number of "passes" and "fails" on the serial interface. All tests should pass.