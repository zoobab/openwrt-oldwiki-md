\#language en-ca

This is a table of peripheral devices (such as USB audio, video, serial,
printer, HID or network storage) which have been tested or are known to
work (or not work, as indicated) under control of a unit running
embedded Linux. This is not a test of whether the device itself can be
run as an embedded stand-alone processor - these are peripherals only.
See the main TableOfHardware for devices on which to run OpenWrt.

''See also \["OpenWrtDocs/TableOfPeripherals"\]''

'''Status Legend''':

> -   '''Supported''' - supported in White Russian (previous release)
> -   '''Partial''' - partially supported, such as multifunction devices
>     in which only some functions work.
> -   '''Untested''' - should work in theory but never tested
> -   '''Kamikaze''' - this device is only supported in Kamikaze
>     (current release)
> -   '''Kamikaze/2.6''' - this device is only supported in Linux 2.6
>     kernels under Kamikaze (current release)
> -   '''WiP''' - Work in Progress
> -   '''Forked''' - Vendor has released some source, but it has not
>     been merged into the main !OpenWrt source tree
> -   '''No''' - confirmed that this device is not supported
> -   '''Info entered''' - Information about the device is entered in
>     this list, for reference.

Devices are grouped by interface, which will typically be USB, network
or (rarely) direct internal connection of serial or flash memory
interfaces as hardware modifications. They are then sub-grouped by
device type.

As of 2007, USB-capable platforms include the Linksys WRTSL54GS, the
ASUS WL-500g series and the ASUS WL-700 routers. The Linksys NSLU2
network attached storage interface may also be used to control various
USB devices. See also \["OpenWrtDocs/Customizing/Hardware/USB"\] and
\["WithUSBv2"\] for a list of supported platforms.

== USB devices == Due to the sheer and growing number of USB devices in
circulation, this list is and will remain a work in progress. If you've
tried any USB device with any version of OpenWRT, please report your
findings by adding the device (and its status, including any drivers
required) to this list:

=== Printers === There are two printing subsystems in common use on this
platform. The P910nd server is a simple pass-through with no
capabilities for spooling or processing data before it is sent to the
printer; its capabilities are limited but it will work with most USB
printers. The CUPS common unified printing system is a spooler, it does
process data into a form specific for each individual printer and it
does allow printers to be shared through Samba, the Windows-compatible
network device subsystem.

To work with CUPS, a device must have a driver that will run directly on
the embedded Linux platform. As such, devices for which no Linux drivers
exist or for which Linux desktop-PC object code with no sources are
provided cannot be listed as being fully compatible in this section.
They may work adequately with P910nd's passthrough (as the conversion to
printer-specific data formats remains on the desktop) but do not work at
all under CUPS.

'''Model''' '''Status''' |

=== Scanners === SANE (Scanner Access Now Easy) is designed in such a
way as to allow scanners to be shared over a TCP/IP network. The
processor which controls the scanner runs a server, the processor on a
desktop PC runs a SANE client in order to use the shared scanning
device. There is an application (!SaneTwain) which allows a Windows PC
to access these shared servers.

To be compatible with a networked device-sharing configuration, the
scanner must have SANE drivers which run under embedded Linux. Unlike
the printers, there is no p910nd-style passthrough driver. Scanners
designed to connect to parallel or SCSI ports will not work (the
USB-parallel adapters control actual parallel printers only, but cannot
control any other devices such as removable storage, scanners or EPROM
device programmers that connect to desktop PC parallel ports in place of
printers). Some of the scanners which support USB directly may work, but
compatibility varies between models.

=== Multifunction printers === These devices consist of a scanner, a
printer and (most often) a FAX modem as one integrated unit. As such,
they physically resemble photocopiers or facsimile machines.

Interface is most often USB, or a combination of USB and some other
connection type (such as parallel or network, often for the printer
only).

'''Model''' '''Status''' |

=== Human interface devices === This category exists primarily for
interface to keyboards and mice. USB joysticks and game adapters
typically also fall into this category, as well as calculator-style
numeric keypads designed for use with USB laptops. There is a standard
USB device class for these; usually there is no requirement for
manufacturer-specific drivers.

These are supported in the standard distribution, although bugs have
been reported: <https://dev.openwrt.org/ticket/2184>

Support for a few oddball devices (such as the Microsoft force-measuring
joysticks) is still 'experimental'.

=== Storage devices === External USB hard drives, CD/DVD readers, flash
memory card readers and flash memories are supported by a standard USB
storage device class. These require usb-core.ko, usb-storage.ko usb2
(preferably) and drivers for the appropriate file system (vfat.ko,
fat.ko, ext3.ko and others for HDD/flash devices, isofs for CD/DVD's);
the drivers are part of the standard distribution and, in general, this
class of device is well-supported.

=== Network interfaces === USB could be used to add an additional
wired-LAN port; these are not often needed, but some models do work with
usbnet.ko and a device-specific driver.

'''Model''' '''Status'''|

=== Wireless network interfaces === TBD.

Notes

:   -   Some network devices connect to desktop Linux PC's using
        ''ndiswrapper''. This is not a native-Linux driver set, it is
        firmware to adapt Windows object-code drivers to Linux
        installations on the same hardware. These will not work at all
        once one moves to a different processor type, so are unsuited
        for use with embedded systems.
    -   There is a project
        \[<http://rt2x00.serialmonkey.com/wiki/index.php/Hardware>
        here\] to add native Linux support for various devices. While a
        device being listed here is no guarantee of it being adaptable
        to a non-desktop platform, the corresponding Linux driver for
        each device is indicated. Do note that different revisions and
        chipsets marketed under the *same* model names sometimes are
        completely different internally.

=== Bluetooth interfaces === Various manufacturers appear to be using
the same few Bluetooth chipsets, the CSR (Cambridge Silicon Radio)
chipset being most common in USB-Bluetooth interfaces. There is some
support for USB-Bluetooth in the stock distributions but individual
devices do need to be tested further to determine compatibility.

=== Personal digital assistants === Palm OS-based devices apparently
operate under a Linux-like OS and may be compatible. The same is not
true of the Pocket PC.

Pocket PC devices such as HP/Compaq's iPAQ line are Windows-based and
poorly/not fully supported. It may be possible to get a TCP/IP
connection for standard Internet applications such as telnet or web
(there is a PuTTY available as an SSH client to run on these and some
third-party support at synce.org) but synchronisation to the Windows
desktop is by proprietary means.

=== Digital cameras === These fall into two categories: Many appear
simply as standard USB network storage devices and may be accessed in
the same manner as any USB flash memory reader; these in general are
compatible. A few use USB as a means to control the camera itself, these
may work if an application such as gphoto2 is installed to access them.

=== Audio === There is a standard USB device class for USB audio and
most devices will work. Note that some 2.4 distributions have issues
with inability to use USB 1.1 audio devices if behind a USB 2.0 hub;
this is a problem for users of multifunction USB docking stations and
for users of devices with only one USB port connector factory-installed.

See UsbAudioHowto for details on audio.

=== Telephony === Various USB devices are available for use with desktop
VoIP softphone applications such as Skype. Some of these are merely
standard USB audio (or USB audio + HID) in a telephone-like package,
others are proprietary and utterly incompatible. Oddly, the low-end
devices are often the more likely units to comply with standards (for
instance, the Skype SK04 is a fully-standard USB audio device, while the
Linksys CIT-200 is utterly incompatible with anything but the WinNT/XP
versions of skype.exe).

=== Modems === Some support for ADSL USB devices is provided by atm.ko -
individual devices need to be tested to determine compatibility.

=== Video capture === There is a standard device class for USB video,
used primarily for webcams and video capture devices. This is supported
by the video4linux drivers.

Some webcams may require device-specific drivers; the level of support
for these is model-dependent. Drivers for a few of these are provided.

=== DVB and broadcast tuners === These often have capabilities not
included in the video4linux devices, such as the ability to change
channels, operate remote switches or interface to infrared
remote-control units. Support for USB TV, radio and satellite tuners is
currently incomplete at best, even for those models where a driver is
available.

Potentially, such a device could automatically receive a signal and
record it to a network or USB hard drive. Support is limited, though.

For DVB (digital satellite, and European digital TV broadcasting) various elements are required to operate a USB tuner:

:   -   Drivers for the device itself (i2c-core, usb-core, usb-dvb-core,
        plus front-end and device drivers for the specific
        model/device): these are not provided as precompiled binaries,
        but for some devices it is possible to build drivers using the
        OpenWrt SVN sources.
    -   Firmware for the device, which is downloaded via USB. Some
        models have available .fw files
        \[<http://www.linuxtv.org/downloads/firmware/> here\]. These
        files must be installed (for most recent kernels) in
        ''/lib/firmware''; paths for older kernels differ.
    -   DVB-apps or dvbutils; a package of command-line utilities for
        tasks such as channel selection or device configuration. These
        are not included with OpenWrt (neither as source nor object),
        although a libdvbpsi library is provided.
    -   DVB channel lists (channels.conf), not included but for Europe
        (only) are available
        \[<http://www.linowsat.de/settings/vdr.html> here\] and
        elsewhere online.
    -   Somewhere to send the received (usually MPEG2-encoded) signal
        data. These streams tend to be large (a gigabyte or more per
        hour even in their original compressed form) if carrying video,
        so they need to be recorded to a hard disk, streamed to a
        desktop PC or sent to a device such as the MediaMVP or Dreambox
        that support hardware MPEG. It is not practical to attempt to
        decompress or transcode these streams on the embedded platform
        due to their size.

There are various applications intended to work with these devices, but
all are intended for desktop use or use on platforms with specialised
MPEG hardware. On the OpenWRT platforms, low-level device drivers are
provided for some models (note that the Twinhan StarBox 2 needs the
dvb-usb-vp702x-02.fw file from
\[<http://www.slackforum.de/forum/index.php?t=msg&th=2706> here\]) but
not all models are supported. The higher-level code (such as
applications to tune, scan, record and store the received data) is still
very much absent.

The linuxtv.org site provides a fair amount of background information as
to what's involved in getting these devices to operate under Linux.
Except for users able and ready to not only compile the OpenWRT kernel
drivers but also port the dvbutils to control these cards, most may find
these devices unusable.

=== Video display === The vast majority of SVGA-USB adapters are not
Linux-compatible. Some support for specific SiS chipsets (sisusb.ko) has
been reported on NSLU2-linux.org and on other Debian-like platforms, but
these are the only devices in this class to support Linux at all. (More
info \[<http://www.nslu2-linux.org/wiki/HowTo/AddVGAAdapter> here\] and
\[<http://wiki.getthekettleon.co.uk/doku.php?id=slug:digiframeslug>
here\])

Otherwise, most of these are proprietary interfaces which only work with
WinXP or maybe NT/2000, rendering them useless under any other operating
system or on any other platform.

=== Other multifunction devices === USB "universal docking stations"
normally consist of a powered USB 2.0 hub and some bundled combination
of USB peripheral interfaces, such as HID, audio, serial/parallel and
network. While the USB 2.0 hub itself will be standard and needs no
drivers to operate, the compatibility of each of the individual USB
peripherals in the bundle must be determined individually.

'''Model''''''Type/Version''' ACP45| | | Prolific 2313 | Prolific 2315 |
ASIX 88772 | C-Media | ACP50|

== NAS servers ==

These appear on the network as SMB servers; often other protocols such
as FTP are optionally supported.

'''Model''' '''Status'''|

== NAS clients == '''Model''' '''Status''' | MediaMVP'''Kamikaze''',
boots as diskless workstation from network. Requires installation of an
application (MVPrelay) to provide the location of a boot file to be
retrieved via TFTP. This app is included in Kamikaze but due to its
recent vintage is not available in the stable Whiterussian distribution
unless you build it yourself.||

== Serial (internal) == Some Linux-based routers provide the ability to
add one (or sometimes two) serial ports by connecting level-translation
hardware inside the device. These serial ports provide bidirectional
data but do not provide control signals; as such, hardware handshake
will not work. Otherwise, most serial devices should be compatible. See
the hardware modification how-to for details.

== SD/MMC (internal) == It is typically possible to connect these flash
memory cards directly to GPIO lines inside the unit, however this is
normally much slower in operation than USB flash readers. Not for the
faint of heart; see the hardware modification how-to for details.
----CategoryCategory