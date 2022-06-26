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

## Repair or Reset
```console
pihole -r
```

## Diagnosis
```console
pihole -d
```

### Cannot access internet
If the Pi-hole is unable to access the internet, it could be that the gateway has changed, or there has been a change from IPv4 to IPv6.


One possible cause is that the /etc/dhcpcd.conf file has outdated ips in it, if so it will have some values like this:
```
interface eth0
    static ip_address=192.168.0.4/24
    static routers=192.168.0.254
    static domain_name_servers=192.168.0.254 8.8.8.8
```
To fix the dhcpcd.conf delete these lines, then reboot the Pi-hole by running the command
```console
sudo reboot
```

the file `/etc/pihole/setupVars.conf` may need to be updated to match as well.
So use `sudo nano /etc/pihole/setupVars.conf` to access it and change the IPv4 and IPv6 addresses to match the ips assigned to the device.
These can be found by looking at your routers configuration and seeing what ips it gave to the Pi-hole (whatever its hostname is called).

## Update
Log into the raspberrypi console and run the command:

```console
pihole -up
```

## Pi-hole Admin Console

The following are possible urls for accessing the Pi-hole Admin Console:
 - [ip6]/admin

If the admin console is not working, it may need to be restarted by running
```console
sudo service lighttpd restart
```

## Router Setup

### DNS
The router may require two DNS servers, you can just repeat the ip for Pi-hole twice. This will ensure that all DNS requests go through the Pi-hole,
but if Pi-hole goes down, then DNS requests will not be resolved. As long as you are aware that this might happen, set both DNS servers to the Pi-hole.
