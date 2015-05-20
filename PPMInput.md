# PPM Input

As of MatrixPilot 3.0, it is possible to use the [DIYDrones PPM Encoder board](http://store.diydrones.com/product_p/br-ppme.htm) to increase the number of inputs and outputs available to MatrixPilot.

If you solder female servo headers onto this board, it can plug directly into receivers that use 0.1" spacing, like most Spektrum receivers.  Then you just plug one servo extension cable from the PPM port of the encoder board into RC Input channel 4 on the UDB.

Note that all 8 channels on the PPM encoder board must be connected to RC channels on your receiver in order to function at all.  if your receiver has fewer than 8 channels, just connect the last channel on your receiver to all the rest of the channels on the encoder board.  For example, my Spektrum AR6100 receiver has 6 channels, so I connect the signal pin of channel 6 on the receiver to the signal pins for channels 6, 7, and 8 on the encoder board.

Then set USE\_PPM\_INPUT to 1 in options.h.

You'll now have up to 8 input channels available to you in the code, and will have extra output channels available, since you can use the physical In1, In2, and In3 ports as extra servo output channels.