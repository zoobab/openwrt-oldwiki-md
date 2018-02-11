'''Hotspot with !OpenWrt + Private VPN access'''

\[\[TableOfContents\]\]

Updated 20. September 2005

Jan Beba <mail@janbeba.de> \[\[BR\]\] Bjoern Biesenbach
<bjoern@bjoern-b.de>

20. May 2005

PDF-Version: <http://bjoern-b.de/files/HotspotOpenvpn.pdf> \[\[BR\]\]
Experimental snapshot 23.4.05 including required packages:
<http://bjoern-b.de/files/openwrt-wrt54g-jffs2.bin>

= Intro =

Today many people have a broadband Internet connection and surely don't
use the whole bandwidth all the time. So why don't give others the
opportunity to use your connection? With this document we want to
describe how to set up a hotspot using an accesspoint running with
!OpenWrt. A very important aspect when you decide to open your wireless
network for everyone often is, that you still want to use it for your
own purpose. This might be accessing a local file- or printserver or
anything else not everybody in front of your house should be able to see
and to use. Also your own connection should be encrypted. WEP encryption
is not only quite insecure but would also conflict with the idea of an
open hotspot. So we decided to create a VPN using OpenVPN.

(Note that this setup allows a single client to get access to the
network behind the VPN, using a static key...)

= Network =

The structure of our network is quite easy. We will use three separated
networks; the first will be our own private network, the second the
public wireless lan and the last our VPN.

> LAN: 192.168.1.0/24
>
> WLAN: 192.168.2.0/24
>
> VPN: 192.168.3.0/24

= What you need =

To use this howto I recommend you to use the newest White Russian RC3.
You can build your own firmware or use the generic images in
\[<http://downloads.openwrt.org/whiterussian/rc3/>\]

To use openvpn you have to install the following packages:

> -   openssl
> -   lzo
> -   kmod-tun
> -   openvpn

I will install them later with ipkg.

= OpenWrt =

Our configuration has been tested with the Linksys WRT54G versions 2.0
and 2.2. If you use other hardware please mind that the interface names
may be changed. Assuming your !OpenWrt installation is untouched your
box is reachable via telnet on {{{192.168.1.1}}}. The first thing to do
is to set a password. Log into your box, type {{{passwd}}} and set your
new root password. After doing so disconnect and reconnect via SSH.

== Network devices ==

The default config is a little tricky. The LAN device (vlan0) and the
WLAN device (eth1) are bridged together to "br0". But as we want to have
separated nets for those devices, we have to split them. Also the
Internet (WAN) device has to be configured.

'''Note:''' that the following commands are examples! You have to adapt
them to your box. For example on some Wrt units you have substitute
{{{wifi\_ifname}}} with {{{wl0\_ifname}}} and so on.

{{{ nvram set lan\_ifname=vlan0 nvram set lan\_proto=static nvram set
lan\_ipaddr=192.168.1.1 nvram set lan\_netmask=255.255.255.0

nvram set wifi\_ifname=eth1 nvram set wifi\_proto=static nvram set
wifi\_ipaddr=192.168.2.1 nvram set wifi\_netmask=255.255.255.0

nvram set wan\_ifname=ppp0 nvram set wan\_proto=pppoe nvram set
wan\_mtu=1492

nvram set pppoe\_ifname=vlan1 nvram set pppoe\_username=user at
provider.name nvram set pppoe\_passwd=yourpassword

nvram set wl0\_ssid=Hotspot nvram commit

reboot }}}

The box will restart and *hopefully* come up again. If your WLAN
interface (eth1) is not reachable, make sure it is ''up''.

On my box I had to add the following lines to /etc/firewall.user so that
the WLAN interface could communicate with the DHCP server and pass
traffic out to the WAN.

{{{ WIFI="\$(nvram get wifi\_ifname)"

iptables -N WIFI\_ACCEPT \[ -z "\$WAN" \] | iptables -A WIFI\_ACCEPT -i
"\$WANDEV" -j RETURN iptables -A WIFI\_ACCEPT -j ACCEPT

iptables -A INPUT -j WIFI\_ACCEPT

iptables -A FORWARD -i br1 -o br1 -j ACCEPT iptables -A FORWARD -i
\$WIFI -o \$WAN -j ACCEPT }}}

== DHCP-Server ==

The dnsmasq package in !OpenWrt is responsible for the DHCPD functions.
As we have a local LAN and a public WLAN we want to serve both with
dynamically IP address allocation. IP addresses in the range between
{{{192.168.1.200-192.168.1.250}}} and {{{192.168.2.200-192.168.2.250}}}
are being offered.

'''/etc/dnsmasq.conf'''

{{{ domain-needed bogus-priv filterwin2k local=/lan/ domain=lan

except-interface=vlan1

dhcp-range=vlan0,192.168.1.200,192.168.1.250,255.255.255.0,3h
dhcp-range=eth1,192.168.2.200,192.168.2.250,255.255.255.0,3h

dhcp-leasefile=/tmp/dhcp.leases

dhcp-option=vlan0,3,192.168.1.1 dhcp-option=vlan0,6,192.168.1.1
dhcp-option=eth1,3,192.168.2.1 dhcp-option=eth1,6,192.168.2.1 }}}

== OpenVPN ==

First we should install the required software.

{{{ ipkg install openvpn }}}

Let's create the directory and a private key for our VPN.

{{{ mkdir /etc/openvpn openvpn --genkey --secret
/etc/openvpn/wlan\_home.key }}}

Load the tunneling module and add it to the autoloader.

{{{ insmod tun echo "tun" » /etc/modules }}}

'''/etc/openvpn/wlan\_home.conf'''

{{{ dev tun0 ifconfig 192.168.3.1 192.168.3.2 secret
/etc/openvpn/wlan\_home.key port 1194 ping 15 ping-restart 45
ping-timer-rem persist-key persist-tun verb 3 }}}

'''/etc/init.d/S60openvpn'''

{{{ \#!/bin/sh openvpn --daemon --config /etc/openvpn/wlan\_home.conf
}}}

Don't forget to assign executable rights to this file.

{{{ chmod a+x /etc/init.d/S60openvpn }}}

== Iptables setup ==

'''/etc/firewall.user'''

{{{ \[...\] iptables -A FORWARD -i eth1 -o ppp0 -j ACCEPT iptables -A
FORWARD -i tun0 -j ACCEPT iptables -A FORWARD -i vlan0 -o tun0 -j ACCEPT
}}}

This has to be appended! The whole file is much longer.

'''Finally you can do a last reboot.'''

If you can only talk to vlan1, you may find you need to change the
second line to:

{{{ iptables -A FORWARD -i tun0 -o vlan0 -j ACCEPT iptables -A FORWARD
-i tun0 -o vlan1 -j ACCEPT }}}

= Clientside =

Now if you want to access the internet from either your local network or
via wifi you just have to select DHCP for your network device. To access
your local network from out the wifi, the OpenVPN client has to be
installed. OpenVPN Install the fitting OpenVPN client for your operating
system. Copy the {{{/etc/openvpn/wlan\_home.key}}} file from the Wrt to
your client. We prefer using SCP.

{{{ scp 192.168.1.1:/etc/openvpn/wlan\_home.key /etc/openvpn/ }}}

If you're using MS Windows copy the file to {{{C:Program
FilesOpenVPNconfig}}}.

Now create the config file.

'''/etc/openvpn/wlan\_home.conf''' or '''C:Program
FilesOpenVPNconfigwlan\_home.conf'''

{{{ dev tun remote 192.168.2.1 ifconfig 192.168.3.2 192.168.3.1 secret
wlan\_home.key port 1194 route-gateway 192.168.3.1 route 0.0.0.0 0.0.0.0
redirect-gateway

ping 15 ping-restart 45 ping-timer-rem persist-tun persist-key

verb 3 }}}

Using '''Linux''' you have to load the tunnel module.

{{{ modprobe tun }}}

Now you can start the tunnel using

{{{ openvpn --daemon --config /etc/openvpn/wlan\_home.conf }}}

For '''Windows''' just right-click onto your config and choose the
second point to execute the config.

If you use '''MacOSX''' you should use something like
\[<http://www.tunnelblick.net> Tunnelblick\] which is OpenVPN with a
GUI. Don't use its default configuration, use the above config and add
the lines:

{{{ user nobody group nobody }}}

(These might also be useful in your OpenVPN server config and linux
client config).