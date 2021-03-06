'''Samba HowTo'''

\[\[TableOfContents\]\] = Intro = This shows you how to give all users
on your network a shared drive where they can read and write files. We
use Samba, the Open Source implementation of Windows networking
(NetBIOS/SMB).

Samba 2.0.x was packaged for !OpenWrt, because the newer versions are
overkill for the CPU and memory available on a typical device.

Samba offers a lot more options but this would be out of the scope. For
more information on Samba see the links at the button.

= Requirements =

:   -   a device supported by !OpenWrt
    -   a recent !OpenWrt version installed (at least White Russian RC3)
    -   some kind of a storage device (optional)

= Installation = Configure your device to use the backports repository,
see \["OpenWrtDocs/Packages"\] for instructions, then install the
package: {{{ ipkg install samba-server}}}

'''TIP:''' A Samba client package is available too: {{{ ipkg install
samba-client}}}

= Configuration = The default Samba configuration uses the {{{/tmp}}}
directory for storage.

/!'''WARNING:''' All data is lost in {{{/tmp}}} after a reboot. See
LocalFileSystemHowTo for alternative storage options.

To change settings edit the {{{/etc/samba/smb.conf}}} file.

Start Samba with

{{{ /etc/init.d/samba start}}}

== localhost ==

If samba does not start, try adding your router's name and ip in
/etc/hosts. (see also <http://forum.openwrt.org/viewtopic.php?id=5401>)

= Clients = '''NOTE:''' In the default config file all users are only
able to read on the share, since all users use the {{{nobody}}} account.
You can change this in the {{{/etc/samba/smb.conf}}} file.

== Linux clients == List shares with {{{smbclient}}}:

{{{ smbclient -L &lt;server\_name&gt; -N}}}

Connect to a Samba share with:

{{{ smbclient //&lt;server\_name&gt;/&lt;share&gt; -U
&lt;username&gt;}}}

Connect with {{{smbfs}}}:

{{{ smbmount //&lt;server\_name&gt;/&lt;share&gt;
/&lt;local\_mount\_point&gt; -o username=&lt;username&gt;}}}

== Windows clients == Now you should be able to browse your share called
{{{tmp}}} from any computer that is a member of {{{OpenWrt}}} workgroup
(the default workgroup name in Windows is {{{WORKGROUP}}}).

List shares with {{{net}}}:

{{{ net view \\&lt;server\_name&gt;}}}

Connect to a Samba share with:

{{{ net use \* \\&lt;server\_name&gt;&lt;share&gt;
/user:&lt;username&gt;}}}

== MacOS X clients == In the Finder, press Command-K for the mount
popup. In the Server Address text box, type:

{{{ <smb://>&lt;server\_ip&gt;/&lt;share&gt;}}}

You may press the "+" sign to save this in your list. Press "Browse" to
see all shares on a server, or "Connect" to mount the share. You'll see
the icon appear on the desktop, and the share will be mounted in
/Volumes.

= Problems =

If you get the following error message while starting samba: {{{ nmbd:
can't load library 'libgcc\_s.so.1' smbd: can't load library
'libgcc\_s.so.1'}}}

Just create a softlink for the library: {{{ ln -s /lib/libc.so.0
/lib/libgcc\_s.so.1}}}

or, much better install the right package (\[28-01-2007 - m4rc0\]
libgcc\_3.4.4-8\_mipsel.ipk now included in RC6.): {{{ ipkg install
libgcc}}}

I experience problems trying to mount samba shares using mount.cifs from
other openwrt machines or from ubuntu. This does not happen with my
samba 3.0.22 shares from my ubuntu main machine. Be aware that there may
be some issue in accessing Openwrt samba shares using cifs. by johncass

With the CIFS code of linux-2.6.23.8 I don't have any file access at
all. You can browse the directories, but file access results in
following errors in the Samba daemon:{{{ write access: smbd\[4177\]:
map\_share\_mode: Incorrect value 40000000 for desired\_access to file
xxxxx read access: smbd\[4177\]: map\_share\_mode: Incorrect value
80000000 for desired\_access to file xxxxx}}} SOLUTION: Enable
"SMBFS-Support" in linux kernel. Do not use Samba in a version higher
than 3.0.26, the Samba people break compatibility to Samba 2.x in these
versions intentionally because they want to support only CIFS in the
future.

If samba runs only a few seconds (check by 'ps') and quits within a few
seconds, take a look at
\[<http://forum.openwrt.org/viewtopic.php?id=8100> this\] thread, and
add OpenWRT to /etc/hosts. The problem seems to be that Samba looks for
a server called OpenWRT, which does not exists if you named your device
differently.

= Links =

:   -   \[<http://www.samba.org/> Samba\]
    -   UsbStorageHowto
    -   IdeStorageHowTo
    -   LocalFileSystemHowTo - configuring local storage,
    -   Parts of this documentation are taken from \[\[BR\]\]-
        <http://www.macsat.com/OpenWRT/samba.php>


