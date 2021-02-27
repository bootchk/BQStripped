##KiCad design files for a minimal circuit board using Texas Instruments part BQ25504 energy harvester.

Stripped means the voltage monitoring and battery management features of the BQ25504 are not used in the normal fashion.  Instead, if there is enough energy to harvest, the board charges the power out pins. Voltage out ranges from 0 to >6 volts. Typically used with a solar cell input and a tantalum capacitor output.

See the BQ25504 data sheet and solar example circuits.

Also shared at OSHPark (the tested Gerbers, i.e. the PDB fabrication files.)

(Also, I have another design which uses a load switch and programs VBAT_OK as control to the loadswitch so that output voltage switches on hard at say 1.9V and limits output voltage to 3.6V (the specs for a Nordic NRF52 radio chip.)  The board is slightly larger.  It uses the same sort of tricks to share resistor nets (which granted, are not that large in area.)  I would be glad to share that design also, but I haven't tested it yet.)

###Use Case

The BQ25504 (and thus the board) boosts voltage from small, low-voltage solar cells (panels) and also provides MPPT to optimize power extraction from the solar cell.

You don’t need this board if you don’t need either of those two functions.  For example if you use a solar panel with enough cells to give you enough voltage, and if your downstream voltage monitoring keeps the storage voltage near the solar panels MPP voltage (where it delivers the most power.)

###More on Stripped Design

The design programs the under-voltage, over-voltage, over-temperature, and in-regulation (VBAT_OK) thresholds to extremes by connecting various BQ25504 pins to ground or VRDIV.  This saves board space, by omitting resistor nets normally used to program the thresholds. For example, under voltage threshold is programmed to zero, and over voltage threshold is programmed much greater than the chips maximum rated voltage.

Typically you use a downstream, separate voltage monitor and/or regulator.  Typically, your downstream avoids draining storage below 1.8V (VCHGEN) so the BQ25504 does not enter coldstart, where it provides very little power.


###Interface

The board has power-in and power-out.  For power-in, there are two pairs of pins: a pair of through holes and a pair of pads on the back.  For power-out, there are three pairs of pins: two pairs of through hole and one pair of pads on the back.  Some of the pairs are optional, you can choose which to use.

The power-in pads accept a small, 5mm SMD photodiode (e.g. TEMD5080) used as a solar cell (which is a smallish solar cell that, in full sun, provides some power) if you want a solar cell on this board.  Often, you wire on a larger solar cell.

The power-out pads accept a small SMD capacitor, say in 3528 metric package, if you want storage on this board.

The third set of power-out pins exist to connect your load to the board (as well as connecting storage to the board.)

###Storage

You must provide storage, typically a capacitor greater than 0.1mF.  See the BQ25504 data sheet under “Choosing components.” Typical storage is a tantalum or super capacitor.

The board does NOT include a small, ceramic capacitor across power out, which might be needed to reduce ripple on power-out.

###Cautions

Since the battery management functions are not used, you should not use a chemical battery for storage without careful consideration of the possible consequences:
- low battery life due to undercharging
- risk of fire due to overcharging.

###BOM

You can generate one from the KiCad files.  See the BQ25504 data sheet for other suggestions.  Lands for the discretes are for 603 packages.  Lands for inductor L1 are for a part made by Wurth.

###Images

In the repository are:

  - PDF of schematic
  - PNG image of the front of the PCB (but its not pretty enough to help much in placing parts.)

###About the design

Certain pins of the BQ chip 'sample' resistor nets to program the chip's voltage regulation functions.  The reference designs from TI use separate resistor nets for each 'program'.  But you can share one resistor net among 'programs.'  That saves board space, but might limit the results of the programs (given the constraint that each resistor net must add up to say 10 meg ohm.)  Here, I eliminate the resistor nets altogether and connect the programming pins directory to reference voltages for the programming nets (VRDIV and ground.)  In the schematic, see the network labelel "High Prog", and the fact that the VBAT_OV pin is grounded.

For more information about these tricks, on the net is a presentation by the chip designer, where you can find a schematic for chip internals with respect to these programming pins.  Also, there is some discussion in the TI forums.

Note this design still has a resistor net for MPPT programming.  In my opinion, thats a waste also, since very few users will program MPPT to other than a rough value of 80% (of the solar cell's Voc.)
