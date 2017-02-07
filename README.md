# Arduino-Model-Railroad-Turnout-Servos
Using an Arduino UnoR3 Mega2560 and two 32-bit servo controllers to control turnouts on a large N-scale model railroad layout

I have a large, complex layout, presently 20 ft long, with more than 40 turnouts (and will have possibly 50 to 60 once I am finished extending it around the 10'x17' room). I will be using an Arduino Uno R3 Mega 2560 board, but that board will not control that many servos. So I will be using two (possibly 3, but probably not) 32-bit servo controller boards (from China), listed as being for an "Arduino Biped Robot". Each of those boards can control 32 servos and can be daisy chained together, and would be connected to the Arduino board through one of the auxiliary serial connections (an Rx/Tx pair of pins -- not the ones connected to the USB jack). 

A few turnouts I will use relays to control under-servo solenoids, which I have several of for the Peco turnouts. (I will be using exclusively Peco turnouts, except for a few of the Kato double-crossovers, which have their own internal solenoids and can also be controlled by relays.) The Arduino Mega 2560 board can control the few relays needed.

That 32-bit servo controller is an interesting piece of electronics, but it is very hard to find any documentation on it, and almost impossibly to find any blogs saying how it can be used. But I found a manual for it at: 
http://www.elechouse.com/elechouse/... Controller V2/32 Servo Controller Manual.pdf
Information on that board is at:
http://www.elechouse.com/elechouse/index.php?main_page=product_info&cPath=100_146&products_id=1883

This is not the only 32-bit servo controller, and I found another one that is a shield for Arduino, but there are so many connections, that the board does not need to mounted on top of an Arduino board (and especially if one daisy chains a second 32 bit controller).  But the one above seems easy to use and I already have two of them.

I am having trouble finding any software to control the turnouts on the layout other than controling through DCC (digital command and control -- as opposed to just plain DC).  I plan to, at least initially, use JMRI, a free software product that can not only run DCC-equipped model train engines but also can control the layout turnouts.  But I can't find any documentation that shows how to interface JMRI with what would be a non-DCC turnout control system that uses Arduino.  The interface between a computer and either a DCC system, such as Digitrax, or an Arduino board is simple serial through a USB cable.

So initially, I'll be just skipping a computer interface for layout control and use some pushbuttons and LEDs.  I will also be installing "track block occupancy detectors" on certain blocks of track, to help prevent derailments from a train hitting a turnout from a "closed" direction.  A turnout is like a "Y", and if a train approaches the base of the "Y", the train will be routed to whichever track the turnout is set to.  But if a train approaches from one of the branches of the "Y" and the turnout is thrown to the other branch, then the engine -- and following cars -- will be derailed.  So I was thinking that programming code in the Arduino can also turn off power to a section of track if a derailment is about to occur.  But that involves not just block detection but motion and direction detection to be robust and accurate.  The logic to do that in software can get quite complex.

However, the turnouts (I use almost all Peco brand plus a few Kato brand) do power routing to the one "branch" track to which the turnout is thrown.  This allows using LEDs (with appropriate resistors and diodes) as "open" (safe) or "closed" indicators for each branch. but that requires two additional changes: 
  1.  The two central output (branch) tracks from the points have to have at least one extra track piece that is connected to the turnout with metal track joiners, and 
  2.  Those two extra track pieces have to be joined to the following track with insulated joiners.
  3.  The track beyond that insulator has to have main power routed to it with wires.
When the turnout is "thrown" to the right track, this will cause the left rail of the right track to have power all the way up to the insulator (and beyond due to the wiring to the next block), but the right rail of the left track will have power removed from it, at least up to the insulated joiner.  So if an engine comes into the left Y branch when the turnout is thrown to the right, it will suddenly hit a section of track that has no power and stop (unless the train has 3 engines in series that are together longer than the extension track).  Using this system instead of software to prevent turnout derailments is far simpler than trying to do it in software (which would inevitably involve insulating blocks of track anyway, plus having block detectors).

I hope that makes sense. 

NOTE: It is actually normal to have several sections of track connected to main power via wires that come up from underneath the layout --- forming a "block" section of track, even though those all sections of track in that block may be already connected through metal track connectors.  And in order to do block occupancy detection, one would insulate the track in one block from any connected blocks. That would allow automatically removing power from a Y-branch block of track if a turnout is thrown the other way, or if software detects an imminent collision.

One of my next steps is to lift up pieces of track in several places in order to add insulated track joiners (creating blocks of isolated track) and to make sure there are enough wires routing power to each new block.  (There are already a lot of wires coming up through small holes in the plywood next to pieces of track on the layout.  This is needed because of occasional poor electrical connections between rail sections and to just plain resistance.  In other words, even if you just have a simple track oval, it is safer to have the rail power connected to at least two sections of track, more if the oval is large.  

Also, having blocks of track isolated via insulated rail joiners makes troubleshooting shorts or bad track connections in case those type probleme pop up.  I've seen a large model rail club have to shut down a whole room of track -- during a weekend open house show -- in order to track down where a short existed.
