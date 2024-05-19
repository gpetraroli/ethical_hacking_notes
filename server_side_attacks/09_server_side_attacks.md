# `Server side attacks`

The first attempt we can perform is scanning the the target IP for open ports and exploitable vulnerabilities.<br>
Once we found a known vulnerability we can google it and see if it is exploitable; a good resource is the Rapid7 website (owner of metasploit framework).<br>
We can then use the metasploit framework to try to exploit the vulnerability.<br>
Here the main metasploit's commands:<br><br>
```$ msconsole``` runs the metasploit console<br>
```$ help``` shows help<br>
```$ show [something]``` something can be exploited, payloads, auxiliaries or options<br>
```$ use [something]``` use a certain exploit, payload or auxiliary
```$ set [option][value]``` configure [option] to have a value of [value]
```$ expoit``` runs the current task<br>

## Example :
