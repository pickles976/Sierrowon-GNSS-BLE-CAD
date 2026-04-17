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

- [x] print out PCB on paper
- [ ] add 12-pin header to board

12 pins We will need CS, SCK, MOSI, MISO, 2x GND,  12V(GNSS Vin), 3.3V, 4 x IO Pins

 SPI pins on the S3 and be set in firmware and can be any arrangement from I04-IO14 and IO2

## 12-Pin Connector Pin Assignment

| Signal | Module Pin | GPIO | Notes |
|--------|-----------|------|-------|
| GND | — | — | Power rail |
| GND | — | — | Power rail |
| 3.3V | — | — | Power rail |
| 12V (GNSS Vin) | — | — | Power rail |
| CS | 34 | IO38 | SPI — cleanest GPIO in range |
| SCK | 32 | IO36 | SPI — native FSPICLK |
| MOSI | 31 | IO35 | SPI — native FSPID |
| MISO | 33 | IO37 | SPI — native FSPIQ |
| IO_1 | 35 | IO39 | JTAG-capable, free as GPIO |
| IO_2 | 36 | IO40 | JTAG-capable, free as GPIO |
| IO_3 | 37 | IO41 | JTAG-capable, free as GPIO |
| IO_4 | 38 | IO42 | JTAG-capable, free as GPIO |

> **Note:** Avoid using module pin 26 (IO26) as GPIO — on the N4R2, IO26 is internally tied to PSRAM in QIO mode and will cause corruption if driven externally. Avoid pins 39–40 (GPIO43/44, UART0 TX/RX) and pin 41 (IO45, VDD_SPI strapping pin). Pins 31–33 (IO35–IO37) have eFuse-determined defaults but are free in QIO flash mode. Pins 35–38 (IO39–IO42) are JTAG by default but freely reassignable; USB-JTAG still works regardless.

+12V trace is 2A, needs to be 1.5mm

Cheers
https://documentation.espressif.com/esp32-s3_datasheet_en.pdf#cd-pins-peri-assignment Page 27
https://documentation.espressif.com/esp32-s3-mini-1_mini-1u_datasheet_en.pdf Page 20


PRofessionallism stuff:
1. Name your schematics
2. Name your nets properly
3. Make sure there are no ERC warnings or errors

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

Gerber files

- PCBWAY
- what are the PCB specs? (defaults should handle it)
    - min trace spacing
    - min trace width
    - min drill size
    - pad size
- 12 mil spacing for power

### 

Output from GPS module: 9-36V
Output from Buck Converter: 3.3V

Q: what do we do with PG in Buck converter?

- [ ] fix ERC errors?

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

Ok I dont have a lot of time to go through this in detail, but some things I noticed in layout

Try to create more space between the USB-C connector and the ESP's antenna, preferrably swaping places with the USB-C and the 12 pin
Remove the little sliver of ground pour above the antenna, I would just move the ESP32 up more to eliminate that and get it right on the edge of the board.
I would add a lot of stitching vias around the GND pins of the ESP32 to connect the two ground pours on top and bottom.

I personally would recommend a 4-layer stackup so we can have a dedicated 3.3V ground pour and increase overall board capacitance, the ESP's wifi TX events can cause a good inrush current and from an EMI perspective we are creating a large current loops with 3.3V routed the way it is

```

### Workflow Stuff

The DXF files are generated from the full .step file of the BLE adapter. Then we import the outer ring and screw holes as the edge cuts of the board. A DXF containing all lines in the sketch is imported to the "user drawings" layer so we can see where our via holes need to go for the pins.

