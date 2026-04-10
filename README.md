# README

## BOM

RS232 converter 
https://www.digikey.com/en/products/detail/texas-instruments/TRSF3232EIPWR/1909461?s=N4IgTCBcDaICoCUDKAxAzGDBRAkgBQHUEQBdAXyA

ESP32-S3-Mini
https://www.digikey.com/en/products/detail/espressif-systems/ESP32-S3-MINI-1-N4R2/22597234
https://learn.adafruit.com/assets/110822

Power Supply
https://www.digikey.com/en/products/detail/recom-power/RPX-4-0-R/14312389


## TODO: 

PRofessionallism stuff:
1. Name your schematics
2. Name your nets properly
3. Make sure there are no ERC warnings or errors


- [x] watch KiCAD tutorials
- [x] part 7
- [ ] learn about PCB concepts

Watch this video to learn how to name the schematic and stuff
https://www.youtube.com/watch?v=0Q6gU7-QqUg&list=PLEBQazB0HUyQ5YJSdCBb79orXaR3Uk5vm&index=2

Place Symbol ("A" hotkey)
"M" to move a symbol (does not move wires)
"G" to grab a symbol (moves wires)
ERC checker and power flags

Assigning footprints
https://www.youtube.com/watch?v=4YSZwcUSgJo&list=PLEBQazB0HUyQ5YJSdCBb79orXaR3Uk5vm&index=5

Laying out the PCB
https://www.youtube.com/watch?v=zdieFtrvjew&list=PLEBQazB0HUyQ5YJSdCBb79orXaR3Uk5vm&index=6
Board setup button
"air wires"
X - trace mode
D - drag trace
G - grab trace
V - create via-hole mid-trace
U - select all segments in a trace
use edge cuts layer to draw board edge

front and back copper filled zones (what are these?)
hand-solder footprints

Gerber files


Questions:
- how do I use the DXF file to create the board?
- how do I find the parts?

- PCBWAY
- what are the PCB specs? (defaults should handle it)
    - min trace spacing
    - min trace width
    - min drill size
    - pad size
- 12 mil spacing for power

- are we doing surface mount or through-hole, and for which parts?
    - all surface mount, through-hole for pins
- design preferences? close together? spread apart? what is better for manufacturing?

### 

0603 for capacitors

Output from GPS module: 9-36V
Output from Buck Converter: 3.3V

- [x] get all the parts
- [x] email nicholas for example schematic
- [x] Wire up ESP32
    - [x] boot
    - [x] Reset
    - [x] USB-C
- [x] Wire up buck converter
    - [x] capacitors
Desired voltage 3.3
RFB1 10K
RFB2 4k32
Cout 52uF
    - [x] connect to ESP32
- [x] wire up RS232
    - [x] caps and resistors
    - [x] connect to everything else
- [x] clean up schematic

Q: what do we do with PG in Buck converter?

- [x] get outine of board
- [x] assign footprints
- [x] capacitors and resistors
    - [x] 0603 for capacitors

- [x] create board outline from DXF
- [x] do routing and stuff
- [ ] which traces need to go to which pins in the middle?

### Things to Study:
- blocks
- pcb manufacturing
- DRC
- set up redmine for documentation

```
for all resistors and capacitors you can use 0603 or 0805, diodes can be SMA, LEDs can also be 0603 or 0805, and kicad should have some footprints for some common pushbutton switches.

For the 52uF it would have to be 1206 most likely.. instead of putting +V_OUT its better just to put the actual voltage, like the +3V3 symbol, and then you dont need the power flags as well (unless you want to pass ERC everywhere)
usually you determine footprints for resistors and capacitors based on what is generally available on digi-key and mouser, but mostly they should all be 0603 and 0805
you can also just place a couple 22uF in parallel to get the 52uF since thats a more common size and we can reduce the BOM. Each additional part equates to an additional feeder on a pick and place and costs more to setup
the ESP should have a 22uF near the VIN pin, and you can use that some one 3x on the output of the buck converter
```