[[TableOfContents]]

= D-Link DWL-2100AP =
[http://www.dlink.com/products/?pid=292 AirPlus XtremeG Wireless Access Point]

== Foreword ==

Please read carefully this warning message before you start. This page is about modifying your DWL-2100AP box. Either if you wish to access the serial console or install OpenWRT permanently, both the hardware and the firmware on your box will require modifications. Your box was not built for this purpose and therefore by making any of the changes described herein '''you will void your warranty'''.

The hardware modifications, despite being minor and easily made by a trained person, do require certain skills you may not posses, like handling a soldering iron, soldering and cutting the PCB and/or wires, measuring voltages and signals and, most importantly, protecting against static charges. A small mistake, like touching the wrong wire while being electrically charged, may damage your box permanently. Care must be taken when connecting wires to assure they are being connected properly and to compatible voltages and currents. The serial communication signals on the 2100AP are TTL-compatible ranging from 0 to 5V. These  '''must not''' be connected directly to a RS232 cable, which operates at much broader voltage range (-12 to +12V). An appropriate level coupler must be built for this purpose. There are many such circuits on the net, and the simplest ones might use a common MAX232 chip.

The firmware part is less critical, but also does require some skill. If anything goes wrong with the installation of a new firmware, you will probably brick your AP. This is a bit painful but it is usually reversible with a jtag cable and software.

If you plan to run openwrt on a regular basis, you will also need to replace the original VxWorks bootloader with redboot. At the moment, this can only be accomplished using a jtag cable and software. This is a very slow process, but fortunately it does not have to be repeated frequently. If you have the right bootloader binary image and follow the procedure described below very carefully, this will hopefully have to be done just once.

== Hardware ==

This section describes the relevant features of the hardware commonly found inside a DWL-2100AP box. It is mostly based on the A2 revision,
and contains some notes for revision A3.

 * Atheros AR2313A SoC
 * Atheros AR2112A RF
 * [http://web.icsi.com.tw/domino/packinfo.nsf/WebDSProcNum/(798C2B999CBCD0EA3FDA31D07F57CC19)?OpenDocument IC42S16800-7T] 16MB RAM
 * [http://www.amd.com/us-en/assets/content_type/white_papers_and_tech_docs/23579c6.pdf AMD AM29LV320DB] 4MB Flash ([http://www.atmel.com/dyn/resources/prod_documents/doc3308.pdf Atmel AT49BV322A] 4MB Flash on rev. A3)
 * [http://www.icplus.com.tw/pp-IP101A.html IC+ IP101A] Ethernet transceiver
 * External antenna on RP-SMA connector
 * Internal antenna
It seems to be based on the Atheros AP43 or AP48 reference design (not sure what the difference between those two is).

== Interfaces ==
=== Serial ===
JP1 (12-pin, without headers) is the serial port.  It's wired very similarly to the serial port in the [:OpenWrtDocs/Hardware/Netgear/WPN824:Netgear WPN824] (also AR2313) and  [:OpenWrtDocs/Hardware/Netgear/WGT624:Netgear WGT624] (AR231'''2''')

{{{
       1    JP1
 VCC - [] () - VCC
  RX - () ()
       () ()
       () ()
  TX - () ()
GND? - () () - GND?
}}}
Some resistors (R264, R273, R275) are missing, so the serial port won't work.  I've bridged them with solder (since I don't have access to SMT equipment), and it seems like it's working. This is not needed for rev. A3.

=== JTAG ===
J1 (14-pin, without headers) is the JTAG port with standard EJTAG 2.6 layout. In JTAG Tools (works in last version jtag-0.6-cvs-20051228 only) board does not detect right, so it needs some definitions to access flash:[[BR]]

{{{
jtag> cable parallel 0x378 WIGGLER
jtag> detect
jtag> include atheros/ar2312/ar2312
jtag> poke 0x58400000 0x000e3ce1
jtag> detectflash 0x1fc00000
}}}
Flash works only in 8bit mode. It really slow, but I successfully debricked both 2100AP I have.

== Software ==
=== Standard D-Link firmware ===
The AP ships with VxWorks.  Bootup is as follows:

{{{
ar531x rev 0x00005850 firmware startup...
SDRAM TEST...PASSED
  WAP-G02A  Boot Procedure                       V1.0
---------------------------------------------------------
  Start ..Boot.B12..
Atheros AR5001AP default version 3.0.0.43A
 0
auto-booting...
Attaching to TFFS... done.
Loading /fl/APIMG1...
  Please wait, loading  image ...
  image check ok!!!
/fl/  - Volume is OK
Reading Configuration File "/fl/apcfg".
Configuration file checksum: 6c9d89 is good
Attaching interface lo0...done
vxWorksTftpPackageInit: init. finish & success!
wireless access point starting...
wlan1 Ready
Ready
}}}
The boot loader can be interrupted by pressing Esc.  Initial boot settings are:

{{{
[Boot]: p
boot device          : tffs:
unit number          : 0
processor number     : 0
file name            : /fl/APIMG1
inet on ethernet (e) : 192.168.1.20:0xffffff00
flags (f)            : 0x0
other (o)            : ae
}}}
I've tried changing the boot settings to get it to netboot via TFTP unsuccessfully, but found that entering the command line below works.  ae is the Atheros Ethernet driver, 1 is the unit number of the Ethernet interface (the DWL-2100AP has unit 1, but no unit 0), 0 is the CPU number, desktoppc is the name of the TFTP server (doesn't seem to matter much), vmlinux is the filename, h sets the IP address of the TFTP server, e sets the IP address and netmask of the Ethernet interface, and f sets the flags to use TFTP.

{{{
$ae(1,0)desktoppc:vmlinux h=192.168.0.1 e=192.168.0.2:0xffffff00 f=0x80
}}}
=== DeviceScape Linux ===
I've tried building a kernel as described on the [:OpenWrtDocs/Hardware/Netgear/WGT624:Netgear WGT624] page, but shortly after init, the system crashes and reboots:

{{{
init started:  BusyBox v1.00-pre10 (2004.06.09-17:51+0000) multi-call binary
Starting pid 10, console /dev/co<6>wlan: 0.7.3.1 BETA
nsole: '/etc/rc.d/rcS'
Load MADWiFi wlan module
Using ../../lib/modules/2.4.25<6>ath_hal: 0.9.9.2
/net/wlan.o
Load MADWiFi Atheros HAL module
Using ../../lib/mo<6>ath_pci: 0.8.5.5 BETA
<4>AHB interrupt: PROCADDR=0x18500014  PROC1=0x80000a16  DMAADDR=0x00000000  DMA1=0x00000000
ar531x rev 0x00005850 firmware startup...
SDRAM TEST...PASSED
}}}
=== DWL-2210AP firmware ===
However, the source download for the DWL-2210AP 1.0.2.8 firmware (from the [ftp://ftp.dlink.com/GPL D-Link GPL download site]) is also based on DeviceScape and seems to include support for the DWL-2100AP:

{{{
*
* Atheros board selection
*
Board type (AP30, AP31, CA8-4, CA8-5, DWL2100, DWL2210, LM-WES900a, USI-AP3x, WGT624) [CONFIG_WES900A] (NEW)
}}}
(I need to check what changes that option makes.)

It seems that the DWL-2100AP firmware's kernel directory (dwl2210-source/apps/atheros/linux) was composed of:

 * Linux 2.4.24pre2,
 * plus Atheros' LBSP installed in arch/mips/ar531x (I think it's the same LBSP release as in the DeviceScape download, but a complete copy),
 * plus arch/mips/boot/compressed and changes to arch/mips/boot/Makefile (the comments at the top of the files indicate that they originate from Instant802 -- now DeviceScape),
 * patched with the LBSP patches (still in arch/mips/ar531x/DIFFS),
 * patched with ebtables-brnf-5_vs_2.4.24.diff.gz (from http://prdownloads.sourceforge.net/ebtables/)
 * patched with squashfs1.3r3.tar.gz's linux-2.4.24 patch (from http://prdownloads.sourceforge.net/squashfs)
 * patched with a ip_conntrack NO_TRACK patch (as yet, I've no idea what it does)
That doesn't explain all the changes from 2.4.24pre2, and they're perhaps the most interesting.

==== Rebuilding with a newer kernel ====
I'm attempting to reapply the patches to 2.4.33rc1.

 * copied the arch/mips/ar531x directory
 * applying the ar531x/DIFFS is simple (hunk 1 in config-shared.in.diff needs to be done by hand)
 * of the unexplained changes, many have already been applied in 2.4.33rc1, and most of the others apply relatively easily.
=== OpenWRT ===
I've been able to build a ramdisk image from Kamikaze revision 6344 and boot it via TFTP on the DWL-2100AP.  Hacking package/madwifi/Makefile to set HAL_TARGET to ap43 provides the right HAL to get the wireless interface working.

=== Redboot ===
Redboot when loaded via JTAG cable works only with ENET1 ethernet (when using ENET0 it hangs (??), but works when loaded from original bootloader).[[BR]] In ae531xecos.c :: ae531x_init I've changed

{{{
unit = ae531x_priv->enetUnit;
}}}
to

{{{
unit=1;
}}}
to get it to work.[[BR]] [[BR]] To get redboot detect flash (in my case it is S29gl032m90) it has to be described in packages/devs/flash/amd/am29xxxxx/v2_0/include/flash_am29xxxxx_parts.inl:

{{{
    {   // S29GL032M, model R0 - uniform sector device
        long_device_id: true,
        device_id  : FLASHWORD(0x7e),
        device_id2 : FLASHWORD(0x1c),
        device_id3 : FLASHWORD(0x00),
        block_size : 0x10000 * CYGNUM_FLASH_INTERLEAVE,
        block_count: 64,
        device_size: 0x400000 * CYGNUM_FLASH_INTERLEAVE,
        base_mask  : ~(0x400000 * CYGNUM_FLASH_INTERLEAVE - 1),
        bootblock  : false,
        banked     : false
    },
    {   // S29GL032M, model R1,R2 - uniform sector device
        long_device_id: true,
        device_id  : FLASHWORD(0x7e),
        device_id2 : FLASHWORD(0x1d),
        device_id3 : FLASHWORD(0x00),
        block_size : 0x10000 * CYGNUM_FLASH_INTERLEAVE,
        block_count: 64,
        device_size: 0x400000 * CYGNUM_FLASH_INTERLEAVE,
        base_mask  : ~(0x400000 * CYGNUM_FLASH_INTERLEAVE - 1),
        bootblock  : false,
        banked     : false
    },
    {   // S29GL032M, model R4 - boot blocks on the bottom
        long_device_id: true,
        device_id  : FLASHWORD(0x7e),
        device_id2 : FLASHWORD(0x1a),
        device_id3 : FLASHWORD(0x00),
        block_size : 0x10000 * CYGNUM_FLASH_INTERLEAVE,
        block_count: 64,
        device_size: 0x400000 * CYGNUM_FLASH_INTERLEAVE,
        base_mask  : ~(0x400000 * CYGNUM_FLASH_INTERLEAVE - 1),
        bootblock  : true,
        bootblocks : { 0x000000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       _LAST_BOOTBLOCK
                     },
        banked     : false
    },
    {   // S29GL032M, model R3 - boot blocks on the top
        long_device_id: true,
        device_id  : FLASHWORD(0x7e),
        device_id2 : FLASHWORD(0x1a),
        device_id3 : FLASHWORD(0x01),
        block_size : 0x10000 * CYGNUM_FLASH_INTERLEAVE,
        block_count: 64,
        device_size: 0x400000 * CYGNUM_FLASH_INTERLEAVE,
        base_mask  : ~(0x400000 * CYGNUM_FLASH_INTERLEAVE - 1),
        bootblock  : true,
        bootblocks : { 0x3F0000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       _LAST_BOOTBLOCK
                     },
        banked     : false
    },
}}}
Note: DWL-2100AP Rev. A3 uses the [http://www.atmel.com/dyn/resources/prod_documents/doc3308.pdf Atmel AT49BV322A] flash, so instead of the above I used:

{{{
    {   // AM49BV322A
        device_id  : FLASHWORD(0xc8),
        block_size : 0x10000 * CYGNUM_FLASH_INTERLEAVE,
        block_count: 64,
        device_size: 0x400000 * CYGNUM_FLASH_INTERLEAVE,
        base_mask  : ~(0x400000 * CYGNUM_FLASH_INTERLEAVE - 1),
        bootblock  : true,
        bootblocks : { 0x000000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       0x002000 * CYGNUM_FLASH_INTERLEAVE,
                       _LAST_BOOTBLOCK
                     },
        banked     : false
    },
}}}
In plf_flash.c set flash to x8bit mode (it does not works in x16 mode):

{{{
#define CYGNUM_FLASH_INTERLEAVE (1)
#define CYGNUM_FLASH_SERIES     (1)
#define CYGNUM_FLASH_WIDTH      (8)
#define CYGNUM_FLASH_16AS8      1
#define CYGNUM_FLASH_BASE       (0xbfc00000)
}}}
This tells redboot that flash begins at 0xbfc00000. SDRAM begins at  0x80000000, so RAM version works now, and I'm trying to make ROM version works.

Furthermore, if you have the AT49BV322D flash chip, you might know that it does not support memory unlocking (unless of course you power off the unit), so I had to disable it all along by adding the following to the top of packages/devs/flash/amd/am29xxxxx/v2_0/include/flash_am29xxxxx.inl:

{{{
#define CYGHWR_FLASH_AM29XXXXX_NO_WRITE_PROTECT
}}}
