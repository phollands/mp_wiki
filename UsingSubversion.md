# Check out the development code

If you're running OS X or Unix/Linix, use the instructions [here](http://code.google.com/p/gentlenav/source/checkout).

If you're running Windows, as I assume most of us are for MPLab, then I suggest the following:

Grab a copy of [TortoiseSVN](http://tortoisesvn.net/downloads). It integrates svn nicely into windows explorer.

Once you have that installed, you can right click on the desktop or in a folder, and choose the "Svn Checkout..." option.  For the repository, enter: http://gentlenav.googlecode.com/svn/trunk/  The rest of the defaults should be fine.  That should get you a clean checkout of the most recent version of each firmware.  Note that it's fine to rename the resulting top level svn folder and move it around.

**Check out this [great video](http://vimeo.com/9245809) of Pete explaining the use of TortoiseSVN with MatrixPilot.**



### Important Note
If you want to fly using the development code from svn trunk, it's your responsibility to keep yourself informed about the current state of the code.  Here are some suggestions on how to do this:

  * Subscribe to this email list and watch for notes about code changes and bugs.
  * Subscribe to the commits list, where you'll see emails about each code change going into svn.  (Info on that list is here: http://groups.google.com/group/uavdevboard-commits )
  * Watch especially carefully for changes to the options.h file, and be sure to make the same changes to any other copies of the options.h file that you use.
  * Don't fly new versions of development code in any plane that you couldn't bear to lose.

Thanks for helping test the code!



# Create a patch

If you make changes that you'd like to share with others, or get feedback on, you can use TortoiseSVN to create a patch file to share.

First, you should update your local copy to include any changes that others have made since you initially checked your copy out.  Just right click the main svn folder (that includes trunk, branches, etc) and choose "SVN Update" to make sure you're seeing and editing the most recent versions of all the files.

Then right click the directory you care about, and choose TortoiseSVN->Check For Modifications.  That'll show you a pretty diff of all the changes you've made.  It's good to read through this carefully and make sure you're not making more changes than you meant to, nor more than are necessary.  Once your diff looks good, you can use the TortoiseSVN->Create Patch command to build a patch file (aka a diff file), and then send it out!