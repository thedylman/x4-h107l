# Hubsan X4 H107L Tools

The X4 uses a Nuvoton MINI54ZAN or MINI54ZDE cpu.

## Debugging Interface
The MINI54 supports debugging via a SWD interface. To debug and flash you will need to connect a minimum of the following pins:

1. Ground (Use battery negative terminal)
2. SWDIO (A test point marked as "D")
3. SWDCLK (A test point marked as "C")

For the Dangerous Prototypes Bus Blaster it is also good to connect the test point marked as "+" to the VTG pin. This allows the programmer to match the target voltage.

## Flashing with OpenOCD
SWD and the MINI51 series is not supported in version 8.0 of OpenOCD. You will need to build the head of OpenOCD from git.

The flash on the X4 is locked, this stops you from reading the current firmware and needs to be unlocked to change the firmware. Unlocking erases the current firmware so once you do this you cannot go back to the original
firmware.

To unlock the cpu run:

```bash
openocd -f <interface.cfg> -c "transport select swd" -f ./openocd/mini51.cfg -c "init; halt; mini51 chip_erase; shutdown"
```

To program the flash run:
```bash
openocd -f <interface.cfg> -c "transport select swd" -f ./openocd/mini51.cfg "program <path to elf> verify reset; shutdown"
```
