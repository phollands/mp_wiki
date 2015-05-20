# MatrixPilot-IP

The Network interface for the UAV dev board (UDB) allows for a versitile, easy and fast access to data within the UDB. It supports TCP and UDP connections in either Server or Client modes. There are three hardware platforms supported all based on Microchip SPI external PHYs: one is WiFi and the other two are Ethernet. As of this writing the IP stack and it's accompaning modules are located in branches/MatrixPilot\_IP/Microchip/TCPIP and branches/MatrixPilot\_IP/MyIpNetwork. For now it is in MatrixPilot\_IPAfter further testing has been performed it will be merged to trunk.

Due to limited program memory in older UDB hardware, only the UDB4 or newer hardware platforms are supported.

Some fine print:
The TCP/IP stack used is maintained by Microchip and its license states that it is free to use as long as it is used with either a Microchip microprocessor or with one of their PHYs.

# Hardware Installation
There are three external SPI PHYs that are supported. They are:
[MRF24WG0Mx](http://www.microchipdirect.com/ProductSearch.aspx?Keywords=AC164149) - WiFi 802.11b/g x=A is for a built-in antenna for short-range. x=B uses an external antenna for longer range.
[ENC624J600](http://www.mouser.com/ProductDetail/Olimex-Ltd/MOD-ENC624J600/?qs=%2fha2pyFaduiM2FizGGE3eb3KhtErVhtMkHmAi73al27GsGQxpDnbNI%252bg4oGPqWoM) - Ethernet 10/100 MBit
[ENC28J60](http://www.microchipdirect.com/ProductSearch.aspx?Keywords=AC164123)   - Ethernet 10 MBit

## MRF24WG0Mx WiFi 802.11b/g



= Configuring MatrixPilot-IP
To configure the IP library, and it's accompanining modules, use the following files:
MatrixPilot/options.h - enable IP by selecting PHY and MatrixPIlot-IP modules
MyIpNetwork/MyIpOptions.h - configure MatrixPilot-IP modules: assign IP type (TCP/UDP), ports and server IPs
MyIpNetwork/HardwareProfile.h - hardware pin assignments for MCHP Stack
MyIpNetwork/TCPIP.h - Microchip TCP/IP stack config for stock library options such as DHCP, static IP, MAC addess, ping, FTP
MyIpNetwork/WF\_Config.h - Microchip TCP/IP stack config for WiFi options such as SSID and security passphrase



## Software configuration
The first step of installation is options.h. It is where you can enable/disable the entire IP stack for use with MatrixPilot by choosing which external PHY you are using. Enable it by defining them as (1) instead of (0). ONly one can be used at a time.
```
#define USE_WIFI_MRF24WG                  (0)
#define USE_ETHERNET_ENC624J600           (0)
#define USE_ETHERNET_ENC28J60             (0)
```

Next you'll want to enable which modules to use.
// Select which Network modules you would like to Enable
#define NETWORK\_USE\_UART1                 (0) // Forward UART1 data
#define NETWORK\_USE\_UART2                 (0) // Forward UART2 data
#define NETWORK\_USE\_FLYBYWIRE             (0) // Joystick -> flght surfaces (over the internet!)
#define NETWORK\_USE\_MAVLINK               (0) // Forward MAVLink data
#define NETWORK\_USE\_DEBUG                 (0) // Debug - Simple Telnet in ASCII
#define NETWORK\_USE\_ADSB                  (0)
#define NETWORK\_USE\_LOGO                  (0)
#define NETWORK\_USE\_CAM\_TRACKING          (0) // Camera Tracking, also set CAM\_USE\_EXTERNAL\_TARGET\_DATA=1
#define NETWORK\_USE\_GPSTEST               (0) // GPS spoof testing
#define NETWORK\_USE\_PWMREPORT             (0) // PWM pin states
#if (NETWORK\_USE\_UART2 == 1)
{ {},0,0,0,0,0,0,0,0,       eSourceUART2,eTCP, "www.google.com", 23},
{ {},0,0,0,0,0,0,0,0,       eSourceUART2,eTCP, "74.125.224.82", 24},
{ {},0,0,0,0,0,0,0,0,       eSourceUART2,eTCP, NULL, 25},
{ {},0,0,0,0,0,0,0,0,       eSourceUART2,eUDP, NULL, 26},
#endif
  1. efine MY\_DEFAULT\_SSID\_NAME                "mySSID"
#define MY\_DEFAULT\_WIFI\_SECURITY\_MODE       WF\_SECURITY\_OPEN
#define MY\_DEFAULT\_WIFI\_SECURITY\_MODE       WF\_SECURITY\_WPA\_AUTO\_WITH\_PASS\_PHRASE
#define MY\_DEFAULT\_PSK\_PHRASE               "MyPassword" // change this to your passphrase for your network
#define MY\_DEFAULT\_WIFI\_SECURITY\_MODE       WF\_SECURITY\_WEP\_40 // or WF\_SECURITY\_WEP\_104
#define MY\_DEFAULT\_WEP\_PHRASE               "WEP Phrase" // change this to your passphrase for your network#define USE_WIFI_MRF24WG                  (0)
#define USE_ETHERNET_ENC624J600           (0)
#define USE_ETHERNET_ENC28J60             (0)
}}}
The NETWORK_USE_UART2 is a popular one, I will use this one as an example. All data that is going in or out the UART2 serial port using the traditional means is internally copied to an internal queue. This can then be viewed or written to over IP. In the case of _UART2, you'll want to set SERIAL_OUTPUT_FORMAT to your preferred serial output so data will be genereted.

Now you need to configure the modules. This is done in /MyIpNetwork/MyIpOptions.h. There are four items to configure, the rest should be set as zeros.
{{{
}}}
Each line is defining all the variables of a global structure called MyIpData[] at compile time, it is a list of sockets. The zeros are several state variables that are used during runtime but the last four are constant values that are set once here and never change. The first one is the eSource type, this corresponds with the module name that you have enabled. The next one defines the IP socket type, either eTCP or eUDP. Thirdly is the field where we set server/client mode. If it is NULL the socket is a listening server, otherwise the socket is in client mode and will attempt to connect to the remote location. It will retry every few seconds forever. The string can be either a traditional numeric IP such as "74.125.224.82" or any DNS resolvable destination such as "myDynamicIP.no-ip.org" or "www.google.com". Lastly, the fourth value is the port number.

== WiFi configuration ==
The WiFi PHY has additional things to configure which are set in /MyIpNetwork/WF_Config.h. The WiFi hardware is a bit dumb and can not inteligently connect to any open network, it must be told exactly what network you want ot connecting to and how.
First set your SSID which is the name of yoru WiFi network.
{{{
}}}

Next configure your security settings. If you don't have any security then choose this option and you are done:
{{{
}}}

If you are using WEP, WPA or WPA2 you'll have to give it the passphrase too. It takes a little while for the PIC to convert the WPA passphrase into a key on every boot but it is easiest to configure so start off with that. Later on you can get fancy and do the conversion using the supplied wireshark link and hardcode the key (and change the security mode) to speed up boot-time.
{{{
}}}
Simialrly for WEP, use these options:
{{{
}}}

```