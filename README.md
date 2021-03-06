# Getting started example for Mbed OS

This guide reviews the steps required to get Blinky with the addition of dynamic OS statistics working on an Mbed OS platform. (Note: To see a rendered example you can import into the Arm Online Compiler, please see our [quick start](https://os.mbed.com/docs/mbed-os/latest/quick-start/online-with-the-online-compiler.html#importing-the-code).)

Please install [Mbed CLI](https://github.com/ARMmbed/mbed-cli#installing-mbed-cli).

## Import the example application

From the command-line, import the example:

```
mbed import mbed-os-example-blinky
cd mbed-os-example-blinky
```

### Now compile

Invoke `mbed compile`, and specify the name of your platform and your favorite toolchain (`GCC_ARM`, `ARM`, `IAR`). For example, for the ARM Compiler 5:

```
mbed compile -m CUSTOM_F427VI -t GCC_ARM
```

Your PC may take a few minutes to compile your code. At the end, you see the following result:

```
[snip]
+----------------------------+-------+-------+------+
| Module             |     .text |    .data |      .bss |
|--------------------|-----------|----------|-----------|
| [fill]             |   177(+0) |   19(+0) |    54(+0) |
| [lib]\c.a          | 27324(+0) | 2472(+0) |    89(+0) |
| [lib]\gcc.a        |  3116(+0) |    0(+0) |     0(+0) |
| [lib]\misc         |   208(+0) |   12(+0) |    28(+0) |
| main.o             |  1114(+0) |    4(+0) |    64(+0) |
| mbed-os\cmsis      |  1033(+0) |    0(+0) |    84(+0) |
| mbed-os\components |   171(+0) |    0(+0) |     0(+0) |
| mbed-os\drivers    |   855(+0) |    4(+0) |   100(+0) |
| mbed-os\events     |  1662(+0) |    0(+0) |  2596(+0) |
| mbed-os\features   |  2024(+0) |    0(+0) | 12688(+0) |
| mbed-os\hal        |  3357(+0) |    8(+0) |   247(+0) |
| mbed-os\platform   |  4366(+0) |  260(+0) |   232(+0) |
| mbed-os\rtos       | 11692(+0) |  168(+0) |  5969(+0) |
| mbed-os\targets    | 12832(+0) |    5(+0) |   857(+0) |
| Subtotals          | 69931(+0) | 2952(+0) | 23008(+0) |
Total Static RAM memory (data + bss): 25960(+0) bytes
Total Flash memory (text + data): 72883(+0) bytes


Image: .\BUILD\CUSTOM_F427VI\GCC_ARM\mbed-os-example-blinky.bin
```

### Program your board

1. Connect your Mbed device to the computer over USB.
1. Copy the binary file to the Mbed device.
1. Press the reset button to start the program.

The LED on your platform turns on and off. The main thread will additionally take a snapshot of the device's runtime statistics and display it over serial to your PC. The snapshot includes:

* System Information:
    * Mbed OS Version: Will currently default to 999999
    * Compiler ID
        * ARM = 1
        * GCC_ARM = 2
        * IAR = 3
    * [CPUID Register Information](#cpuid-register-information)
    * [Compiler Version](#compiler-version)
* CPU Statistics
    * Percentage of runtime that the device has spent awake versus in sleep
* Heap Statistics
    * Current heap size
    * Max heap size which refers to the largest the heap has grown to
* Thread Statistics
    * Provides information on all running threads in the OS including
        * Thread ID
        * Thread Name
        * Thread State
        * Thread Priority
        * Thread Stack Size
        * Thread Stack Space

#### Compiler Version

| Compiler | Version Layout |
| -------- | -------------- |
| ARM      | PVVbbbb (P = Major; VV = Minor; bbbb = build number) |
| GCC      | VVRRPP  (VV = Version; RR = Revision; PP = Patch)    |
| IAR      | VRRRPPP (V = Version; RRR = Revision; PPP = Patch)   |

#### CPUID Register Information

| Bit Field | Field Description | Values |
| --------- | ----------------- | ------ |
|[31:24]    | Implementer       | 0x41 = ARM |
|[23:20]    | Variant           | Major revision 0x0  =  Revision 0 |
|[19:16]    | Architecture      | 0xC  = Baseline Architecture |
|           |                   | 0xF  = Constant (Mainline Architecture) |
|[15:4]     | Part Number       | 0xC20 =  Cortex-M0 |
|           |                   | 0xC60 = Cortex-M0+ |
|           |                   | 0xC23 = Cortex-M3  |
|           |                   | 0xC24 = Cortex-M4  |
|           |                   | 0xC27 = Cortex-M7  |
|           |                   | 0xD20 = Cortex-M23 |
|           |                   | 0xD21 = Cortex-M33 |
|[3:0]      | Revision          | Minor revision: 0x1 = Patch 1 |



You can view individual examples and additional API information of the statistics collection tools at the bottom of the page in the [related links section](#related-links).


### Output

To view the serial output you can use any terminal client of your choosing such as [PuTTY](http://www.putty.org/) or [CoolTerm](http://freeware.the-meiers.org/). Unless otherwise specified, printf defaults to a baud rate of 9600 on Mbed OS.

You can find more information on the Mbed OS configuration tools and serial communication in Mbed OS in the related [related links section](#related-links).

The output should contain the following block transmitted at the blinking LED frequency (actual values may vary depending on your target, build profile, and toolchain):

```
=============================== SYSTEM INFO  ================================
Mbed OS Version: 51104
CPU ID: 0x410fc241
Compiler ID: 2
Compiler Version: 80200
RAM0: Start 0x20000000 Size: 0x30000
RAM1: Start 0x10000000 Size: 0x10000
ROM0: Start 0x8000000 Size: 0x200000
================= CPU STATS =================
Idle: 4% Usage: 96%
================ HEAP STATS =================
Current heap: 1216
Max heap size: 1216
================ THREAD STATS ===============
ID: 0x20005494
Name: main
State: 2
Priority: 24
Stack Size: 4096
Stack Space: 3136
ID: 0x20004e24
Name: rtx_idle
State: 1
Priority: 1
Stack Size: 512
Stack Space: 160
ID: 0x20004de0
Name: rtx_timer
State: 3
Priority: 40
Stack Size: 768
Stack Space: 664

```

## Troubleshooting

If you have problems, you can review the [documentation](https://os.mbed.com/docs/latest/tutorials/debugging.html) for suggestions on what could be wrong and how to fix it.

## Related Links

* [Mbed OS Stats API](https://os.mbed.com/docs/latest/apis/mbed-statistics.html)
* [Mbed OS Configuration](https://os.mbed.com/docs/latest/reference/configuration.html)
* [Mbed OS Serial Communication](https://os.mbed.com/docs/latest/tutorials/serial-communication.html)

### License and contributions

The software is provided under Apache-2.0 license. Contributions to this project are accepted under the same license. Please see contributing.md for more info.

This project contains code from other projects. The original license text is included in those source files. They must comply with our license guide.
