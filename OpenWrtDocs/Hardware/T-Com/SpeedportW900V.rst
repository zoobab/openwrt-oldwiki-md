#pragma section-numbers off
[[TableOfContents]]

= T-Com Speedport W900V =

This page is Work In Progress, speak to jb24 on #openwrt and #ar7 on freenode for more info.

Others users known to have this device:  jb24, crazy_imp, Heini66 ,["macdet"] still in avm mode :(

== Hardware Info ==

Uses TI AR7 chipset, onboard wireless lan, a very nice amount of ram (32MB) and flash (8MB) making it a great device to run OpenWRT on!

Being an AR7 device it also has a built-in ADSL Modem, the Speedport W900V also features as ISDN socket and two telephone sockets for VoIP use.

CPU: TNETD7200ZDW (AR7) @211Mhz  Flash: 8 MB  Ram: 32 MB  WLan Chip: TNETW1350A Ethernet Switch Chip: Infineon ADM6996LC

It also has a single 3.3v serial port, the original T-Com firmware allows you shell access with no password to the device though the serial port.

Details about the DECT part are [:OpenWrtDocs/Hardware/T-Com/SpeedportW900V/DECT:here].

=== Photos ===

http://img177.imageshack.us/img177/5249/pict1605te5.jpg

http://img518.imageshack.us/img518/631/pict1606um5.jpg

http://img507.imageshack.us/img507/6286/pict1607di0.jpg

http://img225.imageshack.us/img225/699/pict1608be7.jpg

http://img146.imageshack.us/img146/5471/pict1609ne2.jpg

http://img183.imageshack.us/img183/2884/rearserialmarkedhl9.jpg

=== Serial Port ===
 TODO

It has a 3.3v serial port to the lower right of the CPU, near the crystal and the large capacitor.  The PCB on my router didn't have a pin header/pin strip attached to it so I bought a pin strip from Conrad Electronic for €0.24 and soldered it carefully to the back of the PCB.

The general location of the port and it's pin-out is as follows:

{{{
        Top right of PCB
_________________________
                         |
F     R A M              |
L                WIFI    |
A                CHIP    |
S        SoC             |
H        CPU  XTal       |
                 4  GND  |
                 3  TX   |
                 2  RX   |
                 1  VCC  |
                         |
              ADM6996    |
                         |
_________________________|
}}}

 Serial port settings: 38400 8N1

=== OS Info ===

{{{
 # cat /proc/version
Linux version 2.6.13.1-ohio (jpluschke@EmbeddedVM) (gcc version 3.4.3) #5 Fri Aug 25 12:37:20 CEST 2006
}}}

=== CPU Info ===
{{{
~ # cat /proc/cpuinfo
system type             : MIPS OHIO
processor               : 0
cpu model               : MIPS 4KEc V4.8
BogoMIPS                : 211.35
wait instruction        : yes
microsecond timers      : yes
tlb_entries             : 16
extra interrupt vector  : yes
hardware watchpoint     : yes
VCED exceptions         : not available
VCEI exceptions         : not available
}}}

=== FLASH Map Info ===
{{{
cat /proc/mtd
dev:    size   erasesize  name
mtd0: 00800000 00010000 "phys_mapped_flash"
mtd1: 006d3d00 00010000 "filesystem"
mtd2: 00770000 00010000 "kernel"
mtd3: 00010000 00010000 "bootloader"
mtd4: 00040000 00010000 "tffs (1)"
mtd5: 00040000 00010000 "tffs (2)"
mtd6: 00200000 00010000 "jffs2"
mtd7: 00570000 00010000 "Kernel without jffs2"
}}}

=== ATM Driver Info (TI Avalanche SAR) ===
{{{
cat /proc/avalanche/avsar_ver
ATM Driver version:[4.06.04.30]
DSL HAL version: [5.00.00.02]
DSP Datapump version: [1.35.60.01]
SAR HAL version: [01.07.2b]
PDSP Firmware version:[0.54]
}}}

=== loaded kernel modules ===
{{{
cat /proc/modules
tiap 880464 0 - Live 0xc04a4000
kdsldmod 515808 2 - Live 0xc0369000
usbahcicore 25456 0 - Live 0xc0053000
usbcore 113456 2 usbahcicore, Live 0xc0164000
isdn_fbox 928912 25 - Live 0xc01d9000
ubik2 67216 1 isdn_fbox, Live 0xc00ba000
tiatm 106560 1 ubik2, Live 0xc00de000
jffs2 108928 1 - Live 0xc005b000
nls_iso8859_1 3584 0 - Live 0xc0008000
nls_cp437 5248 0 - Live 0xc0019000
vfat 11392 0 - Live 0xc0015000
fat 49168 1 vfat, Live 0xc0027000
Piglet 7632 0 - Live 0xc000a000
}}}


=== devices ===
{{{
cat /proc/devices
Character devices:
  1 mem
  2 pty
  3 ttyp
  4 ttyS
  5 /dev/tty
  5 /dev/console
  5 /dev/ptmx
 10 misc
 13 input
 68 capi20
 90 mtd
128 ptm
136 pts
180 usb
191 capi
230 tiatm
240 tffs
241 avm_event
242 watchdog
243 kdsld
244 kdsldptrace
245 ubik2
246 debug
251 avm_led
252 avm_power

Block devices:
 31 mtdblock
}}}

=== free ===
{{{
            total         used         free       shared      buffers
  Mem:        30476        27432         3044            0         3360
 Swap:            0            0            0
Total:        30476        27432         3044
}}}


=== MEMINFO ===
{{{
MemTotal:        30476 kB
MemFree:          3032 kB
Buffers:          3360 kB
Cached:          11224 kB
SwapCached:          0 kB
Active:          10704 kB
Inactive:         7844 kB
HighTotal:           0 kB
HighFree:            0 kB
LowTotal:        30476 kB
LowFree:          3032 kB
SwapTotal:           0 kB
SwapFree:            0 kB
Dirty:               0 kB
Writeback:           0 kB
Mapped:           7312 kB
Slab:             4992 kB
CommitLimit:     15236 kB
Committed_AS:     5676 kB
PageTables:        228 kB
VmallocTotal:  1048560 kB
VmallocUsed:      3600 kB
VmallocChunk:  1042944 kB
}}}

=== IOMEM ===
{{{
cat /proc/iomem
14000000-14208fff : reserved
  14000000-141714a3 : Kernel code
  141714a4-141c50bf : Kernel data
14209000-15ffffff : System RAM
a8610000-a86107ff : cpmac0
}}}


=== running proccesses ===
{{{
~ # ps -ax
 PID  Uid     VmSize Stat Command
    1 root        336 S   init       
    2 root            SWN [ksoftirqd/0]
    3 root            SW< [events/0]
    4 root            SW< [khelper]
    5 root            SW< [kthread]
    6 root            SW< [kblockd/0]
   24 root            SW< [pdflush]
   25 root            SW< [pdflush]
   27 root            SW< [aio/0]
   26 root            SW  [kswapd0]
   69 root            SW  [mtdblockd]
  128 root            SW  [tffsd_mtd_0]
  368 root            SWN [jffs2_gcd_mtd6]
  403 root            SW  [capitransp]
  416 root            SW< [khubd]
  552 root       2772 S N ctlmgr 
  617 root        468 S   wpa_authenticator 
  630 root       1656 S N websrv 
  638 root       1736 S   multid 
  651 root       1920 S   dsld -i -n 
  665 root        880 S   telefon a127.0.0.1 
  669 root       1656 S N websrv 
  671 root       2136 S < voipd 
  673 root       1656 S N websrv 
  677 root       1656 S N websrv 
  680 root        160 S   /bin/run_clock -c /dev/tffs -d 
  685 root        468 S   -sh 
  776 root        312 R   ps -ax 
}}}

=== Kernel messages ===
{{{
~ # cat /proc/kmsg

y tagged, 4-way, linesize 16 bytes.
<4>Primary data cache 8kB, 4-way, linesize 16 bytes.
<4>Synthesized TLB refill handler (20 instructions). Base=0x941e0734
<4>TLB synthesizer field overflow (simm)
<4>Synthesized TLB load handler fastpath (34 instructions) Base=0x941e3760.
<4>TLB synthesizer field overflow (simm)
<4>Synthesized TLB store handler fastpath (34 instructions) Base=0x941e3960.
<4>TLB synthesizer field overflow (simm)
<4>Synthesized TLB modify handler fastpath (33 instructions) Base=0x941e3b60.
<4>PID hash table entries: 256 (order: 8, 4096 bytes)
<4>[ohio_set_clock_notify] avm_clock_id_cpu notify disable 0x940013f8 0x94199e18
<4>[ohio_set_clock_notify] avm_clock_id_cpu notify enable 0x940013f8 0x94199e18
<4>CPU frequency 211.97 MHz
<4>Using 105.984 MHz high precision timer.
<4>[setup_irq]: irq 127 irqaction->handler 0x94041a10 (no_action+0x0/0x8 )
<4>[register_console] enable commandline console 0
<4>Dentry cache hash table entries: 8192 (order: 3, 32768 bytes)
<4>Inode-cache hash table entries: 4096 (order: 2, 16384 bytes)
<6>Memory: 30336k/30684k available (1477k kernel code, 320k reserved, 335k data, 112k init, 0k highmem)
<6>totalram_pages= 7591
<4>Calibrating delay loop... 211.35 BogoMIPS (lpj=1056768)
<4>loops_per_jiffy=1056768
<4>Mount-cache hash table entries: 512
<4>Checking for 'wait' instruction...  available.
<7>Calling initcall 0x941d4414: helper_init+0x0/0x30()
<7>Calling initcall 0x941d4558: ksysfs_init+0x0/0x3c()
<7>Calling initcall 0x941d640c: filelock_init+0x0/0x48()
<7>Calling initcall 0x941d6c50: init_script_binfmt+0x0/0xc()
<7>Calling initcall 0x941d6c5c: init_elf_binfmt+0x0/0xc()
<7>Calling initcall 0x941dba7c: netlink_proto_init+0x0/0x2ac()
<6>NET: Registered protocol family 16
<7>Calling initcall 0x941d74cc: kobject_uevent_init+0x0/0x50()
<7>Calling initcall 0x941d7754: tty_class_init+0x0/0x34()
<7>Calling initcall 0x941cb57c: frame_info_init+0x0/0xb4()
<4>Can't analyze prologue code at 9416fb5c
<7>Calling initcall 0x941d60fc: init_bio+0x0/0x17c()
<7>Calling initcall 0x941d7e00: misc_init+0x0/0xc0()
<7>Calling initcall 0x941d9814: genhd_device_init+0x0/0x44()
<7>Calling initcall 0x941d9f2c: init_mtd+0x0/0x48()
<7>Calling initcall 0x941dacf0: input_init+0x0/0x1e0()
<7>Calling initcall 0x941db514: proto_init+0x0/0x48()
<7>Calling initcall 0x941db6d8: net_dev_init+0x0/0x1dc()
<7>Calling initcall 0x941d6360: init_pipe_fs+0x0/0x6c()
<7>Calling initcall 0x941d75e4: chr_dev_init+0x0/0xb4()
<7>Calling initcall 0x941d3a94: create_proc_profile+0x0/0x64()
<7>Calling initcall 0x941d3b74: ioresources_init+0x0/0x6c()
<7>Calling initcall 0x941d3d28: uid_cache_init+0x0/0x88()
<7>Calling initcall 0x941d4104: param_sysfs_init+0x0/0x208()
<7>Calling initcall 0x941d430c: init_posix_timers+0x0/0x108()
<7>Calling initcall 0x941d4444: init_posix_cpu_timers+0x0/0xd4()
<7>Calling initcall 0x941d4518: kallsyms_init+0x0/0x40()
<7>Calling initcall 0x941d5794: init_per_zone_pages_min+0x0/0x64()
<7>Calling initcall 0x941d5bdc: pdflush_init+0x0/0x28()
<7>Calling initcall 0x941d5c04: cpucache_init+0x0/0x20()
<7>Calling initcall 0x941d5f50: kswapd_init+0x0/0x70()
<7>Calling initcall 0x941d6004: init_tmpfs+0x0/0x3c()
<7>Calling initcall 0x941d63cc: fasync_init+0x0/0x40()
<7>Calling initcall 0x941d6ad0: aio_setup+0x0/0x8c()
<7>Calling initcall 0x941d6b5c: inotify_setup+0x0/0xf4()
<7>Calling initcall 0x941d7194: init_devpts_fs+0x0/0x48()
<7>Calling initcall 0x941d71dc: init_squashfs_fs+0x0/0xe4()
<6>Squashfs 2.2-r2b (released 2006/02/23) (C) 2002-2005 Phillip Lougher
<7>Calling initcall 0x941d72c0: init_ramfs_fs+0x0/0xc()
<7>Calling initcall 0x941d72d8: ipc_init+0x0/0x30()
<7>Calling initcall 0x941d7698: rand_initialize+0x0/0x3c()
<7>Calling initcall 0x941d7788: tty_init+0x0/0x184()
<7>Calling initcall 0x941d790c: pty_init+0x0/0x4f4()
<7>Calling initcall 0x941d7ec0: avm_power_init+0x0/0x11c()
<7>Calling initcall 0x941d7fdc: avm_sammel_init+0x0/0x158()
<4>[avm] configured: watchdog eventled enable shift register enable direct gpio 
<4>	gpio usage: reset=12 clock=13 store=10 data=9 
<4>AR7WDT: Watchdog Driver for AR7 Hardware (Version 1.0, build: Aug 25 2006 12:35:26)
<7>Calling initcall 0x941d8e60: serial8250_init+0x0/0x10c()
<6>Serial: 8250/16550 driver $Revision: 1.90 $ 1 ports, IRQ sharing disabled
<4>[uart_add_one_port]
<4>ttyS0 at MMIO 0x0 (irq = 15) is a OHIO_UART
<4>[uart_add_one_port] dont rigister console port->type = 16
<4>port->cons = 0x941a7680 port->cons->flags = 0x7
<4>[uart_add_one_port] sucess
<7>Calling initcall 0x941d9858: noop_init+0x0/0xc()
<6>io scheduler noop registered
<7>Calling initcall 0x941d9864: cpmac_main_probe+0x0/0xd8()
<7>Calling initcall 0x941d993c: cpphy_entry_probe+0x0/0xcc()
<7>Calling initcall 0x941d9a08: cpphy_entry_probe+0x0/0xe8()
<3>cpmac_if_register, dev cpmac0 (phy_id 0) registered
<7>Calling initcall 0x941d9af0: cpphy_entry_probe+0x0/0xcc()
<7>Calling initcall 0x941d9bbc: cpphy_entry_probe+0x0/0xe8()
<3>cpmac_if_register, phy_id 0 already registered
<7>Calling initcall 0x941d9d34: net_olddevs_init+0x0/0x100()
<7>Calling initcall 0x941d9e9c: tun_init+0x0/0x90()
<6>tun: Universal TUN/TAP device driver, 1.6
<6>tun: (C) 1999-2004 Max Krasnyansky <maxk@qualcomm.com>
<7>Calling initcall 0x941d9f74: cmdline_parser_init+0x0/0xc()
<7>Calling initcall 0x941d9f80: init_mtdchar+0x0/0xc0()
<7>Calling initcall 0x941da040: init_mtdblock+0x0/0xc()
<7>Calling initcall 0x941da04c: cfi_probe_init+0x0/0x24()
<7>Calling initcall 0x941da070: cfi_amdstd_init+0x0/0x30()
<7>Calling initcall 0x941da0a0: cfi_intelext_init+0x0/0x4c()
<7>Calling initcall 0x941da0ec: jedec_probe_init+0x0/0x24()
<7>Calling initcall 0x941da110: map_ram_init+0x0/0x24()
<7>Calling initcall 0x941da134: init_physmap+0x0/0x170()
<5>physmap flash device: 400000 at 10000000
<6>phys_mapped_flash: Found 1 x16 devices at 0x0 in 16-bit bank
<4> Amd/Fujitsu Extended Query Table at 0x0040
<4>phys_mapped_flash: Swapping erase regions for broken CFI table.
<5>number of CFI chips: 1
<5>cfi_cmdset_0002: Disabling erase-suspend-program due to code brokenness.
<5>RedBoot partition parsing not available
<7>Calling initcall 0x941da2a4: platram_init+0x0/0x30()
<4>Generic platform RAM MTD, (c) 2004 Simtec Electronics
<7>Calling initcall 0x941da2d4: init_ohio_flash+0x0/0xa1c()
<5>Ohio flash driver (size->0x400000 mem->0x10000000)
<4>flash_size=0x800000
<6>Ohio flash memory: Found 1 x16 devices at 0x0 in 16-bit bank
<4> Amd/Fujitsu Extended Query Table at 0x0040
<4>Ohio flash memory: Swapping erase regions for broken CFI table.
<5>number of CFI chips: 1
<5>cfi_cmdset_0002: Disabling erase-suspend-program due to code brokenness.
<4>[mtd]: jffs2_size = 32 * 64KByte (0x200000 Bytes)
<4>[ohio_find_hidden_filesystem]: super block found: bytes_used: 0x41d326/4313894
<4>[init_ohio_flash] find hidden filesystem size=0x6d3d00 offset=0xac300
<4>[mtd] configure jffs2 partition
<4>[mtd] fs_size=0x4c0000 max=0x2b0000 is=0x200000 max jffs2_size value 43
<5>Creating 7 MTD partitions on "Ohio flash memory":
<5>0x000ac300-0x00780000 : "filesystem"
<5>	'nor-flash'
<4>	'Bits can be cleared (flash)'
<4>	'Has an erase function'
<4>mtd: partition "filesystem" doesn't start on an erase block boundary -- force read-only
<5>0x00010000-0x00780000 : "kernel"
<5>	'nor-flash'
<4>	'Bits can be cleared (flash)'
<4>	'Has an erase function'
<5>0x00000000-0x00010000 : "bootloader"
<5>	'nor-flash'
<4>	'Bits can be cleared (flash)'
<4>	'Has an erase function'
<4>	'Virtual blocks not allowed'
<5>0x00780000-0x007c0000 : "tffs (1)"
<5>	'nor-flash'
<4>	'Bits can be cleared (flash)'
<4>	'Has an erase function'
<4>	'Virtual blocks not allowed'
<5>0x007c0000-0x00800000 : "tffs (2)"
<5>	'nor-flash'
<4>	'Bits can be cleared (flash)'
<4>	'Has an erase function'
<4>	'Virtual blocks not allowed'
<5>0x00580000-0x00780000 : "jffs2"
<5>	'nor-flash'
<4>	'Bits can be cleared (flash)'
<4>	'Has an erase function'
<4>	'Virtual blocks not allowed'
<5>0x00010000-0x00580000 : "Kernel without jffs2"
<5>	'nor-flash'
<4>	'Bits can be cleared (flash)'
<4>	'Has an erase function'
<4>	'Virtual blocks not allowed'
<7>Calling initcall 0x941daed0: kcapi_init+0x0/0x9c()
<7>Calling initcall 0x941db014: capi_init+0x0/0x324()
<5>capi20: Rev 1.1.2.7: started up with major 68 (middleware+capifs)
<7>Calling initcall 0x941db338: capifs_init+0x0/0x108()
<5>capifs: Rev 1.1.2.3
<7>Calling initcall 0x941dc72c: inet_init+0x0/0x504()
<6>NET: Registered protocol family 2
<4>IP route cache hash table entries: 512 (order: -1, 2048 bytes)
<4>TCP established hash table entries: 2048 (order: 2, 16384 bytes)
<4>TCP bind hash table entries: 2048 (order: 1, 8192 bytes)
<6>TCP: Hash tables configured (established 2048 bind 2048)
<6>TCP reno registered
<7>Calling initcall 0x941df168: init+0x0/0x8()
<7>Calling initcall 0x941df170: bictcp_register+0x0/0xc()
<6>TCP bic registered
<7>Calling initcall 0x941df17c: mcfw_init_module+0x0/0x88()
<6>mcfw_init: ok
<7>Calling initcall 0x941df204: af_unix_init+0x0/0xa0()
<6>NET: Registered protocol family 1
<7>Calling initcall 0x941df2a4: packet_init+0x0/0x80()
<6>NET: Registered protocol family 17
<7>Calling initcall 0x941df324: br_init+0x0/0x68()
<7>Calling initcall 0x941df3e0: atm_init+0x0/0xec()
<6>NET: Registered protocol family 8
<6>NET: Registered protocol family 20
<7>Calling initcall 0x941df584: br2684_init+0x0/0x54()
<7>Calling initcall 0x941ca2a0: ohio_install_dummy_irq_functions+0x0/0x58()
<4>[setup_irq]: irq 1 irqaction->handler 0x94001664 (dummy_timer_irq+0x0/0x14 )
<4>[setup_irq]: irq 6 irqaction->handler 0x94001678 (dummy_system_irq_2+0x0/0x18 )
<7>Calling initcall 0x9400174c: ohio_late_init+0x0/0x3c()
<4>[ohio_late_init] 
<4>[ohio_set_clock_notify] avm_clock_id_system notify disable 0x9400169c 0x94277e48
<4>[ohio_set_clock_notify] avm_clock_id_system notify enable 0x9400169c 0x94277e48
<7>Calling initcall 0x941ca3f8: ohio_clk_switch_init+0x0/0x60()
<7>Calling initcall 0x941ca6ac: speedup_init+0x0/0x2c()
<7>Calling initcall 0x941d76d4: seqgen_init+0x0/0x20()
<7>Calling initcall 0x940c5b18: avm_event_push_button_init+0x0/0x114()
<7>Calling initcall 0x941d8610: tffs_init+0x0/0x2e4()
<6>[tffs] alloc_chrdev_region() param=mtd4
<6>[tffs] CONFIG_TFFS_MTD_DEVICE_0=4 CONFIG_TFFS_MTD_DEVICE_1=5
<6>[tffs] Character device init successfull 
<4>TFFS: tiny flash file system driver. GPL (c) AVM Berlin (Version 2.0)
<4>      mount on mtd4 and mtd5 (double buffering)
<6>Adam2 environment variables API installed.
<7>Calling initcall 0x941d9524: early_uart_console_switch+0x0/0xb8()
<7>Calling initcall 0x94113d08: net_random_reseed+0x0/0x34()
<7>Calling initcall 0x941ddc50: ip_auto_config+0x0/0x1140()
<4>[prepare_namespace] new mount root /dev/mtdblock1
<6>tffsd: wait for events
<4>use lzma compression 
<4>VFS: Mounted root (squashfs filesystem) readonly.
<4>Freeing prom memory: 0kb freed
<6>Freeing unused kernel memory: 112k freed (7619 free)
<4>[setup_irq]: irq 15 irqaction->handler 0x940cf534 (serial8250_interrupt+0x0/0x128 )
<4>[setup_irq]: irq 15 irqaction->handler 0x940cf534 (serial8250_interrupt+0x0/0x128 )
<4>[setup_irq]: irq 15 irqaction->handler 0x940cf534 (serial8250_interrupt+0x0/0x128 )
<4>[setup_irq]: irq 15 irqaction->handler 0x940cf534 (serial8250_interrupt+0x0/0x128 )
<4>AR7WDT: System Init UEberwachung 240 Sekunden
<4>TFFS Name Table 8
<4>Piglet: module license '
<4>(C) Copyright 2005 by AVM
<4>' taints kernel.
<6>JFFS2 version 2.2. (NAND) (C) 2001-2003 Red Hat, Inc.
<4>registered device TI Avalanche SAR
<4>tiatm driver (patch_annex=0xc00f69ec)
<4>[tiatm] Set StrictPriority=0
<4>DSP binary filesize = 300480 bytes
<4>[setup_irq]: irq 23 irqaction->handler 0xc00de20c (tn7atm_sar_irq+0x0/0x30 [tiatm] )
<4>[setup_irq]: irq 31 irqaction->handler 0xc00de23c (tn7atm_dsl_irq+0x0/0x28 [tiatm] )
<4>[tiatm]: Powermanagment (States => 1,3,10) supported!
<4>Texas Instruments ATM driver: version:[4.06.04.30]
<4>ubik2 driver (ubik2 - 0x10=0xc00c8f54)
<4>atm_dsp_register_ubik2: ubik2_ToMIPS_notify=0xc00bac00
<4>atm_dsp_register_ubik2: dsp mem pointer 0xa1c0f1f0
<3>ubik2_init_interface: DSP-Link Version v3 8480
<6>isdn_fbox: Loading...
<6>isdn_fbox: Driver 'isdn_fbox' attached to stack
<6>isdn_fbox: CAPI driver registered.
<4>isdn_fbox: AVM F!Box expected @ port 0x0000, irq 0
<4>isdn_fbox: Loading...
<4>gpio_ssi_init: done
<6>isdn_fbox: Stack version 3.11-07
<6>isdn_fbox: D-channel 0: DSS1  
<6>isdn_fbox: D-channel 1: DSS1  
<6>isdn_fbox: D-channel 2: DSS1_N
<6>isdn_fbox: D-channel 3: POTS  
<6>isdn_fbox: D-channel 4: SIP   
<4>isdn_fbox: Loaded.
<4>BLK: DECT StartUp, mode = WAIT, firmware: 00.13.12
<4>BLK: DECT StartUp, mode = MASTER INIT, firmware: 00.13.12
<6>usbcore: registered new driver usbfs
<6>usbcore: registered new driver hub
<6>	AHCI RevisionID = 0x02, RamSize = 16384, NumPorts= 1
<4>[setup_irq]: irq 9 irqaction->handler 0xc00543b0 (ahci_irq+0x0/0x8a0 [usbahcicore] )
<6>ahci : new USB bus registered, assigned bus number 1
<6>kdsldmod: init start
<6>kdsld: cache_create(datapipe)
<6>kdsld: cache_create(datapipe_mod)
<6>kdsld: cache_create(ipaccessset)
<6>kdsld: cache_create(ipaccessrule)
<6>kdsld: cache_create(ipfragid)
<6>kdsld: cache_create(ipmasqentry)
<6>kdsld: cache_create(ipmasqfwinfo)
<6>kdsld: cache_create(ipmasqigdpm)
<6>kdsld: cache_create(ipmasqappldata)
<6>kdsld: cache_create(ipmasqmcgroup)
<6>kdsld: cache_create(dnsmasqentry)
<6>kdsld: cache_create(dnsstaticentry)
<6>kdsld: cache_create(pingerentry)
<6>kdsld: cache_create(pingerwaiter)
<6>kdsld: cache_create(iprouteset)
<6>kdsld: DATAPIPE: with header optimization
<6>kdsldmod: init done
<6>kdsld: PPP led: off (value=0)
<6>hub 1-0:1.0: USB hub found
<6>hub 1-0:1.0: 1 port detected
<4>[speedup] disable
<4>429493997: Configuration succeeded !!!
<4>[ohio_vlynq_init] device 0
<4>[ohio_vlynq_startup_link] 
<4>[setup_irq]: irq 29 irqaction->handler 0x94004f2c (vlynq_interrupt+0x0/0x34 )
<4>[setup_irq]: irq 79 irqaction->handler 0xc04d0a10 (whal_acxIntrHandler+0x0/0x1e8 [tiap] )
<4>429494004:  
<4>429494027: WDRV_MAINSM: WLAN Driver initialized successfully
<4>
<4>429494027: FW Watchdog is Enabled
<6>dda: tiwlan0 in initializing Succeeded wireless extensions: ret = 0
<6>tiwlan0 device is activated
<6>wdsup0 device is activated
<6>wdsdw0 device is activated
<6>wdsdw1 device is activated
<6>wdsdw2 device is activated
<6>wdsdw3 device is activated
<4>[setup_irq]: irq 27 irqaction->handler 0x940e016c (cpmac_main_isr+0x0/0x78 )
<3>cpmac_main_ioctl, unknown ioctl 35142
<6>device eth0 entered promiscuous mode
<6>device cpmac0 entered promiscuous mode
<6>lan: port 1(eth0) entering learning state
<4>tiwlan_ddaDoIoctl : Unknown ioctl 35142
<6>device tiwlan0 entered promiscuous mode
<6>lan: port 2(tiwlan0) entering learning state
<4>tiwlan_ddaWdsDoIoctl : Unknown ioctl 35142
<6>device wdsup0 entered promiscuous mode
<6>lan: port 3(wdsup0) entering learning state
<4>tiwlan_ddaWdsDoIoctl : Unknown ioctl 35142
<6>device wdsdw0 entered promiscuous mode
<6>lan: port 4(wdsdw0) entering learning state
<4>tiwlan_ddaWdsDoIoctl : Unknown ioctl 35142
<6>device wdsdw1 entered promiscuous mode
<6>lan: port 5(wdsdw1) entering learning state
<4>tiwlan_ddaWdsDoIoctl : Unknown ioctl 35142
<6>device wdsdw2 entered promiscuous mode
<6>lan: port 6(wdsdw2) entering learning state
<4>tiwlan_ddaWdsDoIoctl : Unknown ioctl 35142
<6>device wdsdw3 entered promiscuous mode
<6>lan: port 7(wdsdw3) entering learning state
<6>kdsld: sync lost
<3>[avm_led] virt led not registered
<3>[avm_led] format error: "SET Name,Instanz = state"
<4>BLK: DECT StartUp, mode = NORMAL, firmware: 00.13.12
<4>AR7WDT: System Init UEberwachung abgeschlossen (199440 ms noch verfuegbar)
<6>SysRq : Changing Loglevel
<4>Loglevel set to 4
<6>lan: topology change detected, propagating
<6>lan: port 1(eth0) entering forwarding state
<6>lan: topology change detected, propagating
<6>lan: port 2(tiwlan0) entering forwarding state
<6>lan: topology change detected, propagating
<6>lan: port 3(wdsup0) entering forwarding state
<6>lan: topology change detected, propagating
<6>lan: port 4(wdsdw0) entering forwarding state
<6>lan: topology change detected, propagating
<6>lan: port 5(wdsdw1) entering forwarding state
<6>lan: topology change detected, propagating
<6>lan: port 6(wdsdw2) entering forwarding state
<6>lan: topology change detected, propagating
<6>lan: port 7(wdsdw3) entering forwarding state
<6>mcfw_query_sent: tiwlan:0:0.0.0.0 1000
<4>[tiatm] DSL in training!
}}}






== Configuration Files ==
=== AVM public-key1 ===
{{{
~ # cat /etc/avm_firmware_public_key1
00e90086875d8c11ffa1437bd6569b7c0061ec77a5059d1c0acc97883160d72bfebb743e1230821d1c7e976c3e6c1c608ee2c40a80380cd12290566ecb5df9819a2eb16349d1e496d037bb5d121d13c4b46939bdfd6a2d5d0261155c5f43c62ff6c1dcea8ec21ad482bce59f0279242747b8e52efcc25da81c97cd1c3f12516e87
010001
}}}

=== AVM public-key2 ===
{{{
~ # cat /etc/avm_firmware_public_key2
00d3eb28173d70a3a38727d2477000c48136c64a24e9d486a0fa7dc3e12a4a7fd35b240d977c20f54610cea16b504ce4d8bbd2617bdfbf035f0ba9ecb14cb585311f88952335f0f6bac2c9b5c831adaf9c848ce48265b8b3de4f9895915a8d9a0328fb1609af628179636f8f4359ff5b9bf9ce68cdf343fa85e31a5c178d3e0639
010001
}}}

=== AVM LED Configuration ===
{{{
~ # cat etc/led.conf
DEF error,0 = 1,128,0,all
DEF error,1 = 3,127,0,all
DOUBLE error,0 TO error,1
DEF power,0 = 0,7,0,power
DEF power,1 = 0,7,0,power
DEF tr69,0 = 2,6,1,tr69
DEF cpmac,3 = 99,0,2,lan4
DEF cpmac,2 = 99,1,3,lan3
DEF cpmac,1 = 99,2,4,lan2
DEF cpmac,0 = 99,3,5,lan
DEF wlan,0 = 2,5,6,wlan
DEF ab,1 = 2,4,7,festnetz
DEF ab,2 = 2,4,7,festnetz
DEF ab,3 = 2,4,7,festnetz
DEF voip_con,0 = 2,3,8,voip_con
DEF pppoe,0 = 2,2,9,dsl
DEF pppoe,1 = 2,2,9,dsl
DEF adsl,0 = 2,1,10,adsl
DEF adsl,1 = 2,1,10,adsl
DEF ata,0 = 2,0,11,ata
DEF usb,0 = 99,32,12,usb
DEF usb,1 = 99,32,12,usb
DEF blockring,0 = 99,32,13,blockring
DEF missedcall,0 = 99,32,14,missedcall
DEF budget,0 = 99,32,15,budget
DEF info,0 = 99,32,16,info
DEF info,1 = 99,32,16,info
DEF info,2 = 99,32,16,info
DEF info,3 = 99,32,16,info
DEF info,4 = 99,32,16,info
DEF internet,0 = 99,32,17,internet
DEF internet,1 = 99,32,17,internet
DEF isdn,0 = 99,32,18,isdn
DEF isdn,1 = 99,32,18,isdn
DEF isdn,2 = 99,32,18,isdn
DEF stick_surf,0 = 99,32,19,stick_surf
DEF stick_surf,1 = 99,32,19,stick_surf
DEF stick_surf,2 = 99,32,19,stick_surf
DEF stick_surf,3 = 99,32,19,stick_surf
DEF null,0 = 99,32,20,null_device
MAP budget,0 TO info,1
MAP ab,2 TO null,0
SET power,0 = 1
}}}


=== Network Interfaces Configuration ===
{{{ 
~ # ifconfig
cpmac0    Link encap:Ethernet  HWaddr 00:04:0E:E4:D4:9C
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:256
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

eth0      Link encap:Ethernet  HWaddr 00:04:0E:E4:D4:9C
          UP BROADCAST RUNNING ALLMULTI MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:28 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:0 (0.0 B)  TX bytes:1512 (1.4 KiB)

lan       Link encap:Ethernet  HWaddr 00:04:0E:E4:D4:9C
          inet addr:192.168.2.1  Bcast:192.168.2.255  Mask:255.255.255.0
          UP BROADCAST RUNNING ALLMULTI MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:30 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:0 (0.0 B)  TX bytes:1512 (1.4 KiB)

lan:0     Link encap:Ethernet  HWaddr 00:04:0E:E4:D4:9C
          inet addr:192.168.2.254  Bcast:192.168.2.255  Mask:255.255.255.0
          UP BROADCAST RUNNING ALLMULTI MULTICAST  MTU:1500  Metric:1

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:16436  Metric:1
          RX packets:295 errors:0 dropped:0 overruns:0 frame:0
          TX packets:295 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:36415 (35.5 KiB)  TX bytes:36415 (35.5 KiB)

tiwlan0   Link encap:Ethernet  HWaddr 00:04:0E:E4:D4:9E
          UP BROADCAST RUNNING ALLMULTI MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:28 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:1400 (1.3 KiB)

wdsdw0    Link encap:Ethernet  HWaddr 00:04:0E:E4:D4:9E
          UP BROADCAST RUNNING ALLMULTI MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:28 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:1400 (1.3 KiB)

wdsdw1    Link encap:Ethernet  HWaddr 00:04:0E:E4:D4:9E
          UP BROADCAST RUNNING ALLMULTI MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:28 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:1400 (1.3 KiB)

wdsdw2    Link encap:Ethernet  HWaddr 00:04:0E:E4:D4:9E
          UP BROADCAST RUNNING ALLMULTI MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:28 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:1400 (1.3 KiB)

wdsdw3    Link encap:Ethernet  HWaddr 00:04:0E:E4:D4:9E
          UP BROADCAST RUNNING ALLMULTI MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:28 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:1400 (1.3 KiB)

wdsup0    Link encap:Ethernet  HWaddr 00:04:0E:E4:D4:9E
          UP BROADCAST RUNNING ALLMULTI MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:28 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:1400 (1.3 KiB)
}}}

== Original Firmware Info ==
=== Backing up original firmware ===

 TODO

To backup the original firmware you'll need console access to the device, e.g. use a serial cable and minicom to access the root shell.

You can then dump each mtd block to a file and download them via the built in web browser as below.

{{{
mkdir /var/backup
cp /dev/mtd1 /var/backup/mtd1-rootfs.bin
cp /dev/mtd2 /var/backup/mtd2-kernel-and-rootfs.bin
cp /dev/mtd3 /var/backup/mtd3-bootloader.bin
cp /dev/mtd4 /var/backup/mtd4-nvram1.bin
cp /dev/mtd5 /var/backup/mtd5-nvram2.bin
kill `pidof websrv`
cd /var
rm html
ln -s backup html
websrv
}}}

You can then download the files by pointing your webbrowser at {{{http://<your router's ip>/mtd0-rootfs.bin}}} etc.

All being well you should have files like this

{{{
# ls -al
drwxr-xr-x    2 root     root            0 Jan  1 01:10 .
drwxrwxr-x    8 root     root            0 Jan  1 01:04 ..
-rw-r--r--    1 root     root      7158272 Jan  1 01:09 mtd1-rootfs.bin
-rw-r--r--    1 root     root      7798784 Jan  1 01:09 mtd2-kernel-and-rootfs.bin
-rw-r--r--    1 root     root        65536 Jan  1 01:09 mtd3-bootloader.bin
-rw-r--r--    1 root     root       262144 Jan  1 01:10 mtd4-nvram1.bin
-rw-r--r--    1 root     root       262144 Jan  1 01:10 mtd5-nvram2.bin
}}}

Another way is to do this via a tftp (tftp server eg ip 192.168.178.21):
{{{ 
cat /dev/mtdblock3 | tftp -p -l - -r urlader.image 192.168.178.21
cat /dev/mtdblock7 | tftp -p -l - -r kernel.image 192.168.178.21
}}}
this can be easily flashed back via adam2 for recovering

=== Restoring Original Firmware ===
==== Undoing changes to adam2 config ====
 TODO: finish this section, here's some hints for now

{{{
setenv kernel_args idle=4
unsetenv mtd5
}}}

=== Building / compiling the T-Home/AVM factory made 2.6.13.1-ohio kernel ===

For compiling the original (factory made) kernel 2.6.13.1-ohio

get the GPL sources from:

http://www.t-home.de/dlp/eki/downloads/Speedport/Speedport%20W%20900V/GPL-r4884-8mb_26-tar.bz2



* unzip GPL-r4884-8mb_26-tar.bz2
{{{
tar -jxvf GPL-r4884-8mb_26-tar.bz2 /buildroot/W900V-orig/
}}}

* setting up/correct buildroot
{{{

su -c 'mkdir -p /home/jpluschke/FBox/spezial/'
su -c 'ln -s /home/debian/buildroot/W900V-orig /home/jpluschke/FBox/spezial/r4884'
find ../ -wholename '/proc' -prune -o -type l -print0 | xargs -0 file - | grep broken | cut -d ":" -f 1
ll
}}}

* compile kernel in buildroot/W900V-orig/GPL with

{{{
make
}}}

* the recently built kernel is located here:
{{{
buildroot/W900V-orig/GPL/kernel_8mb_26_build/kernel/linux-2.6.13.1/vmlinux

vmlinux: ELF 32-bit LSB executable, MIPS, version 1 (SYSV), statically linked, not stripped
}}}

== Original Flash Map ==
Bootloader: ADAM2 (+ Environment)

Kernel: Linux 2.6.13.1-ohio

Filesystem: SquashFS (lzma compressed)

Configuration-Files: TFFS

Hidden Root

Bei Hidden Root befindet sich das root Dateisystem – ähnlich wie bei Contiguous SquashFS – direkt hinter dem Kernel (256 Byte Padding). Diese Boxen kann man daran erkennen, dass die Start- und End-Adresse von mtd0 in der mtd Tabelle gleich 0 und die Datei filesystem.image im Firmware Update leer ist. kernel.image enthält sowohl den Kernel als auch das root Dateisystem. Hidden Root am Beispiel der Fon WLAN 7170 (die kürzeren Balken für den Kernel und das root Dateisystem sollen nur andeuten, dass auf der Box von den 8 MB noch einiges mehr frei ist, als bei den 4 MB Boxen):

Diese Boxen verwenden Hidden Root:

    * Fritzbox Fon WLAN 7140
    * Fritzbox Fon WLAN 7170
    * Speedport W900V

Bei Hidden Root befindet sich das root Dateisystem – ähnlich wie bei Contiguous SquashFS – direkt hinter dem Kernel (256 Byte Padding). Diese Boxen kann man daran erkennen, dass die Start- und End-Adresse von mtd0 in der mtd Tabelle gleich 0 und die Datei filesystem.image im Firmware Update leer ist. kernel.image enthält sowohl den Kernel als auch das root Dateisystem. Hidden Root am Beispiel der Fon WLAN 7170 (die kürzeren Balken für den Kernel und das root Dateisystem sollen nur andeuten, dass auf der Box von den 8 MB noch einiges mehr frei ist, als bei den 4 MB Boxen):

http://img519.imageshack.us/img519/2088/flashspw900vhiddenrootdz2.png

 (TODO)
||'''partition''' ||'''start''' ||'''end''' ||'''size''' ||'''description''' ||
||mtd0 ||{{{0x90000000}}} ||{{{0x90000000}}} ||{{{0x000000}}} ||Hidden Root! ||
||mtd1 ||{{{0x90010000}}} ||{{{0x90780000}}} ||{{{0x770000}}} ||kernel+jffs2 (jffs starts at 0x00580000) ||
||mtd2 ||{{{0x90000000}}} ||{{{0x90010000}}} ||{{{0x010000}}} ||ADAM2/bootloader ||
||mtd3 ||{{{0x90780000}}} ||{{{0x907c0000}}} ||{{{0x040000}}} ||tffs (1) ? ||
||mtd4 ||{{{0x907c0000}}} ||{{{0x90800000}}} ||{{{0x040000}}} ||tffs (2) ? ||

 Physical order of partitions on flash chip is:

mtd0,mtd2,mtd1,mtd3,mtd4

mtd0 is Hidden Root!


=== Bootloader ADAM2 / EVA ===
EVA ist der neue AVM-Bootloader für Fritzboxen mit 2.6er Kernel und löst ADAM2 in den Fritzboxen nach und nach ab.

'''Konfiguration'''

EVA wird in den ersten 64kB des Flash gespeichert (mtd2, 0x90000000-0x90010000). In diesem Binary gibt es zwei Bereiche, die die grundlegende Konfiguration der Box "einfrieren". Bei Version 1203 (laut urlader-version aus dem Environment) liegt bei 0xae0 der Defaultbereich und bei 0x580 der individuelle Bereich, der sich pro Box unterscheidet und unter anderem die MAC-Adressen enthält. Bei einer Box mit EVA kann man also durch zerstören des Environments seine MAC-Adressen nicht mehr verlieren, dafür aber auch nicht mehr so einfach ändern, da EVA die "eingefrorenen" Werte bei jedem Boot ggf. im Environment wieder auffrischt.

Wenn EVA nicht ab Werk installiert ist, sondern per Update auf die Box kommt, wird der individuelle Konfigbereich beim Update mit dem Utility urlader.setconfig24 erzeugt. Die Datei urlader.config im Update-Image enthält eine Liste mit Environmentnamen, die dabei "eingefroren" werden sollen (Liste aus dem 7050 Update 14.04.31, kann bei anderen Boxen evtl. abweichen):


The bootloader is ADAM2 which provides a console on the serial port and allows flashing via FTP.  The W900V's bootloader's default IP address is 192.168.178.1, you should be able to ping it on that address a second or two after the device is rebooted.  If not reset the device back to its defaults using the reset button on the back (see tcom's site for info on how to do this).  My router came with a very recent edition, v1.153, from August 2006.

{{{
maca                    overwrite
macb                    overwrite
macwlan                 overwrite
macdsl                  overwrite
usb_board_mac           overwrite
usb_rndis_mac           overwrite
bluetooth               overwrite
reserved                overwrite
HWRevision              overwrite
ProductID               overwrite
SerialNumber            overwrite
usb_device_id           overwrite
usb_revision_id         overwrite
usb_manufacturer_name   overwrite
annex                   overwrite
}}}
Der Konfigbereich selbst sieht in etwa so aus:

=== Loaderformat für LZMA-komprimierte Binaries ===
Neben dem alten TI Binärformat unterstützt EVA auch LZMA-komprimierte Kernel ohne integrierten Entpacker. Diese Kernel werden von EVA vor dem Booten entpackt. Der neue Header sieht in etwa so aus:

{{{
 magic:
   0xfeed1281  32bit little endian
}}}
{{{
 record: (wie TI-Format)
   length              32bit little endian
   loadaddress         32bit little endian
   data                length bytes
   checksum            Zweierkomplement der 32bit Summe über
                       length (32bit Addition),
                       loadaddress (32bit Addition),
                       data (byteweise 8bit Addition)
}}}
{{{
 lzma record: (innerhalb von data)
   0x075a0201          32bit little endian (7'Z' 2.1?)
                       unterscheidet/beschreibt evtl. den Record Inhalt?
   compressed length   32bit little endian
   uncompressed length 32bit little endian
   checksum            32bit little endian crc32 of compressed data
   compressed data     compressed length + 8 bytes
}}}
{{{
 lzma compressed data:
   lzma properties      8bit
   lzma dict size      32bit little endian
   alignment           24bit == 0x000000
   packed data         compressed length octets
}}}
{{{
 entry record: (wie TI-Format)
   0x0                 32bit
   entryaddress        32bit little endian
}}}
Mit folgendem Perl-Skript lässt sich ein Kernel aus kernel.image entpacken:

{{{
#! /usr/bin/perl
use Compress::unLZMA;
use Archive::Zip;
open INPUT, "<$ARGV[0]" or die "can't open $ARGV[0]: $!";
read INPUT, $buf, 4;
$magic = unpack("V", $buf);
if ($magic != 0xfeed1281) {
  die "bad magic";
}
read INPUT, $buf, 4;
$len = unpack("V", $buf);
read INPUT, $buf, 4*2; # address, unknown
read INPUT, $buf, 4;
$clen = unpack("V", $buf);
read INPUT, $buf, 4;
$dlen = unpack("V", $buf);
read INPUT, $buf, 4;
$cksum = unpack("V", $buf);
printf "Archive checksum: 0x%08x\n", $cksum;
read INPUT, $buf, 1+4; # properties, dictionary size
read INPUT, $dummy, 3; # alignment
$buf .= pack('VV', $dlen, 0); # 8 bytes of real size
#$buf .= pack('VV', -1, -1); # 8 bytes of real size
read INPUT, $buf2, $clen;
$crc = Archive::Zip::computeCRC32($buf2);
printf "Input CRC32: 0x%08x\n", $crc;
if ($cksum != $crc) {
  die "wrong checksum";
}
$buf .= $buf2;
$data = Compress::unLZMA::uncompress($buf);
unless (defined $data) {
  die "uncompress: $@";
}
open OUTPUT, ">$ARGV[1]" or die "can't write $ARGV[1]";
print OUTPUT $data;
#truncate OUTPUT, $dlen;
}}}

Es werden also die Einstellungen für den Memory Controller (EMIF), RAM und Flashgröße, MTD setup und ein paar wichtige, unveränderliche Environmentvariablen gespeichert.

=== ADAM2 environment variables ===

{{{

AVM_Ar7 >printenv
HWRevision            102.1.1.0
ProductID             Fritz_Box_DECT_W900V
SerialNumber          0000000000000000
annex                 B
autoload              yes
bootloaderVersion     1.153
bootserport           tty0
bluetooth             00:00:00:00:00:00
cpufrequency          211968000
firstfreeaddress      0x946AE570
firmware_version      tcom
firmware_info         34.04.21
flashsize             0x00800000
jffs2_size            32
maca                  00:00:00:00:00:00
macb                  00:00:00:00:00:00
macwlan               00:00:00:00:00:00
macdsl                00:00:00:00:00:00
memsize               0x02000000
modetty0              38400,n,8,1,hw
modetty1              38400,n,8,1,hw
mtd0                  0x90000000,0x90000000
mtd1                  0x90010000,0x90780000
mtd2                  0x90000000,0x90010000
mtd3                  0x90780000,0x907C0000
mtd4                  0x907C0000,0x90800000
mtd5                  0x90780000,0x90780000
my_ipaddress          192.168.178.1
prompt                AVM_Ar7
ptest
reserved              00:00:00:00:00:00
req_fullrate_freq     125000000
sysfrequency          125000000
urlader-version       1153
usb_board_mac         00:00:00:00:00:00
usb_rndis_mac         00:00:00:00:00:00
usb_device_id         0x0000
usb_revision_id       0x0000
usb_device_name       USB DSL Device
usb_manufacturer_name  AVM
wlan_key              xxxxxxxxxxxxxxxx
wlan_cal              03FF,0006,0017,00F0,010A,0101,010A,02F7,035A
}}}

=== Boot log from old firmware ===

{{{
(AVM) EVA Revision: 1.153 Version: 1153
(C) Copyright 2005 AVM Date: Aug 24 2006 Time: 14:17:25 (0) 2 0-1111
[FLASH:] MACRONIX Top-MirrorBit-Flash 8MB 32 Bytes WriteBuffer
[FLASH:](Eraseregion [0] 127 sectors a 64kB)
[FLASH:](Eraseregion [1] 8 sectors a 8kB)
[SYSTEM:] OHIO on 211MHz/125MHz
AVM_Ar7 >AVM decompress Kernel:
.............done
start kernel
[ohio_pre_init] System Clk = 62500000 Hz                Linux version 2.6.13.1-ohio (jpluschke@EmbeddedVM) (gcc version 3.4.3) #5 Fri Aug 25 12:37:20 CEST 2006
YAMON MEMORY DESCRIPTOR dump:
prom_memsize = 0x02000000
memsize=32 MByte
prom_flashsize = 0x00800000
flashsize=8 MByte
&_end=0x94208ec8 PFN_ALIGN(&_end)=0x94209000 CPHYSADDR(PFN_ALIGN(&_end))=0x14209000 memsize=0x2000000
[0,941e2fc0]: base<14000000> size<00209000> type<Dont use memory>
[0,941e2fcc]: base<14209000> size<01df7000> type<Free memmory>
prom_memsize = 0x02000000
memsize=32 MByte
prom_flashsize = 0x00800000
flashsize=8 MByte
&_end=0x94208ec8 PFN_ALIGN(&_end)=0x94209000 CPHYSADDR(PFN_ALIGN(&_end))=0x14209000 memsize=0x2000000
CPU revision is: 00018448
ohio_setup+0x0/0x100
[ohio_clk_init]: dsl xtal 35328000Hz lan xtal 25000000Hz
[ohio_gpio_init]
Determined physical RAM map:
 memory: 00209000 @ 14000000 (reserved)
 memory: 01df7000 @ 14209000 (usable)
On node 0 totalpages: 8192
[alloc_node_mem_map] reduce size from 2883616 Bytes to  262176 Bytes
[alloc_node_mem_map]: (org) sizeof(mem_map) = 262176 mem_map=0x9420c000-0x9424c020
[alloc_node_mem_map]: sizeof(mem_map) = 2883616 mem_map=0x93f8c000-0x9424c020
  DMA zone: 8192 pages, LIFO batch:3
  Normal zone: 0 pages, LIFO batch:1
  HighMem zone: 0 pages, LIFO batch:1
Built 1 zonelists
Kernel command line:  console=ttyS0,38400n8r
[ld_mmu_r4xx0] memcpy((void *)(CAC_BASE   + 0x100), &except_vec2_generic, 0x30)
Primary instruction cache 16kB, physically tagged, 4-way, linesize 16 bytes.
Primary data cache 8kB, 4-way, linesize 16 bytes.
Synthesized TLB refill handler (20 instructions). Base=0x941e0734
TLB synthesizer field overflow (simm)
Synthesized TLB load handler fastpath (34 instructions) Base=0x941e3760.
TLB synthesizer field overflow (simm)
Synthesized TLB store handler fastpath (34 instructions) Base=0x941e3960.
TLB synthesizer field overflow (simm)
Synthesized TLB modify handler fastpath (33 instructions) Base=0x941e3b60.
PID hash table entries: 256 (order: 8, 4096 bytes)
[ohio_set_clock_notify] avm_clock_id_cpu notify disable 0x940013f8 0x94199e18
[ohio_set_clock_notify] avm_clock_id_cpu notify enable 0x940013f8 0x94199e18
CPU frequency 211.97 MHz
Using 105.984 MHz high precision timer.
[setup_irq]: irq 127 irqaction->handler 0x94041a10 (no_action+0x0/0x8 )
[register_console] enable commandline console 0
Dentry cache hash table entries: 8192 (order: 3, 32768 bytes)
Inode-cache hash table entries: 4096 (order: 2, 16384 bytes)
Memory: 30336k/30684k available (1477k kernel code, 320k reserved, 335k data, 112k init, 0k highmem)
totalram_pages= 7591
Calibrating delay loop... 211.35 BogoMIPS (lpj=1056768)
loops_per_jiffy=1056768
Mount-cache hash table entries: 512
Checking for 'wait' instruction...  available.
NET: Registered protocol family 16
Can't analyze prologue code at 9416fb5c
Squashfs 2.2-r2b (released 2006/02/23) (C) 2002-2005 Phillip Lougher
[avm] configured: watchdog eventled enable shift register enable direct gpio
gpio usage: reset=12 clock=13 store=10 data=9
AR7WDT: Watchdog Driver for AR7 Hardware (Version 1.0, build: Aug 25 2006 12:35:26)
Serial: 8250/16550 driver $Revision: 1.90 $ 1 ports, IRQ sharing disabled
[uart_add_one_port]
ttyS0 at MMIO 0x0 (irq = 15) is a OHIO_UART
[uart_add_one_port] dont rigister console port->type = 16
port->cons = 0x941a7680 port->cons->flags = 0x7
[uart_add_one_port] sucess
io scheduler noop registered
cpmac_if_register, dev cpmac0 (phy_id 0) registered
cpmac_if_register, phy_id 0 already registered
tun: Universal TUN/TAP device driver, 1.6
tun: (C) 1999-2004 Max Krasnyansky <maxk@qualcomm.com>
physmap flash device: 400000 at 10000000
phys_mapped_flash: Found 1 x16 devices at 0x0 in 16-bit bank
 Amd/Fujitsu Extended Query Table at 0x0040
phys_mapped_flash: Swapping erase regions for broken CFI table.
number of CFI chips: 1
cfi_cmdset_0002: Disabling erase-suspend-program due to code brokenness.
RedBoot partition parsing not available
Generic platform RAM MTD, (c) 2004 Simtec Electronics
Ohio flash driver (size->0x400000 mem->0x10000000)
flash_size=0x800000
Ohio flash memory: Found 1 x16 devices at 0x0 in 16-bit bank
 Amd/Fujitsu Extended Query Table at 0x0040
Ohio flash memory: Swapping erase regions for broken CFI table.
number of CFI chips: 1
cfi_cmdset_0002: Disabling erase-suspend-program due to code brokenness.
[mtd]: jffs2_size = 32 * 64KByte (0x200000 Bytes)
[ohio_find_hidden_filesystem]: super block found: bytes_used: 0x41d326/4313894
[init_ohio_flash] find hidden filesystem size=0x6d3d00 offset=0xac300
[mtd] configure jffs2 partition
[mtd] fs_size=0x4c0000 max=0x2b0000 is=0x200000 max jffs2_size value 43
Creating 7 MTD partitions on "Ohio flash memory":
0x000ac300-0x00780000 : "filesystem"
        'nor-flash'
        'Bits can be cleared (flash)'
        'Has an erase function'
mtd: partition "filesystem" doesn't start on an erase block boundary -- force read-only
0x00010000-0x00780000 : "kernel"
        'nor-flash'
        'Bits can be cleared (flash)'
        'Has an erase function'
0x00000000-0x00010000 : "bootloader"
        'nor-flash'
        'Bits can be cleared (flash)'
        'Has an erase function'
        'Virtual blocks not allowed'
0x00780000-0x007c0000 : "tffs (1)"
        'nor-flash'
        'Bits can be cleared (flash)'
        'Has an erase function'
        'Virtual blocks not allowed'
0x007c0000-0x00800000 : "tffs (2)"
        'nor-flash'
        'Bits can be cleared (flash)'
        'Has an erase function'
        'Virtual blocks not allowed'
0x00580000-0x00780000 : "jffs2"
        'nor-flash'
        'Bits can be cleared (flash)'
        'Has an erase function'
        'Virtual blocks not allowed'
0x00010000-0x00580000 : "Kernel without jffs2"
        'nor-flash'
        'Bits can be cleared (flash)'
        'Has an erase function'
        'Virtual blocks not allowed'
capi20: Rev 1.1.2.7: started up with major 68 (middleware+capifs)
capifs: Rev 1.1.2.3
NET: Registered protocol family 2
IP route cache hash table entries: 512 (order: -1, 2048 bytes)
TCP established hash table entries: 2048 (order: 2, 16384 bytes)
TCP bind hash table entries: 2048 (order: 1, 8192 bytes)
TCP: Hash tables configured (established 2048 bind 2048)
TCP reno registered
TCP bic registered
mcfw_init: ok
NET: Registered protocol family 1
NET: Registered protocol family 17
NET: Registered protocol family 8
NET: Registered protocol family 20
[setup_irq]: irq 1 irqaction->handler 0x94001664 (dummy_timer_irq+0x0/0x14 )
[setup_irq]: irq 6 irqaction->handler 0x94001678 (dummy_system_irq_2+0x0/0x18 )
[ohio_late_init]
[ohio_set_clock_notify] avm_clock_id_system notify disable 0x9400169c 0x94277e48
[ohio_set_clock_notify] avm_clock_id_system notify enable 0x9400169c 0x94277e48
[tffs] alloc_chrdev_region() param=mtd4
[tffs] CONFIG_TFFS_MTD_DEVICE_0=4 CONFIG_TFFS_MTD_DEVICE_1=5
[tffs] Character device init successfull
TFFS: tiny flash file system driver. GPL (c) AVM Berlin (Version 2.0)
      mount on mtd4 and mtd5 (double buffering)
Adam2 environment variables API installed.
[prepare_namespace] new mount root /dev/mtdblock1
tffsd: wait for events
use lzma compression
VFS: Mounted root (squashfs filesystem) readonly.
Freeing prom memory: 0kb freed
Freeing unused kernel memory: 112k freed (7619 free)
[setup_irq]: irq 15 irqaction->handler 0x940cf534 (serial8250_interrupt+0x0/0x128 )
[setup_irq]: irq 15 irqaction->handler 0x940cf534 (serial8250_interrupt+0x0/0x128 )
[setup_irq]: irq 15 irqaction->handler 0x940cf534 (serial8250_interrupt+0x0/0x128 )
3init started:  BusyBox v1.1.0 (2006.03.22-15:32+0000) multi-call binary
[setup_irq]: irq 15 irqaction->handler 0x940cf534 (serial8250_interrupt+0x0/0x128 )
AR7WDT: System Init UEberwachung 240 Sekunden
TFFS Name Table 8
Jan  1 00:00:04 ar7cfgctl[196]: FactoryDefault=/etc/default/tcom/ar7.cfg (ar7)
Jan  1 00:00:04 ar7cfgctl[196]: load_config(ar7): factory default loaded
HWRevision      102.1.1.0
ProductID       Fritz_Box_DECT_W900V
SerialNumber    0000000000000000
annex   B
autoload        yes
bootloaderVersion       1.153
bootserport     tty0
bluetooth       00:04:0E:FF:FF:07
cpufrequency    211968000
firstfreeaddress        0x946AE570
firmware_version        tcom
firmware_info   34.04.21
flashsize       0x00800000
jffs2_size      32
maca    XX:XX:XX:XX:XX:XX
macb    XX:XX:XX:XX:XX:XX+1
macwlan XX:XX:XX:XX:XX:XX+2
macdsl  XX:XX:XX:XX:XX:XX+3
memsize 0x02000000
modetty0        38400,n,8,1,hw
modetty1        38400,n,8,1,hw
mtd0    0x90000000,0x90000000
mtd1    0x90010000,0x90780000
mtd2    0x90000000,0x90010000
mtd3    0x90780000,0x907C0000
mtd4    0x907C0000,0x90800000
mtd5    0x90780000,0x90780000
my_ipaddress    192.168.178.1
prompt  AVM_Ar7
ptest
reserved        00:04:0E:FF:FF:00
req_fullrate_freq       125000000
sysfrequency    125000000
urlader-version 1153
usb_board_mac   XX:XX:XX:XX:XX:XX
usb_rndis_mac   XX:XX:XX:XX:XX:XX+1
usb_device_id   0x0000
usb_revision_id 0x0000
usb_device_name USB DSL Device
usb_manufacturer_name   AVM
wlan_key        0000000000000000
wlan_cal        03FF,0006,0017,00F0,010A,0101,010A,02F7,035A
Jan  1 01:00:06 ar7cfgctl[268]: FactoryDefault=/etc/default/tcom/ar7.cfg (ar7)
Jan  1 01:00:06 ar7cfgctl[268]: load_config(ar7): factory default loaded
HWRevision='102'
HWRevision_ATA='1'
HWRevision_BitFileCount='1'
HWRevision_Reserved1='0'
ANNEX='B'
OEM='tcom'
OEM_DEFAULT_INDEX=''
OEM_tmp='tcom'
Language='de'
Country='049'
TZ='CET-1CEST,M3.5.0,M10.5.0'
CONFIG_AB_COUNT='2'
CONFIG_ANNEX='B'
CONFIG_ASSIST='y'
CONFIG_ATA='n'
CONFIG_ATA_FULL='n'
CONFIG_AUDIO='n'
CONFIG_AURA='n'
CONFIG_BLUETOOTH='n'
CONFIG_BLUETOOTH_CTP='n'
CONFIG_BUTTON='y'
CONFIG_CAPI='y'
CONFIG_CAPI_MIPS='n'
CONFIG_CAPI_NT='y'
CONFIG_CAPI_POTS='y'
CONFIG_CAPI_TE='y'
CONFIG_CAPI_UBIK='n'
CONFIG_CAPI_XILINX='y'
CONFIG_CDROM='n'
CONFIG_CDROM_FALLBACK='n'
CONFIG_DECT='y'
CONFIG_DSL='y'
CONFIG_ENVIRONMENT_PATH='/proc/sys/urlader'
CONFIG_ETH_COUNT='4'
CONFIG_FIRMWARE_URL='http://www.telekom.de/faq'
CONFIG_FON='y'
CONFIG_HOMEI2C='n'
CONFIG_HOSTNAME='speedport.ip'
CONFIG_I2C='n'
CONFIG_INSTALL_TYPE='ar7_8MB_xilinx_4eth_2ab_isdn_nt_te_pots_wlan_usb_host_dect_37264'
CONFIG_JFFS2='y'
CONFIG_LED_NO_DSL_LED='y'
CONFIG_MAILER='n'
CONFIG_MEDIACLI='n'
CONFIG_MEDIASRV='n'
CONFIG_NAND='n'
CONFIG_NFS='n'
CONFIG_OEM_DEFAULT='tcom'
CONFIG_PRODUKT='Fritz_Box_DECT_W900V'
CONFIG_PRODUKT_NAME='Speedport W 900V'
CONFIG_RAMSIZE='32'
CONFIG_ROMSIZE='8'
CONFIG_SERVICEPORTAL_URL='http://www.avm.de/fritzbox-service-portal.php?hardware=102&oem=tcom&language=de&country=&version=34.04.21&subversion='
CONFIG_STOREUSRCFG='y'
CONFIG_SUBVERSION=''
CONFIG_TAM='y'
CONFIG_TAM_MODE='1'
CONFIG_TR069='y'
CONFIG_UBIK2='n'
CONFIG_UPNP='n'
CONFIG_USB='n'
CONFIG_USB_HOST_AVM='y'
CONFIG_USB_HOST_TI='n'
CONFIG_USB_PRINT_SERV='y'
CONFIG_USB_STORAGE='y'
CONFIG_USB_WLAN_AUTH='y'
CONFIG_VDSL='n'
CONFIG_VERSION='04.21'
CONFIG_VERSION_MAJOR='34'
CONFIG_VLYNQ='y'
CONFIG_VLYNQ0='0'
CONFIG_VLYNQ1='0'
CONFIG_VPN='n'
CONFIG_WLAN='y'
CONFIG_WLAN_1130TNET='n'
CONFIG_WLAN_1350TNET='y'
CONFIG_WLAN_GREEN='n'
CONFIG_WLAN_WDS='y'
CONFIG_XILINX='y'
set 'Activate Wizzard'
mknod: /var/flash/ar7.cfg: File exists
cp /etc/default.049/fx_moh.default /var/fx_moh
checkempty: : No such file or directory
checkempty: warning, /var/flash/ar7.cfg not found - nop
checkempty: : No such file or directory
checkempty: warning, /var/flash/voip.cfg not found - nop
Piglet: module license '
(C) Copyright 2005 by AVM
' taints kernel.
[jffs2] xx=mtd6: 00200000 00010000 "jffs2"
[jffs2] i=mtd6:
[jffs2] jffs2_pat=6
[jffs2] i=00200000
[jffs2] jffs2_size=32
[jffs2] i=00010000
[jffs2] i="jffs2"
[jffs2] load jffs2 module
JFFS2 version 2.2. (NAND) (C) 2001-2003 Red Hat, Inc.
[jffs2] mount jffs
[jffs2] write env variable jffs2_size to 32
TAM: create JFFS2 directory /data/tam
attempting to load DSL Firmware '/lib/modules/microvoip-dsl.bin'
**** ANNEX: 'B'
*************************************
setting DSL Firmware to Annex B
registered device TI Avalanche SAR
tiatm driver (patch_annex=0xc00f69ec)
[tiatm] Set StrictPriority=0
DSP binary filesize = 300480 bytes
[setup_irq]: irq 23 irqaction->handler 0xc00de20c (tn7atm_sar_irq+0x0/0x30 [tiatm] )
[setup_irq]: irq 31 irqaction->handler 0xc00de23c (tn7atm_dsl_irq+0x0/0x28 [tiatm] )
[tiatm]: Powermanagment (States => 1,3,10) supported!
Texas Instruments ATM driver: version:[4.06.04.30]
ubik2 driver (ubik2 - 0x10=0xc00c8f54)
atm_dsp_register_ubik2: ubik2_ToMIPS_notify=0xc00bac00
atm_dsp_register_ubik2: dsp mem pointer 0xa1c0f1f0
ubik2_init_interface: DSP-Link Version v3 8480
isdn_fbox: Loading...
isdn_fbox: Driver 'isdn_fbox' attached to stack
isdn_fbox: CAPI driver registered.
isdn_fbox: AVM F!Box expected @ port 0x0000, irq 0
isdn_fbox: Loading...
gpio_ssi_init: done
isdn_fbox: Stack version 3.11-07
isdn_fbox: D-channel 0: DSS1
isdn_fbox: D-channel 1: DSS1
isdn_fbox: D-channel 2: DSS1_N
isdn_fbox: D-channel 3: POTS
isdn_fbox: D-channel 4: SIP
isdn_fbox: Loaded.
BLK: DECT StartUp, mode = WAIT, firmware: 00.13.12
BLK: DECT StartUp, mode = MASTER INIT, firmware: 00.13.12
usbcore: registered new driver usbfs
usbcore: registered new driver hub
AHCI RevisionID = 0x02, RamSize = 16384, NumPorts= 1
[setup_irq]: irq 9 irqaction->handler 0xc00543b0 (ahci_irq+0x0/0x8a0 [usbahcicore] )
ahci : new USB bus registered, assigned bus number 1
usb storage: nothing to do ....
2000-01-01 01:00:15 ar7cfgctl: FactoryDefault=/etc/default/tcom/ar7.cfg (ar7)
Jan  1 01:00:15 ar7cfgctl[432]: FactoryDefault=/etc/default/tcom/ar7.cfg (ar7)
2000-01-01 01:00:15 ar7cfgctl: load_config(ar7): factory default loaded
Jan  1 01:00:15 ar7cfgctl[432]: load_config(ar7): factory default loaded
kdsldmod: init start
kdsld: cache_create(datapipe)
kdsld: cache_create(datapipe_mod)
kdsld: cache_create(ipaccessset)
kdsld: cache_create(ipaccessrule)
kdsld: cache_create(ipfragid)
kdsld: cache_create(ipmasqentry)
kdsld: cache_create(ipmasqfwinfo)
kdsld: cache_create(ipmasqigdpm)
kdsld: cache_create(ipmasqappldata)
kdsld: cache_create(ipmasqmcgroup)
kdsld: cache_create(dnsmasqentry)
kdsld: cache_create(dnsstaticentry)
kdsld: cache_create(pingerentry)
kdsld: cache_create(pingerwaiter)
kdsld: cache_create(iprouteset)
kdsld: DATAPIPE: with header optimization
kdsldmod: init done
kdsld: PPP led: off (value=0)
hub 1-0:1.0: USB hub found
hub 1-0:1.0: 1 port detected
2000-01-01 01:00:18 cltmgr: csock: using poll
Jan  1 01:00:18 cltmgr[460]: csock: using poll
2000-01-01 01:00:20 cltmgr: avmssl_init: done
Jan  1 01:00:20 cltmgr[460]: avmssl_init: done
2000-01-01 01:00:20 cltmgr: process priority is 19
Jan  1 01:00:20 cltmgr[460]: process priority is 19
Jan  1 01:00:21 ar7cfgctl[560]: FactoryDefault=/etc/default/tcom/ar7.cfg (ar7)
Jan  1 01:00:21 ar7cfgctl[560]: load_config(ar7): factory default loaded
Jan  1 01:00:21 cltmgr[549]: FactoryDefault=/etc/default/tcom/ar7.cfg (ar7)
MAC WLAN:       00:04:0E:E4:D4:9E
Jan  1 01:00:22 cltmgr[549]: load_config(ar7): factory default loaded
Jan  1 01:00:23 cltmgr[549]: FactoryDefault=/etc/default/tcom/voip.cfg (voip)
Jan  1 01:00:23 cltmgr[549]: load_config(voip): factory default loaded
Jan  1 01:00:23 cltmgr[549]: TR-069 is activated in tr069.cfg
Jan  1 01:00:24 cltmgr[549]: mapping to info-LED already exist
Jan  1 01:00:24 cltmgr[549]: box init ok
Jan  1 01:00:24 cltmgr[549]: FactoryDefault=/etc/default/tcom/stat.cfg (stat)
Jan  1 01:00:24 cltmgr[549]: /etc/default/tcom/stat.cfg: is empty
Jan  1 01:00:24 cltmgr[549]: load_config(stat): file empty - factory default loaded
Jan  1 01:00:24 cltmgr[549]: Statistic load_config failed
Jan  1 01:00:24 cltmgr[549]: [speedup] disable
dlopen(/usr/share/ctlmgr/libvdsl.so) failed: File not found
Jan  1 01:00:24 cltmgr[549]: WAN (ata) led value = 0
Jan  1 01:00:24 cltmgr[549]: TR069_Init() TR069_Init(full)
Jan  1 01:00:24 cltmgr[549]: get_VoiceProfileNumberOfEntries() get_VoiceProfileNumberOfEntries 2 voipaccounts existing
Jan  1 01:00:24 cltmgr[549]: Register_WANConnectionDevice() adding 'connection_voip' to TR069Mapper_AddCallbacksForCtlmgrList
Jan  1 01:00:24 cltmgr[549]: Register_Portmapping() adding 'rule' to TR069Mapper_AddCallbacksForCtlmgrList
Jan  1 01:00:24 cltmgr[549]: Register_Layer3Forwarding() adding 'route' to TR069Mapper_AddCallbacksForCtlmgrList
Jan  1 01:00:24 cltmgr[549]: Register_VoiceProfile() adding 'sip' to TR069Mapper_AddCallbacksForCtlmgrList
Jan  1 01:00:24 cltmgr[549]: TR069_Full_Init() TR069_Full_Init() ret=0
Jan  1 01:00:24 cltmgr[549]: TR069_Init returned 0
Jan  1 01:00:24 cltmgr[549]: TR069_Init(full mode) returned 0
Jan  1 01:00:24 cltmgr[549]: TR069_SetSessionNotifier ret=0
Jan  1 01:00:24 cltmgr[549]: verbose: DISABLED
WSTART (Jul  6 2006 15:38:07)wstart: config ok(1)
wpa auth:waiting for driver to come up...
Wstart - made configure_wpa_authenticator
[Wstart] wlan_cal ist gesetzt: NVS DATEI WIRD MIT KALIBRIERUNGSWERTEN GEPATCHT!!!
 [Wstart] 9 Parameter werden gepatcht:
[Wstart] NVS DATEI WURDE GEPATCHT.
NVS File loaded.
429493997: Configuration succeeded !!!
[ohio_vlynq_init] device 0
[ohio_vlynq_startup_link]
[setup_irq]: irq 29 irqaction->handler 0x94004f2c (vlynq_interrupt+0x0/0x34 )
[setup_irq]: irq 79 irqaction->handler 0xc04d0a10 (whal_acxIntrHandler+0x0/0x1e8 [tiap] )
429494004:
Jan  1 01:00:28 cltmgr[549]: 0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.1:53 - Network is unreachable (128)
Jan  1 01:00:28 cltmgr[549]: 0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.2:53 - Network is unreachable (128)
Jan  1 01:00:29 cltmgr[549]: 0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.1:53 - Network is unreachable (128)
Jan  1 01:00:29 cltmgr[549]: 0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.2:53 - Network is unreachable (128)
429494027: WDRV_MAINSM: WLAN Driver initialized successfully
429494027: FW Watchdog is Enabled
dda: tiwlan0 in initializing Succeeded wireless extensions: ret = 0
wlan started (OK)
Wstart - made configure_and_start_ap_driver
WSTART: done(0)
WLAN is enabled
tiwlan0 device is activated
wdsup0 device is activated
Jan  1 01:00:31 cltmgr[549]: wdsdw0 device is activated
0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.1:53 - Network is unreachable (128)
Jan  1 01:00:31 cwdsdw1 device is activated
ltmgr[549]: 0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.2:53 - Network is unreachable (128)
wdsdw2 device is activated
wdsdw3 device is activated
/etc/init.d/rc.net: /etc/init.d/rc.net: 366: igdd: not found
Jan  1 01:00:32 websrv[630]: csock: using poll
Jan  1 01:00:32 cltmgr[549]: 0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.1:53 - Network is unreachable (128)
Jan  1 01:00:32 cltmgr[549]: 0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.2:53 - Network is unreachable (128)
2000-01-01 01:00:33 multid: FactoryDefault=/etc/default/tcom/ar7.cfg (ar7)
Jan  1 01:00:33 multid[636]: FactoryDefault=/etc/default/tcom/ar7.cfg (ar7)
Jan  1 01:00:33 cltmgr[549]: 0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.1:53 - Network is unreachable (128)
Jan  1 01:00:33 cltmgr[549]: 0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.2:53 - Network is unreachable (128)
2000-01-01 01:00:33 multid: load_config(ar7): factory default loaded
Jan  1 01:00:33 multid[636]: load_config(ar7): factory default loaded
2000-01-01 01:00:33 multid: startup (Aug 31 2006 17:10:44)
Jan  1 01:00:33 multid[636]: startup (Aug 31 2006 17:10:44)
2000-01-01 01:00:33 multid: csock: using poll
Jan  1 01:00:33 multid[636]: csock: using poll
2000-01-01 01:00:34 multid: avmssl_init: done
Jan  1 01:00:34 multid[636]: avmssl_init: done
Jan  1 01:00:34 multid[638]: new cpmac driver detected
Jan  1 01:00:34 websrv[630]: avmssl_init: done
Jan  1 01:00:[setup_irq]: irq 27 irqaction->handler 0x940e016c (34 mcpmac_main_isr+0x0/0x78 ult)
id[638]: enabling non-ATA-Mode
Jan  1 01:00:34 multid[638]: normal
Jan  1 01:00:34 multid[638]: if_setup: lan:0: SIOCSIFADDR failed - No such device (19)
cpmac_main_ioctl, unknown ioctl 35142
device eth0 entered promiscuous mode
device cpmac0 entered promiscuous mode
lan: port 1(eth0) entering learning state
Jan  1 01:00:34 multid[638]:tiwlan_ddaDoIoctl : Unknown ioctl 35142
 if_device tiwlan0 entered promiscuous mode
setlan: port 2(tiwlan0) entering learning state
up: usbrndis: SIOCSIFADDR failed - No such device (19)
Jan  1 01:00:34 multid[638]: br_add_if: get index for usbrndis failed - No such device (19)
Jan  1 01:00:34 multid[638]: if_allmulti: usbrndis: SIOCGIFFLAGS failed - No such device (19)
Jan  1 01:00:34 cltmgr[549]: 0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.1:53 - Network is unreachable (128)
tiwlan_ddaWdsDoIoctl : Unknown ioctl 35142
device wdsup0 entered promiscuous mode
lan: port 3(wdsup0) entering learning state
tiwlan_ddaWdsDoIoctl : Unknown ioctl 35142
device wdsdw0 entered promiscuous mode
lan: port 4(wdsdw0) entering learning state
tiwlan_ddaWdsDoIoctl : Unknown ioctl 35142
device wdsdw1 entered promiscuous mode
lan: port 5(wdsdw1) entering learning state
Jan  1 01:00:35 cltmgr[549]: 0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.2:53 - Network is unreachable (128)
tiwlan_ddaWdsDoIoctl : Unknown ioctl 35142
device wdsdw2 entered promiscuous mode
lan: port 6(wdsdw2) entering learning state
tiwlan_ddaWdsDoIoctl : Unknown ioctl 35142
device wdsdw3 entered promiscuous mode
lan: port 7(wdsdw3) entering learning state
Jan  1 01:00:35 multid[638]: if_setup: eth1: SIOCSIFADDR failed - No such device (19)
Jan  1 01:00:35 multid[638]: br_add_if: get index for eth1 failed - No such device (19)
Jan  1 01:00:35 multid[638]: if_allmulti: eth1: SIOCGIFFLAGS failed - No such device (19)
Jan  1 01:00:35 multid[638]: if_setup: eth2: SIOCSIFADDR failed - No such device (19)
Jan  1 01:00:35 multid[638]: br_add_if: get index for eth2 failed - No such device (19)
Jan  1 01:00:35 multid[638]: if_allmulti: eth2: SIOCGIFFLAGS failed - No such device (19)
Jan  1 01:00:35 multid[638]: if_setup: eth3: SIOCSIFADDR failed - No such device (19)
Jan  1 01:00:35 multid[638]: br_add_if: get index for eth3 failed - No such device (19)
Jan  1 01:00:35 websrv[630]: FactoryDefault=/etc/default/tcom/ar7.cfg (ar7)
Jan  1 01:00:35 multid[638]: if_allmulti: eth3: SIOCGIFFLAGS failed - No such device (19)
Jan  1 01:00:35 multid[638]: static routes: 0 deleted (0 failed), 0 added (0 failed)
Jan  1 01:00:35 multid[638]: static routes: 0 deleted (0 failed), 0 added (0 failed)
Jan  1 01:00:35 multid[638]: IGMP tos configured to 0x80
Jan  1 01:00:35 multid[638]: mrouter: using IGMPv3 for upstream interface dsl
Jan  1 01:00:35 multid[638]: mrouter: using IGMPv3 for other interfaces
2000-01-01 01:00:35 dsld: csock: using poll
Jan  1 01:00:35 dsld[649]: csock: using poll
Jan  1 01:00:35 multid[638]: DHCPD on lan
Jan  1 01:00:35 multid[638]: DHCPD on lan:0 skipped, is virtual interface
Jan  1 01:00:35 multid[638]: dhcpd: can't open /var/flash/multid.leases - No such file or directory (2)
Jan  1 01:00:35 multid[638]: DDNS: no valid accounts
Jan  1 01:00:35 multid[638]: ONLINE: script /bin/onlinechanged not found.
Jan  1 01:00:35 multid[638]: verbose: DISABLED
Jan  1 01:00:35 cltmgr[549]: 0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.1:53 - Network is unreachable (128)
Jan  1 01:00:35 cltmgr[549]: 0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.2:53 - Network is unreachable (128)
2000-01-01 01:00:35 dsld: avmssl_init: done
Jan  1 01:00:35 dsld[649]: avmssl_init: done
2000-01-01 01:00:35 dsld: FactoryDefault=/etc/default/tcom/ar7.cfg (ar7)
Jan  1 01:00:35 dsld[649]: FactoryDefault=/etc/default/tcom/ar7.cfg (ar7)
2000-01-01 01:00:35 dsld: load_config(ar7): factory default loaded
Jan  1 01:00:36 dsld[649]: load_config(ar7): factory default loaded
2000-01-01 01:00:36 dsld: startup (Jul  6 2006 15:37:07)
Jan  1 01:00:36 dsld[649]: startup (Jul  6 2006 15:37:07)
2000-01-01 01:00:36 dsld: new cpmac driver detected
Jan  1 01:00:36 dsld[649]: new cpmac driver detected
Jan  1 01:00:36 websrv[630]: load_config(ar7): factory default loaded
/etc/init.d/rc.net: /etc/init.d/rc.net: 1: avmike: not found
Jan  1 01:00:36 dsld[651]: DSL Mac xx:xx:xx:xx:xx:xx
Jan  1 01:00:36 dsld[651]: VOIP Mac xx:xx:xx:xx:xx:xx
Jan  1 01:00:36 dsld[651]: voip: ppptarget voip disabled, ignored Jan  1 01:00:36 dsld[651]: FactoryDefault=/etc/default/tcom/stat.cfgkdsld: sync lost  (stat) Jan  1 01:00:36 dsld[651]: /etc/default/tcom/stat.cfg: is empty Jan  1 01:00:36 dsld[651]: load_config(stat): file empty - factory default loaded Jan  1 01:00:36 dsld[651]: Statistic load_config failed Jan  1 01:00:36 dsld[651]: verbose: DISABLED Jan  1 01:00:36 cltmgr[549]: 0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.1:53 - Network is unreachable (128) Jan  1 01:00:36 cltmgr[549]: 0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.2:53 - Network is unreachable (128) [avm_led] virt led not registered [avm_led] format error: "SET Name,Instanz = state" telefon: use clock_gettime(CLOCK_MONOTONIC)! BLK: DECT StartUp, mode = NORMAL, firmware: 00.13.12 telefon: SIGCHLD received! telefon: WARNING No config file '/var/flash/fx_def' ! telefon: WARNING No CG file '/var/flash/fx_cg' ! voipd: csock: using poll Jan  1 01:00:40 websrv[630]: startup (Jul  6 2006 15:36:02) voipd: avmssl_init: done 2000-01-01 01:00:40 ar7cfgctl: FactoryDefault=/etc/default/tcom/ar7.cfg (ar7) Jan  1 01:00:40 ar7cfgctl[677]: FactoryDefault=/etc/default/tcom/ar7.cfg (ar7) Jan  1 01:00:40 voipd[673]: FactoryDefault=/etc/default/tcom/voip.cfg (voip) Jan  1 01:00:40 voipd[673]: load_config(voip): factory default loaded Jan  1 01:00:40 voipd[673]: startup (AVM Speedport W 900V 34.04.21 AVM SIP v5.01.48 Jul 12 2006 16:18:47) Jan  1 01:00:40 voipd[673]: using capi controller 5 Jan  1 01:00:40 voipd[673]: using b1 protocol 10 2000-01-01 01:00:41 ar7cfgctl: load_config(ar7): factory default loaded Jan  1 01:00:41 ar7cfgctl[677]: load_config(ar7): factory default loaded Jan  1 01:00:41 voipd[673]: tel: supported Jan  1 01:00:41 voipd[673]: ENUM NOT enabled Jan  1 01:00:41 voipd[673]: enumdomain: e164.arpa Jan  1 01:00:41 voipd[673]: enumdomain: e164.org Jan  1 01:00:41 voipd[673]: VoIP led value = 0 Jan  1 01:00:41 voipd[673]: no useragent configured Jan  1 01:00:41 voipd[673]: INFO led: off (value=0) Jan  1 01:00:41 voipd[673]: priority is -20 Jan  1 01:00:41 voipd[673]: encaplen 32 Jan  1 01:00:41 voipd[673]: brutto speed unknown Jan  1 01:00:41 voipd[673]: connstatus 0 -> 1 Jan  1 01:00:41 voipd[673]: PCMA: 98933 bits/second (encaplen=32,30ms) Jan  1 01:00:41 voipd[673]: PCMU: 98933 bits/second (encaplen=32,30ms) Jan  1 01:00:41 voipd[673]: G726-32: 70666 bits/second (encaplen=32,30ms) Jan  1 01:00:41 voipd[673]: G726-40: 70666 bits/second (encaplen=32,30ms) Jan  1 01:00:41 voipd[673]: G726-24: 56533 bits/second (encaplen=32,30ms) Jan  1 01:00:41 voipd[673]: iLBC: 42400 bits/second (encaplen=32,30ms) Jan  1 01:00:41 voipd[673]: Speex: 63600 bits/second (encaplen=32,30ms) Jan  1 01:00:41 voipd[673]: PCMA: 106000 bits/second (encaplen=32,20ms) Jan  1 01:00:41 voipd[673]: PCMU: 106000 bits/second (encaplen=32,20ms) Jan  1 01:00:41 voipd[673]: G726-32: 84800 bits/second (encaplen=32,20ms) Jan  1 01:00:41 voipd[673]: G726-40: 84800 bits/second (encaplen=32,20ms) Jan  1 01:00:41 voipd[673]: G726-24: 63600 bits/second (encaplen=32,20ms) Jan  1 01:00:41 voipd[673]: iLBC: 63600 bits/second (encaplen=32,20msAR7WDT: System Init UEberwachung abgeschlossen (201510 ms noch verfuegbar) ) Jan  1 01:00:41 voipd[673]: Speex: 63600 bits/second (encaplen=32,20ms) Jan  1 01:00:41 voipd[673]: verbose: DISABLED run_clock demon started  Jan  1 01:00:41 cltmgr[549]: 0.0.0.0:2048SysRq : Changing Loglevel Loglevel set to 4 : failed to send UDP-  Please press Enter to activate this console.   Jan  1 01:00:41 cltmgr[549]: 0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.2:53 - Network is unreachable (128) Jan  1 01:00:42 cltmgr[549]: 0.0.0.0:2048: failed to send UDP-datagram to 192.168.180.1:53 - Network is unreachable (128) }}}

== Openwrt running on Speedport W900v ==
{{{
BusyBox v1.4.2 (2007-06-24 15:34:13 CEST) Built-in shell (ash)
Enter 'help' for a list of built-in commands.

  _______                     ________        __
 |       |.-----.-----.-----.|  |  |  |.----.|  |_
 |   -   ||  _  |  -__|     ||  |  |  ||   _||   _|
 |_______||   __|_____|__|__||________||__|  |____|
          |__| W I R E L E S S   F R E E D O M
 KAMIKAZE (bleeding edge, r7718) -------------------
  * 10 oz Vodka       Shake well with ice and strain
  * 10 oz Triple sec  mixture into 10 shot glasses.
  * 10 oz lime juice  Salute!
 ---------------------------------------------------
root@OpenWrt:/#
root@OpenWrt:/#
}}}

CategoryModel ["CategoryAR7Device"]
