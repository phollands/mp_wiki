# UDB Logo

Here's where I'll explain how to describe flight plans using the UDB's variant of the LOGO programming language.

For now, the best documentation is the [flightplan-logo.h file iteslf](http://code.google.com/p/gentlenav/source/browse/trunk/MatrixPilot/flightplan-logo.h).


## UDB Logo Editor

Also check out the UDB Logo Editor.  It is located at:

Tools/UDB\_Logo\_Editor/udb\_logo\_editor.html

in the download.

This tool lets you run UDB Logo programs on a Google Maps page, to preview  the flight plan.  It's great for learning and debugging.  I suggest testing all UDB Logo programs in this tool before flying them.

Note that there are a few UI elements in this tool that need some explanation:

  * The pink marker with a dot in the middle is the origin.  You can move this around to see how your flight would pan out with a different home / origin.
  * The Airplane icon represents where your plane is when you switch into waypoint mode.  (Different from home / origin location.)  Since UDB Logo programs begin from your current location, this can affect what happens as your program runs.
  * The big green dot and green line between it and the plane icon represent which direction your plane is pointed when switching into waypoint mode.  Move this around to see how it will affect your flight plan.