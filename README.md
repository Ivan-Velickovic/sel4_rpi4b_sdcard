# seL4 Raspberry Pi 4B image

This is a pre-built image for the Raspberry Pi 4B
intended for use with seL4.

## Why does this exist?

When you use something like the
[official Raspberry Pi imager](https://www.raspberrypi.com/software/)
it sets up your microSD card if you were to boot straight into their Linux
OS and nothing else. This makes sense for most users, but if you're
using the board with seL4, you need to change a couple things.

While there is only a couple steps to setting up the microSD card for booting
into seL4, if you're new to this sort of stuff it can be quite easy to
mess something up.

## Use

> [!NOTE]
> A 32GB or higher microSD card is recommended, smaller cards may run into issues
> with this image.

Instead of booting straight into Raspberry Pi OS, this image boots into U-Boot
which from there can be used to boot whatever you want. In this case, seL4.
The image still contains Raspberry Pi OS, so from the U-Boot command line
you can still boot into it if you want.

First, download the image from the
[releases page](https://github.com/Ivan-Velickovic/sel4_rpi4b_sdcard/releases/latest).

Now you want to take your microSD card and plug it into your computer.

For flashing the image, you have two options:
1. Use [balenaEtcher](https://etcher.balena.io/) to flash the SD card.
2. Unzip the image and use `dd` on the command-line.

If you're new to this sort of thing, the first option is recommended.

## Running

When you look at the serial you should see the following output
from U-Boot. This shows that the board is correctly booting up.
```
U-Boot 2025.04-rc1-00109-g97c125e6bb44 (Feb 05 2025 - 10:54:17 +1100)

DRAM:  948 MiB (effective 3.9 GiB)
RPI 4 Model B (0xc03111)
Core:  215 devices, 17 uclasses, devicetree: board
MMC:   mmcnr@7e300000: 1, mmc@7e340000: 0
Loading Environment from FAT... OK
In:    serial,usbkbd
Out:   serial,vidconsole
Err:   serial,vidconsole
Net:   
Warning: ethernet@7d580000 MAC addresses don't match:
Address in DT is                dc:a6:32:0e:25:48
Address in environment is       dc:a6:32:0e:da:f5
eth0: ethernet@7d580000

PCIe BRCM: link up, 5.0 Gbps x1 (SSC)
starting USB...
Bus xhci_pci: Register 5000420 NbrPorts 5
Starting the controller
USB XHCI 1.00
scanning bus xhci_pci for devices... 2 USB Device(s) found
       scanning usb for storage devices... 0 Storage Device(s) found
Hit any key to stop autoboot:  0 
2451869 bytes read in 162 ms (14.4 MiB/s)
## Starting application at 0x10000000 ...
```

By default, U-Boot executes the `bootcmd` [environment variable](https://docs.u-boot.org/en/latest/usage/environment.html)
that contains U-Boot commands.

The pre-built image is setup to run a Microkit 'hello world' system upon booting:
```
LDR|INFO: altloader for seL4 starting
LDR|INFO: Flags:                0x0000000000000001
             seL4 configured as hypervisor
LDR|INFO: Kernel:      entry:   0x0000008000001000
LDR|INFO: Root server: physmem: 0x0000000000244000 -- 0x000000000024e000
LDR|INFO:              virtmem: 0x000000008a000000 -- 0x000000008a00a000
LDR|INFO:              entry  : 0x000000008a000000
LDR|INFO: region: 0x00000000   addr: 0x0000000000001000   size: 0x0000000000240000   offset: 0x0000000000000000   type: 0x0000000000000001
LDR|INFO: region: 0x00000001   addr: 0x0000000000244000   size: 0x00000000000097c0   offset: 0x0000000000240000   type: 0x0000000000000001
LDR|INFO: region: 0x00000002   addr: 0x0000000000241000   size: 0x00000000000009a0   offset: 0x00000000002497c0   type: 0x0000000000000001
LDR|INFO: region: 0x00000003   addr: 0x0000000000242000   size: 0x00000000000006c5   offset: 0x000000000024a160   type: 0x0000000000000001
LDR|INFO: region: 0x00000004   addr: 0x0000000000243000   size: 0x0000000000000080   offset: 0x000000000024a825   type: 0x0000000000000001
LDR|INFO: copying region 0x00000000
LDR|INFO: copying region 0x00000001
LDR|INFO: copying region 0x00000002
LDR|INFO: copying region 0x00000003
LDR|INFO: copying region 0x00000004
LDR|INFO: CurrentEL=EL2
LDR|INFO: Resetting CNTVOFF
LDR|INFO: enabling MMU
LDR|INFO: jumping to kernel
Bootstrapping kernel
available phys memory regions: 1
  [1000..3b400000]
reserved virt address space regions: 3
  [8000001000..8000241000]
  [8000241000..8000244000]
  [8000244000..800024e000]
Booting all finished, dropped to user space
MON|INFO: Microkit Bootstrap
MON|INFO: bootinfo untyped list matches expected list
MON|INFO: Number of bootstrap invocations: 0x00000009
MON|INFO: Number of system invocations:    0x00000026
MON|INFO: completed bootstrap invocations
MON|INFO: completed system invocations
hello, world
```
