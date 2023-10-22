# 3D Printer Rotary Cutting Add-On
A 3D printer add-on to enable rotary cutting (vinyl/tape cutting) for PCB creation or otherwise.
CC BY-NC

## Table of contents
1. [The Project](#project)
2. [Materials](#materials)
3. [Assembly](#assembly)
4. [Examples](#examples)

## The project <a name="project"></a>

![Rotary cutter mounted onto 3D-printer](https://github.com/kvriet/3dprinter-rotary-cutting/blob/main/Media/cutter%20on%20machine-1%20small.png?raw=true)
![Rotary cutter in action](https://github.com/kvriet/3dprinter-rotary-cutting/blob/main/Media/cutting%20copper.gif?raw=true)

This project adds rotary cutting capabilities to an existing Ender 3 Pro configuration, but can be attached to other printers too. This add-on will not interfere with the end stops on the Ender and can be left on while printing normally (but maybe retract the knife while printing). In the future, I will include in this project a detailed, step-by-step manual on how to cut copper tape for PCB fabrication using sheets of acrylic.

## How to make the rotary cutter holder <a name="materials"></a>
The rotary cutter used here is the Roland Cricut cutting plotter: https://www.aliexpress.com/item/32768955648.html

The holder for the cutter can be 3D printed in PLA without using supports. A layer thickness of 0.2 or 0.3mm will work.

In terms of hardware, you will also need:
- 3x M2x15 bolt for the top to tune how far the knife comes out of the holder, and for the front clamp
- 2x M2 nut for use with the clamp bolts
- 1x M3x8 to secure the vinyl cutter to the Ender

## Printed rotary cutter assembly <a name="assembly"></a>
![Print 1](https://github.com/kvriet/3dprinter-rotary-cutting/blob/main/Media/rotary%20cutter%20print-1.png?raw=true)

![Print 2](https://github.com/kvriet/3dprinter-rotary-cutting/blob/main/Media/rotary%20cutter%20print-2.png?raw=true)

![Print 3](https://github.com/kvriet/3dprinter-rotary-cutting/blob/main/Media/rotary%20cutter%20print-3.png?raw=true)

![Print 4](https://github.com/kvriet/3dprinter-rotary-cutting/blob/main/Media/rotary%20cutter%20print-4.png?raw=true)

![Print 5](https://github.com/kvriet/3dprinter-rotary-cutting/blob/main/Media/rotary%20cutter%20print-5.png?raw=true)

![Print 6](https://github.com/kvriet/3dprinter-rotary-cutting/blob/main/Media/rotary%20cutter%20print-6.png?raw=true)

## Creating circuits <a name="circuit_creation"></a>
### KiCad
I design my circuits in KiCad (version 6.0), creating a schematic and then a footprint in the PCB Editor environment. Important for rotary cutting is to minimise the number of sharp corners that the knife has to follow. To create smooth lines, I use the 'fillet tracks' option in KiCad 6.0 with a 3mm fillet radius, but adjust to your own taste. I adjust the width of my tracks depending on whether I'm cutting a sticker or tape already applied to acrylic. For stickers, I use 2mm wide tracks as this prevents them from tearing accidentally, or curling up too much on a sticker sheet. For copper tape already applied to the acrylic, the tracks can be 1mm or 0.5mm wide.

### From KiCad to gcode <a name="gcode_creation"></a>
Once the KiCad PCB is done, export the copper layers you need (usually just F.Cu or B.Cu) to SVG. Open the SVG in Inkscape. Delete pads and holes. Make sure you just have the tracks. Select all and transform Stroke to Path (Ctrl+Alt+C). Use Union (Ctrl++) to make single objects out of each single track. Select all, set the stroke style to 1px and give it a colour (usually black), and set the infill to none. Make sure your tracks are on a layer.

To create the gcode from this SVG, go to Extensions --> Gcodetools --> Orientation points and click 'Apply'. Next select Tools library from Gcodetools and click 'Apply'. Finally, select Path to Gcode. Under 'Preferences' set the filename (make sure it ends in .gcode) and set Safe Z height to 5mm. Under 'Options' set Z scale to 1mm and Z offset to 2mm, which will make the knife move 1mm over the bed. Under 'Path to Gcode' click 'Apply'. Your gcode will now be created.

The created gcode will start cutting wherever the nozzle is located when the file is activated on the printer. This can be useful for exact circuit placement. If you want the printer start from the origin, add the following gcode to the header:

;; ========== start of inserted gcode
G28; Home Z (X and Y also)
G1 X0 Y0 Z5.0 F15240; Move to start position
;; ========== end of inserted gcode

Fully retract the knife when starting the gcode, then slowly bring it down to start cutting and find the best knife depth for you. The knife should only cut through the copper, not through the sticker backing. Try first with some test lines. You can also do this by setting the nozzle Z height to 1mm, disabling steppers, lowering the knife by turning the screw, and then slowly moving the extruder or the bed and seeing how good the cuts are.

## Cutting circuits <a name="circuit_cutting"></a>
For creating the copper traces, two main approaches can be taken:
1. Cutting out the traces on the copper tape sticker: First put masking tape on the area of copper tape you want to cut. Then fix the copper tape on the print bed with some clamps or tape. Then the traces can be cut out on the sticker. However, features need to be big and round, otherwise the sticker will curl up and come loose from the sticker. The copper traces can then be transferred from the sticker to the sheet of acrylic using some more masking tape. Once the traces have been transferred, the masking tape needs to be removed.
2. Cutting out the traces on copper already taped to acrylic: First transfer the copper tape to the sheet of acylic. Then fix the acrylic to the print bed using some clamps or tape. Then cut out the traces. Take the acrylic from the printer and remove the copper tape that is not part of the traces. Features can be very small and angular, as the copper tape is already firmly attached to the acrylic and will not start curling.

## Some example circuits cut with this setup <a name="examples"></a>
![Example circuit 1](https://github.com/kvriet/3dprinter-rotary-cutting/blob/main/Media/example%20circuits-1.png?raw=true)

![Example circuit 2](https://github.com/kvriet/3dprinter-rotary-cutting/blob/main/Media/example%20circuits-2.png?raw=true)

![Example circuit 3](https://github.com/kvriet/3dprinter-rotary-cutting/blob/main/Media/example%20circuits-3.png?raw=true)

![Example circuit 4](https://github.com/kvriet/3dprinter-rotary-cutting/blob/main/Media/example%20circuits-4.png?raw=true)

