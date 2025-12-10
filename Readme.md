# Game Boy Color LCD Frame Persistence / Duplication Quirk
GBDK Example for utilizing the GBC hardware quirk which allows previously 
rendered video frames to be persisted / duplicated on the screen while the
underlying VRAM contents are modified. The duplication is achieved by
briefly turning the LCD off and then back on again during V-Blank.

# Origins
The technique was used in a few official Game Boy Color releases (Men In Black, 
FIFA 2000, Robot Wars Metal Mayhem, etc).

This example was prompted by other people discussing the topic in the gbdev discord.

# Why is it useful?
At the expense of a lower frame rate it allows for substantial changes to the
contents of VRAM without concern for the changes showing up on screen as tearing
or partially altered images. When the persistence is stopped the Game Boy Color
will resume rendering entire frames again as normal.

# Controls
- `Left` / `Right`: Decrease or increase the number of frames to persist (min 0, power-up default 1)
- `Up` / `Down`: Scroll image up down. When persisting frames longer the scrolling become look choppier
- `A` / `B`: Cycle through test images

# Build
See the bin directory

# Implementation
See: `void display_briefly_off_toggle_vbl(void)` in `gbc_hicolor.c` and the `add_VBL()`/`remove_VBL()` calls for it.

# Example code
This project is built on the GBDK "Hi-Color" example which is a useful test-bed
since it continually modifies the GBC color palette registers as the screen is
being drawn for each frame.

This means that screen LCD drivers or hardware PPU implementations
(FPGA-based clones) which don't exactly mimic the OEM behavior will exhibit
various kinds of image degradation on persisted frames, while an OEM console 
with an original screen (and most aftermarket LCDs) will continue to show
the original image without issue aside from some shimmering.

# Hardware Behavior
See the photos in the `console_photos` folder for examples from OEM and Clone hardware.
- OEM hardware with OEM screens: the test pattern image looks as expected.
- OEM hardware with aftermarket screens: most look as expected with occasional small quirks.
- SOC clones with their OEM Screens (GBBoy Colour variants): the test pattern image looks as expected.
- SOC clones with an aftermarket LCDs (GBBoy Colour variant): the image has minor glitches, and then offsets as persistence duration increases
- FPGA Clones
  - Analogue Pocket (cart slot): The image is entirely corrupted
  - Modretro Chromatic: For the first frame the image looks, subsequent frames have white flashes
  - FPGBC (core v1.14): The image is entirely corrupted

# Emulators
The following emulators are known to emulate the frame persistence:
- Emulicious
- BGB (as of version `1.6.6` it also emulates the visual shimmer seen during frame persistence)
- SameBoy

# Example image
Example image Pixel art originally by RodrixAP under Creative Commons Attribution 2.0 Generic (CC BY 2.0)

https://www.flickr.com/photos/rodrixap/10591266994/in/album-72157637154901153/

