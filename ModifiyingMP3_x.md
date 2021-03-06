# Introduction
This describes easy ways to develop code modules for MatrixPilot.

Aims are:
  * Do this in a safe way
  * Do it in an easy way
  * Make the changes maintainable
This topic does not include bugfixes.  Those are core problems which need to be addressed in the released code.

This will also cover creating a new project in a new directory, leaving the original project unchanged.



**The basics**

If you need different behavoir and functions in MatrixPilot, you will need to add or modify code.

The plan is to overide the existing code with new code, not add to it.

Adding or modifying the core code makes changes for everyone.  This is dangerous and difficult to test.




**What to overide**

You need to find the right place for an overiding function.

Try to find the function that you dont want to run.
This function should only be doing that one function, no more.
The function should preferably not be calling subfunctions of very different actions

If you can not find the right function to overide, ask someone.
It might not exist.  In this case we need to improve the core code to accept changes.



**An example**


Lets say we wish to change the servo mixers.

_The original method - copying MatrixPilot_

  1. Define a new airframe type

> 2. Change options.h for the new airframe

> 3. Hunt through all of the code for anthing that used the old airframe type

> 4. Modify all of the #if(AIRFRAME\_TYPE == ??) options in all of the code to fit the new airframe

> 5. Add a new #if(AIRFRAME\_TYPE == ??) into the servoMix function in servoMix.c

> 6. Insert code

> 7. Test and debug all points of modification


_The alternative_

  1. Copy servoMix.c to a reasonable new name, say tricopterMix.c

> 2. Delete the contents of the servoMix function including any if statements contained

> 3. Insert your new code into the blank function

> 4. Delete servoMix from your MPLAB project

> 5. Add the new file - tricopterMix.c to the project

> 6. Debug only the new function



**Adding a new project**

Maybe you have modified a few files and wish to create a different project for them.

  1. Create a suitable application project in parrallel with the MatrixPilot, libUDB and libDCM folder

> 2. Copy the MPLAB project file from the MatrixPilot folder to the new folder.

The MPLAB project is different for different boards:
UDB2/3 = MatrixPilot.mcp
UDB4 = MatrixPilot\_udb4.mcp

> 3. Rename the project to have a suitable title

> 4. Open the new project in MPLAB

> 5. It will have lost links to all the files in MatrixPilot (headers and implementations).  You need to fix these links.

> 6. Now you need to let the MatrixPilot files how to find each other:

Follwing the menus, tabs, dropdown boxes etc..

Project->Build Options->project->Directories->include search path

Project->Build Options->project->Directories->assembler include path
> add to both the search folder path "..\MatrixPilot"


The modified files can be moved to the new folder if you would like to keep them away from the original files.

Then build and enjoy.