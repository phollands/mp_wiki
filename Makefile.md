# Build System (Makefile)

As of 2015 the project is supported by a ‘make’ based build system. The master makefile resides in the head of the source tree, but you do not want to run make directly from there. Instead you need to use one of the build-all scripts:

build-all.bat (Windows)
build-all.sh (Linux – note you may need to ‘chmod +x’ first)

The build process requires the GNU build tools installed, and suitable cross-compilers for any additional targets beyond SILSIM (Software In the Loop SIMulation). Linux users will likely have most of the required tools already installed, however a few small utilities may be required to be added into the system path on Windows platforms.


versions:    Windows    Linux
make
gcc
sed
sort
[
python 2.7

[Microchip XC16 Compiler](http://www.microchip.com/pagehandler/en_us/devtools/mplabxc/)

Windows users can also build with the legacy Microchip C30 compiler if installed.

MPLAB and Visual Studio projects can be generated for the current source tree with the Tools/build/make-build.py script.

All build system output is to a generated ‘./build’ subdirectory.

## Portability

As of MatrixPilot 3.4 we're targeting not just the dsPIC based controller boards, but also other platforms when building the code for Software-In-the-Loop [(SILSIM)](SoftwareInLoop.md) Simulation.  This means that we need to keep the code portable.  Things to keep in mind:
  * Windows 7
  * Windows 8
  * Ubuntu 8.2
  * Ubuntu 12.x