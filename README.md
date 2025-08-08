# gba-multiboot-in-arm

**Note: unless a proper header is added, these programs will not work on real hardware (nor some emulators).**
I do not include the bitmap of the Nintendo logo in the header in order to avoid copyright infringement.
If you want to run this program on an emulator, I'd recommend [mGBA](https://mgba.io/).

Lets a host GBA send a program to a client GBA without the client needing a cartridge.

## Build Instructions

1. *Optional (but usually necessary to run properly):* Replace the bytes designated for the Nintendo Logo in the header with the proper bitmap data.
2. Assemble with the [Goldroad 1.7](https://www.gbadev.org/tools.php?showinfo=192) assembler:
```sh
goldroad.exe multiboot.asm
```
