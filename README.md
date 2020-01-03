# Thinkpad L430/L530 Embedded Controller Firmware

This repo contains documentation and tools to deal with EC firmware of the Thinkpad L530 and L430.
Furthermore it provides samples of the L430/L530 EC firmware versions contained in BIOS updates released by Lenovo.

# Embedded Controller

The L430 and L530 contain a Nuvoton NPCE795GA0DX as the embedded controller. I couldn't find any datasheet.

According to [1] this SoC contains a CR16C+ core, runs at 25MHz, has no internal flash and 100KByte SRAM. It supports up to 4MB external SPI flash.

The Winbond WPCE775x seems to be a cousin of it, see [2]. This document describes the boot ROM in chapter 3.3.1


## Toolchain

A gcc/binutils/... toolchain for CR16 can be built using the instruction from [3].

# Lenovo Firmware

## Versions

According to [5] Lenovo released these versions of EC firmware for the L430/L530. The BIOS / BIOS ID is the first BIOS version seen with this EC firmware.

 ECP  | ECP ID   | Issue Date | BIOS | BIOS ID  | package ID
------|----------|------------|------|----------|------------
 1.04 | G3HT30WW | 2012/05/09 | 1.06 | G3ET32WW | G3UJ01UC   
 1.06 | G3HT32WW | 2012/06/28 | 1.08 | G3ET34WW | G3UJ02UC   
 1.08 | G3HT34WW | 2012/07/06 | 1.10 | G3ET36WW | G3UJ03UC   
 1.10 | G3HT36WW | 2012/08/10 | 1.11 | G3ET37WW | G3UJ04UC   
 1.12 | G3HT38WW | 2012/09/18 | 2.03 | G3ET64WW | G3UJ05UC   
 1.13 | G3HT39WW | 2012/10/01 | 2.04 | G3ET65WW | G3UJ06UC   
 1.14 | G3HT40WW | 2013/06/11 | 2.54 | G3ET94WW | G3UJ13UC   

## Extract EC Firmware From *.exe Under Linux

1. Execute the _g3ujXXus.exe_ with wine and use all default actions in the graphical installation routine except for the last step "Install Thinkpad BIOS Update Utility now", which will fail anyhow.
1. Copy the file _~/.wine/drive_c/DRIVERS/FLASH/g3ujXXus/G3ETxxWW/$01D4000.FL1_ and rename it, e.g. into fl1.bin
1. Open fl1.bin with UEFITool, it contains a UEFI capsule.
1. Extract the body of the raw File with guid  _F33E367F-41D2-4201-9CB7-AFA63DCCEEC9_, e.g. into F33E.bin.
1. Open F33E.bin with UEFITool, it contains an Intel image, i.e. it starts with a SPI flash descriptor region.
1. The BIOS region start with a non-empty padding of length 0x20000, this is the EC firmware.


# Links

* [1] Nuvoton Selection Guide https://www.nuvoton.com/selection-guide/
* [2] WPCE775x Software User Guide http://read.pudn.com/downloads365/ebook/1585622/WPCE775x_UG_1.0.pdf
* [3] https://github.com/clburrus/CR16/blob/master/CR16-dev-build-log.md
* [4] CR16C Programmer's Reference Manual https://dump.bitcheese.net/files/zujukix/Prog_16C.pdf
* [5] Lenovo Release Notes L430/L530 BIOS 2.76 https://download.lenovo.com/pccbbs/mobiles/g3uj33uc.txt
