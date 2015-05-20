# Filesystem

In order to enable filesytem support in your MatrixPilot build modify options.h to include the line:

#define USE\_FILESYSTEM 1

This will enable a small filesytem on the UDB4/5 onboard EEPROM, or AUAV3 onboard FLASH.

For larger filesystem support attach an external uSD socket/card to an SPI bus and define