# gba-multiboot-in-arm

Lets a host GBA send a program to a client GBA without the client needing a cartridge.

**Note: unless a proper header is added, these programs will not work on real hardware (nor some emulators).**
I do not include the bitmap of the Nintendo logo in the header in order to avoid copyright infringement.
If you want to run this program on an emulator, I'd recommend [mGBA](https://mgba.io/) (**see usage instructions**).

---

## Build Instructions

1. *Optional (but usually necessary to run properly):* Replace the bytes designated for the Nintendo Logo in the header with the proper bitmap data.
2. Assemble with the [Goldroad 1.7](https://www.gbadev.org/tools.php?showinfo=192) assembler:
```sh
goldroad.exe multiboot.asm
```

---

## Usage Instructions

Here are two methods to run the program that have been tested:

### Method 1: Real Hardware (Link Cable)

1. Build the program; **build step one is not optional for this method**.
2. Plug the purple end (skinnier end) of a GBA link cable into the host GBA and the grey end (wider end) of the cable into the client GBA.
3. Run the built binary on the host GBA.
4. Turn on the client GBA and enter multiboot mode: if there is no inserted cartridge, it will automatically boot into multiboot mode; if there is a cartridge, press the 'Start' and 'Select' buttons when the GBA is turning on (when "GAME BOY" appears on the screen) to enter multiboot mode. You will know the GBA is in multiboot mode when the Nintendo logo does not appear on the screen.
5. Press the 'A' button on the host GBA. The indicator pixel in the top left corner should turn from red to dark yellow to green, at which the client GBA should boot the recieved client program.

#### Troubleshooting

Issue | Explanation and Possible Fix(es)
----- | --------------------------------
Program(s) is/are not loading up (Nintendo logo might look garbled) | This means the Nintendo logo bitmap in the header is not being seen as correct. Possible fixes: check the program header and rebuild the program with the correct Nintendo logo bitmap, re-insert the cartridge on the host GBA (if using a flash-cart), or check the link cable (properly plugged in and in good and working shape).
Indicator pixel stuck at dark yellow | This indicates that the host GBA is trying to communicate with the client GBA. Possible fixes: try waiting more time before pressing the 'A' button on the host GBA (there's no rush), re-insert the link cable in both GBAs and try again, swap around the link cable and try again (host might have client end and vice versa), restart the client GBA and go into multiboot mode and try again (if you have a cartridge inserted, try leaving it out before rebooting), or check if the link cable is bad.
Indicator pixel is stuck at red | This means the program is not detecting if the 'A' button is pressed. Possible fixes: try swapping which GBA is the host and which is the client or test other games on the host GBA to verify if the 'A' button works.
Indicator pixel turned green but nothing happens on the client GBA | This means that the program sent is not in the correct format or the program was loaded too fast. Possible fixes: try waiting more time before pressing the 'A' button on the host GBA (there's no rush), check your link cable, or make sure the client program is in the correct format when building the program (see the comment on the client code in [`multiboot.asm`](https://github.com/JKSquires/gba-multiboot-in-arm/blob/d495d2ff7c0b86b20bddc3f65690fa04ca10a0e8/multiboot.asm#L133)).

### Method 2: mGBA

Step one in this method is optional, but highly recommended. **If step one is skipped, the client program will be loaded on the client window but not will not run** (the Nintendo logo will be garbled).
1. *Optional.* build the program, and follow step one in the build instructions.
2. Run [mGBA](https://mgba.io/) with the GBA BIOS (you will have to source this yourself). This can be done with the following command:
```sh
mgba -b <path to GBA BIOS binary>
```
3. In the 'File' menu, click 'New multiplayer window' (from now on, the original window that was open will be called the host window, and the window that was just opened will be called the client window).
4. In the host window, load the program binary (a.k.a. ROM) (to load the binary, drag it into the window pane or click on 'Load ROM' in the 'File' menu and locate and open the binary).
5. In the client window, open the 'File' menu and click the 'Boot BIOS' button.
6. Once both windows have finished loading their programs and the flash across the 'Game Boy' logo has finished, press the key bound to the GBA's 'A' button (by default, it is usually the 'x' key on the keyboard).
7. The program may load on the client window. If it does not (the indicator pixel on the host window stays dark yellow for more than a few seconds max) try steps 2 through 6 again (you may have to do this multiple times because mGBA doesn't always connect the two instances properly).

#### Troubleshooting

Issue | Explanation and Possible Fix(es)
----- | --------------------------------
Indicator pixel is stuck at dark yellow (and client window does not change) | This means the connection between the two mGBA instances doesn't work. Possible fixes: keep trying to do steps 2 through 6 (it has taken more than 5 attemps multiple times for me in the past), try waiting more time before pressing the 'A' button on the host GBA (there's no rush), or if all else fails, try all of these potential fixes with a different BIOS binary and/or rebuild the program binary.
Nintendo logo looks garbled and the client program does not run | This means the Nintendo logo bitmap in the header is not being seen as correct. Possible fixes: check the program header and rebuild the program with a valid Nintendo logo bitmap (build step one).
Indicator pixel turned green but nothing happens on the client GBA | This means that the program sent is not in the correct format or the program was loaded too fast. Possible fixes: try waiting more time before pressing the 'A' button on the host GBA (there's no rush) or make sure the client program is in the correct format when building the program (see the comment on the client code in [`multiboot.asm`](https://github.com/JKSquires/gba-multiboot-in-arm/blob/d495d2ff7c0b86b20bddc3f65690fa04ca10a0e8/multiboot.asm#L133)).
Clicking 'Boot BIOS' does nothing | This means that mGBA doesn't have a BIOS file to run. Possible fixes: do step two again and make sure the path to the BIOS binary is valid (do not just run mGBA without providing a BIOS binary).
