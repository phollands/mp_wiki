# Oveview of the MatrixPilot MAVLink Code Structure

Before reading this section, you should browse the [main mavlink website](http://qgroundcontrol.org/mavlink/start), to become familiar with some of the core concepts.

This section provides an overview of how MAVLink has been implemented in MatrixPilot. There are two primary parts:-

  * [MAVLink.c](http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/MAVLink.c#22) which replaces telemetry.c when MAVLink is selected in options.h
  * [MAVLink header files](http://code.google.com/p/gentlenav/source/browse/#svn%2Ftrunk%2FMAVLink%2Finclude%2Fcommon) which contain encoding and decoding subroutines.

# Sending MAVLink Messages

The main routine for sending MAVLink messages is called [mavlink\_output\_40hz()](http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/MAVLink.c#728) . The routine is called 40 times per second from function [dcm\_heartbeat\_callback()](http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/servoPrepare.c#110) found within source file servoPrepare.c .

mavlink\_output\_40Hz() simply prepares data for encoding into a number of different MAVLink message packets. These messages have a variety of datastream rates (e.g. some are sent at 8hz, and some sent at 1Hz etc). And the messages are staggered where possible over 1 second, so that the communications channel is not overloaded.

Let's take the mavlink heartbeat message as an example. It [starts with](http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/MAVLink.c#783)
```
if (mavlink_frequency_send(MAVLINK_RATE_HEARTBEAT, mavlink_counter_40hz + spread_transmission_load)) 
```
mavlink\_frequency\_send will check whether it is the right time within the 40hz frequency, to send the mavlink message, given the specification  [MAVLINK\_RATE\_HEARTBEAT](http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/mavlink_options.h#33), defined in mavlink\_options.h).

The code then calculates the mode of the plane, and prepares to send that in
```
mavlink_msg_heartbeat_send(MAVLINK_COMM_0, MAV_TYPE_FIXED_WING, MAV_AUTOPILOT_UDB, mavlink_base_mode, mavlink_custom_mode, MAV_STATE_ACTIVE);
```

A similar exercise is then done for a variety of MAVLink messages as in :-
  * [mavlink\_msg\_gps\_raw\_int\_send](http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/MAVLink.c#822)
  * [mavlink\_msg\_global\_position\_int\_send](http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/MAVLink.c#849)
  * [mavlink\_msg\_attitude\_send ](http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/MAVLink.c#899)

That is probably enough information, for now, on the structure of MAVLink.c for sending messages.

# MAVLink .h header files

MAVLink provides a large number of potential messages that can be used in MAVLink.c. We currently only use a fraction of the set of pre-defined messages. The messages are defined in two main header directories. Note that each individual MAVLink message is only included in the firmware image if the specific message is actually called in MAVLink.c.

There are two directories of MAVLink messages.
  * [common](http://code.google.com/p/gentlenav/source/browse/trunk/MAVLink/include/common/) which is used for MAVLink messages which are common to all autopilot implementations
  * [matrixpilot](http://code.google.com/p/gentlenav/source/browse/trunk/MAVLink/include/#include%2Fmatrixpilot) which are messages that have been created specifically for our own autopilot.

For now, you should just become familiar with some of the MAVLink subtroutine definitions. For example, the MAVLink heartbeat is in the common headers (common to all MAVLink enabled autopilots). mavlink\_msg\_heartbeat\_send, which we were using just now above, is defined in [this location](http://code.google.com/p/gentlenav/source/browse/trunk/MAVLink/include/common/mavlink_msg_heartbeat.h#143).

These header files contain subroutines not only for encoding mavlink messages, but also for parsing and decoding the messages. So how are MAVLink messages received and parsed ?

# Receiving MAVLink Messages

As each byte is received by the UART serial interrupt, a [routine is called in MAVLink.c](http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/MAVLink.c#311) called
```

void udb_serial_callback_received_byte(uint8_t rxchar)
{
        if (mavlink_parse_char(0, rxchar, &msg[mavlink_message_index], &r_mavlink_status))
        {
                // Check that handling of previous message has completed before calling again
                if (handling_of_message_completed == true)
                {
                        // Switch between incoming message buffers
                        if (mavlink_message_index == 0) mavlink_message_index = 1;
                        else mavlink_message_index = 0;
                        handling_of_message_completed = false;
                        trigger_event(mavlink_process_message_handle);
                }
        }
}
```

You will see that each received byte is sent to a parsing routine called mavlink\_parse\_char(). Whenever parsing is complete, a routine is going to be called indirectly via "trigger\_event", which will initiate a call to [handleMessage(void)](http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/MAVLink.c#602) running at a different priority level and a different thread. The variable mavlink\_message\_index switches between 1 and 0 to ensure that a different buffer is used for incoming messages, while the other message is being processed by handleMessage().

I should probably explain trigger\_event(mavlink\_process\_message\_handle) in some more detail. During initialisation, the MAVLink code [registers](http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/MAVLink.c#128) the routine handleMessage() with the event manager like this:-
```
mavlink_process_message_handle = register_event_p(&handleMessage, EVENT_PRIORITY_MEDIUM);
```
so the routine handleMessage() is registered with the event manager at EVENT\_PRIORITY\_MEDIUM. When triggered, handleMessage will run with an [interrupt priority level 2](http://code.google.com/p/gentlenav/source/browse/trunk/libUDB/interrupt.h#56).