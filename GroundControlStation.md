# GroundControlStation (GCS)

One of the most important pieces in an UAV (Un-manned Aerial Vehicle) system is the GCS.
That is what we could call our user interface, or part of our user interface.
Some people fly their UAV's using ordinary radio control and FPV (First Person View) video systems, while others uses an on-board autopilot of some sort. The most basic auto pilots doesn't do very much more than keep the wings leveled at the same time it tries to fly towards a point in 3D space that the owner would like it to go.

Then there is the more sophisticated UAV systems, the ones that qualify for being a TRUE UAV, namely those that contains an on-board flight controller that is able to guide the aircraft through it's flight from start to finish, or in other words; from take-off to landing. These systems make up a TRUE UAV or what's bee known to most of us today as "drones" trough media reports of military hardware with the same basic functionality.




## Contentes of a GCS

What is then a GCS made up of?
Well, that could be a number of components, but there is some key ingredients:

  * A radio control system (RC system) for direct control of the UAV's control surfaces.
  * Often a video link from the UAV for live pictures of the flight.
  * A bi-directional data link.
  * Antennas. Often on a directional base capable of following the UAV for optimal reception of video and data transmitted.
  * A computer on the ground that runs the user interface.
  * Some sturdy racks or cases to mount all the hardware in.
The best solution for a professional grade, micro / or mini UAV would be a vehicle dedicated to the task since a complex system contains a lot of boxes and many meters of cable and connectors of all sorts and colors.

Needless to say, if a GCS isn't very well organized it could be a direct disaster, or rather the cause of one. Remember; the more cables and connectors there are in a system, especially if it's meant to be mobile and should be assembled and re-assembled many times, the more likely something will break. A break or bad connection in a critical component could render the system inoperable. (Trust me, I'm an old army diver and I can assure you  that the Murphy's Law is as real as any laws known to man).

Because of this you have to think through your needs of an GCS very carefully before staring to build one, or worse; starting to use something that someone else has built.
If you are in the "professional" category of UAV users you probably know all this and have everything well thought out and set up. You probably have a vehicle specially equipped for the UAV use.

For everyone else, who want to play around with this technology without spending too much on hardware there is some useful stuff out there, but there is also a lot of stuff that's not so useful. Both open source and not-so-open source. Be careful before trying to fly using something you haven't tested throughly on the ground. Be especial careful with GCS software that incorporates "flight instruments". Beware of the delay between the true UAV attitude and the attitude often presented on.screen at your GCS. If there is more than a few 10's of a second of delay, It's useless in my point of view. It might have some "bling" factor for your flight buddies at the field, but don't fly so far away that you NEED to use the instruments to get back (this is called flying IFR in the world of aviation IFR stans for Instrument Flight Rules and implies that you have to use instruments to orient yourself and your aircraft safely to the destination).

If the instrument deck on the GCS has more than a few 10's of a second you wil lloose the contrl of the aircraft if you fly it through turbulence. Since you have no force feedback (yet), you have no chance to correct a sudden attitude alteration.

As long as we haven't any GCS that gives INSTANT feedback on the instruments we fly by what's called VFR. (VFR stands for Visual Flight Rules and means that you don't fly without clear visual sight of the aircraft and it's attitude.
If you have a video link that provides you with clear imagery with well known landmarks you can fly VFR. As soon as your video system dies on you, you are in IFR and you better have a system that supports that kind of flying, or you will kill someone on the ground, especially if you fly anything larger than a park-flyer.

I know I sound pessimistic, but I want to make you think before setting of in a direction that you can't return from. If you make a SMALL mistake with an UAV, it may cost you a whole lot of money and someone else their life.

Since this is an open source project I will not mention any software or hardware that doesn't follow these rules as of now.

None of the software or hardware that is mentioned in this article, or linked to, is to be taken as recommendations of some sort. It's just an orientation of what is available on the market today, and what might lie in the future. We MAY do a "recommendation list" or rate some products, but that will ultimate mean than some products will be classified as GARBAGE.

This article is just a starter. It will be complemented and expanded in the next few days.
I also want to have some feedback regarding the open source policy we should use herein.




Marcus Fahlén Dec. 7 2010