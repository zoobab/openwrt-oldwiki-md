'''Linksys WRTSL54GS'''

[[TableOfContents]]
= Hardware versions =

These models have a 266 MHz CPU.  There are 2 variants:

||serial||flash||RAM  || firmware ||
||CJK0  ||8 MB ||32 MB|| WhiteRussian RC5 and later works.||
||CJK1  ||8 MB ||32 MB|| WhiteRussian RC5 and later works.||

The antenna is *NOT* removable. Well not without some soldering work.

= Interface diagram (per mbm) =

{{{
                     .-OpenWrt--------------------.
                     | .-----.                    |
                     | | br0 |--------------.     |
                     | '-----'              |     |
                     |    |                 |     |
                     | .------. .------. .------. |
                     '-| eth0 |-| eth1 |-| eth2 |-'
                       '------' '------' '------'
                          |        |        |
                          |        '---.    |
    .-switch--------------|----------. |    |
    | .-vlan0-------------|--.       | |    |
    | |                .--|--+-vlan1 | |    |
    | |[0] [1] [2] [3] | [5] | [4] | | |    |
    | '-|---|---|---|--+-----'     | | |    |
    |   |   |   |   |  '-----------' | |    |
    '---|---|---|---|----------------' |    |
        |   |   |   |      .-----------'    |
        |   |   |   |      |       .--------'
        |   |   |   |      |       |
    .---|---|---|---|------|-------|----.
    |  [1] [2] [3] [4]   [wan]   [wifi] |
    '-case------------------------------'
}}}
This design is different from many units which use a single interface to handle both LAN & WAN traffic, so performance should be better.

Note that internal switch port 4 is not externally available.

= Hardware hacking =

== Serial ports ==
As this is a USB device, the easiest way to add a serial port is to connect a USB-to-serial adapter cable, either directly to the onboard USB port or through a standard USB hub. Kernel drivers are provided for a few of the most common USB-to-serial adapters.

However, if you have installed OpenWRT, you will have noticed that two serial ports (/dev/tts/0 and /dev/tts/1) are already being detected by the kernel. These are internal to the unit, but lack a level-converter (a chip such as the MAX3232 will do the trick) and external connectors to bring them outside the box. The two internal serial ports do not provide RS232 handshaking signals; they do provide serial data in both directions.

2 serial ports in a 2x5 (10-pin) block near front of board, console on ttyS0 at 115,200 baud. No hardware flow control available.  Pins are arranged in exact configuration for addition of an IDC-10 ribbon-cable connector. Unfortunately Linksys did not put one there so you will have to add your own.  The signal from the WRT board is 3.3 Volt TTL however, so you cannot simply wire to a standard RS-232 connector as they operate at 12 Volts. You will need a circuit to convert the 3.3 Volt signal to a level that is usable by a host. 

||'''connector'''||'''1'''||'''2'''||'''3'''||'''4'''||'''5'''||'''defaults'''||'''usage'''||
||JP4(ttyS0)||3.3v||TX||RX||NC||GND||115,200 baud, 8-n-1, none||console root shell||
||JP3(ttyS1)||3.3v||TX||RX||NC||GND||9,600   baud, 8-n-1, none||     unused       ||

NC=not connected, this pin is not used.

To check current serial port setting:
{{{
root@OpenWRT:~# cat /proc/tty/driver/serial
serinfo:1.0 driver:5.05c revision:2001-07-08
0: uart:16550A port:B8000300 irq:3 baud:114583 tx:5108 rx:129 RTS|DTR
1: uart:16550A port:B8000400 irq:3 baud:9593 tx:0 rx:0 CTS|DSR|CD
}}}

If you are going to work much with the serial ports, recommend to use the buildroot kit to build a firmware with BusyBox including the optional stty, getty, setserial, and maybe login programs. If you intend to use external hardware modems, mgetty may be used to handle incoming data and fax calls.

=== MAX233 kit console ===
inline:wrtsl54gs_serial_IDC10.jpg

Above are 2 common IDC-10 sockets. On the left is a straight IDC-10 socket which would be useful for internal hookups, or running a ribbon to a convenient mounting point for an external connector.  The type on the right is a "right-angle" socket and is the one installed on the SL above it.  These sockets cost less than one dollar/euro and fit relatively easily onto the PCB.

inline:wrtsl54gs_serial_complete.jpg

Here's one complete serial-console setup, using a MAX233 kit with ribbon-cable connectors. This makes it easy to move among multiple routers assuming they are fitted with an IDC-10 socket.  This is the SL unit that was the testbed for the first image of OpenWRT for this model, thanks to assistance of developer Kaloz.

If you intend to use this setup with only one of these devices, a cleaner installation approach may be to conceal the level translation circuitry inside the router's case and bring out just the serial data and ground lines on a three-pin jack for each port.

=== TTL-232R-3V3-AJ USB console ===

Another good choice is using a USB device that natively supports the 3.3V TTL signal levels. One such product:

http://www.ftdichip.com/Products/EvaluationKits/TTL-232R-3V3-AJ.htm

There is a tiny PCB in the host-end of the USB cable that is doing the signal conversion. Since the circuit is powered from the USB port, only 3 wires need to be soldered instead of the 4 needed in the MAX233 design.  You need to hook up TX, RX, and GND pins and a convenient choice for external interface port is a stereo jack.  It's a clean and elegant setup. Thanks to netprince & JimWright, here's how it can look installed:

inline:wrt_jack_cable.jpg

inline:Serial_hack.jpg


stereo-jack connector
||tip(1)||ring(2)||sleeve(3)||
||TX||RX||GND||

You have to cross TX and RX wires from the plug to the WRT board.

wiring diagram examples:
||plug  ||to||WRTSL54GS ||WRT54GL||
||1(TX) ||->||JP4-3(RX) ||6(RX)  ||
||2(RX) ||->||JP4-2(TX) ||4(TX)  ||
||3(GND)||->||JP4-5(GND)||10(GND)||

Note:  When selecting the audio jack, make sure that the threaded end is long enough to poke through your case and still be able to attach the nut that secures it. Many common stereo plugs are for use with a thin metal faceplate and do not have sufficient depth of thread. The one pictured above is from [http://www.altex.com/product_info.php?cPath=3_106_330_334&products_id=4009 Altex Electronics] part number 502K, vincentfox reports that an identical part is available from [http://shop.outpost.com/product/3343172 Fry's/Outpost.com], Mfg Philmore, part number 504K.

=== STR232 ===

Another alternative is the [http://www.compsys1.com/workbench/On_top_of_the_Bench/Max233_Adapter/max233_adapter.html STR232] adapter, which includes the MAX232 chip on a small board with the stereo jack. You will have to make up a simple pigtail from a stereo plug to a DB-9. The board has jumpers to manage the tip/ring crossover.

inline:st232_assm2w.jpg

== JTAG ==

inline:wrtsl54gs_jtag.jpg

No JTAG header is available.  However, all basic pins are present on test points.

SRST and TRST haven't been identified, but ignoring them doesn't prevent JTAG from operating.

Be warned that soldering or probing on test points is fairly tricky.  If you are using a cheap unbuffered cable, you only need to solder 5 wires since you won't be using Vcc line.

Both Xilinx and Wiggler cables should work - see [http://wiki.openwrt.org/OpenWrtDocs/Customizing/Hardware/JTAG_Cable this] wiki entry.

HairyDairyMaid's debricker is working, but currently requires /skipdetect and instrlen:8 options since the 4704 isn't in the list of supported processors.  The 28F640J3 flash in the SL is in the known part list of the debricker.

== GPIO ==
||'''GPIO #'''||'''direction'''||'''location'''||'''name'''||'''function'''||
||0||output||LEDC15||DMZ||LED - DMZ||
||1||output||LEDC9 ||POWER LED||LED - Power||
||2||output||RA59(back)||?||?||
||3||output||RA60(back)||?||?||
||4||input||PSW2||button||Button - SES||
||5||output||LEDC13||SES white||LED - SES white||
||6||input||PSW1||RESET||Button - reset||
||7||output||LEDC14||SES amber||LED - SES amber||

== Dual USB port ==

The board has functional position for a stacked dual-USB port, although it is only fitted for a single port. As an alternative to adding an external USB hub, remove the existing unit, and substitute with a dual.  Stacked dual-USB port can be scavenged from an old/dead motherboard.

== Further USB1.1 ports ==
There is a theoretical method (no attempts actually made) to add 3 more USB ports, but they are limited to being USB1.1, as the relevant pads for USB2.0 are not routed out from under the USB hub chip. In the image below, the blue dots are the existing USB2.0 traces to the socket, the yellow arrows are the untracked pads necessary for USB2.0, and the red dots are the necessary connections for the extra 3 potential USB1.1 ports. Obviously, the 0V and +5V connections will need to be made somewhere for each port as well, and some care will need to be taken to ensure the source can supply enough current.

inline:wrtsl54gs_usb.jpg

== LED10 ==
The LED10 location at front of board contains no LED. Perhaps it is usable for something. But it does not appear to be connected directly to a GPIO pin as a voltmeter shows nothing when cycling all GPIO lines.

== RAM upgrade ==
RAM upgrade to 64 megs is possible by adding the 20 missing resistors and another 32-meg chip.  Noted in the Links section as having been done using a Micron MT46V16M16 which is pin-compatible replacement for the Hynix.

It is likely that by replacement of original RAM chip with a 64-meg one like Micron MT46V32M16, plus a second one at the unoccupied position, the memory could be upgraded to 128 megs.

= Board info and CPU model =
||'''Model'''||'''boardrev'''||'''boardtype'''||'''boardflags'''||'''boardnum'''||'''wl0_corerev'''||'''cpu  model'''||
||WRTSL54GS||0x10||0x042f||0x0018||42||9||BCM4704 rev8||

= More information =

Autopsy photos http://www.linksysinfo.org/forums/showthread.php?t=47389

64 meg RAM upgrade: http://www.linksysinfo.org/forums/showthread.php?t=46673

Original exploration thread http://www.linksysinfo.org/forums/showthread.php?t=43413&highlight=wrtsl54gs

Spillover into OpenWRT  http://forum.openwrt.org/viewtopic.php?id=3529

You can get the MAX233 parts kit here:
http://www.compsys1.com/workbench/On_top_of_the_Bench/Max233_Adapter/max233_adapter.html
Recent information was, an extra $6 added to kit price on request for an assembled version.

Another USB TTL convertor device:
http://www.compsys1.com/html/usb_rs232.html

= Firmware download =

Recommend to use WhiteRussian RC5 or later.
