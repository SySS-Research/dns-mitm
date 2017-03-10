dns-mitm.py
===========

This is a fake DNS server that answers requests for a domain's A record with
a custom IP address. It is intended to be used in an isolated network for
pentests. You could also use `dnsmasq` for that, but sometimes you just want
to use a small script.

If you want to put yourself in a MitM position of a given connection, you
would usually do so by modifying the network setup: either in software
(ARP-spoofing etc.) or in hardware (unplugging network cables). This script
is for situations in which you are unable or unwilling to change the network
setup but have control over the "victim" device.

Possible use cases could be:
 * You want to analyze traffic of a mobile app you are testing, so you
 change the DNS server on your mobile device
 * You want to filter ads on your TV, so you set its DNS server to your
 Raspberry Pi on the same network which is running this script

It makes sense to assign multiple IP addresses to your device, for example
with `ip address add 192.168.1.16/28 dev eth0`.

This way you can spoof multiple domains with an indiviual IP address each.
Otherwise, you won't know the original destination of the intercepted
traffic arriving at your machine without deep package inspection.

Usage
-----

You can specify IP addresses on the command line or in a separate hosts
file, e.g. to answer all requests to `.*.example.com` to `192.168.1.42`:

    $ ./dns-mitm.py .*.example.com,192.168.1.42

or

    $ ./dns-mitm.py -f hosts.dat

where `hosts.dat` uses the same syntax as `/etc/hosts`.

By default, the script tries to determine the DNS server that the system is
using. You may want to specify a different DNS server with the `-d` option.

For more information, type `./dns-mitm.py -h`.
