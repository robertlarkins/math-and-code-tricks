# Pi-hole
https://pi-hole.net/

## Setup

### Upstream DNS Provider
Unsure of the best one to use, this site provides some description https://www.techradar.com/nz/news/best-dns-server
I have gone with OpenDNS.

### IPv6
If your router used IPv6, then the pi-hole needs to be set to allow this.


If a new router is installed, or has a name or ip address change, then the default route on the Pi-hole may need to be updated.
This can be done 

### Repair or Reset
```
pihole -r
```

It could be that the /etc/dhcpcd.conf file has outdated ips in it, this will likely look something like this:
```
interface eth0
    static ip_address=192.168.0.4/24
    static routers=192.168.0.254
    static domain_name_servers=192.168.0.254 8.8.8.8
```
To fix the dhcpcd.conf delete these lines, then run `pihole -r` and choose *Reconfigure*.

## Update
Log into the raspberrypi console and run the command:

```console
pihole -up
```

## Pi-hole Admin Console

The following are possible urls for accessing the Pi-hole Admin Console:
 - [ip6]/admin
 - 
