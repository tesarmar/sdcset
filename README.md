# SDCset
A command line utility used to set and control Laird Summit 802.11 supplicant

## Download
https://github.com/tesarmar/SDCset/releases

## Compatibility
### SSD30AG
 - Datalogic Elf
 - Topsystem Voxter Elite

### SSD40NBT
 - Datalogic Falcon X3+

## Usage
 - Copy the application executable to some persistent directory in the device memory
 - Create batch file including the desired command line parameters (see example below)
 - Place the batch file to the Windows startup directory
```batch
@echo off

rem Set application executable location
set CMD=\somelocation\SDCset.exe

rem Switch off the WLAN NIC during configuration
start /wait "%CMD%" -radio off

rem Set global settings
start /wait "%CMD%" -global -roam_trig 65 -roam_delta 5 -roam_per 5 -dfs_chan on -aggressive on -ccx off -wmm on -txdiver on -rxdiver startmain -tray on -disppwd off -adminrequire on -adminpwd "adminpass" -opmk on

rem Create/modify wireless profile
start /wait "%CMD%" -config -add someprofile -ssid SOMESSID -client "someuser" -eap peapmschap -encrypt wpa2aes -eapuser "someuser" -eappassword "somepass" -eapvalidateserver off -eapusemsstore off -rfmode "bgna" -brate auto -pwr cam

rem Set power settings
start /wait "%CMD%" -power cam

rem Activate the newly created/configured profile and reactivate WLAN NIC
start /wait "%CMD%" -config -active someprofile
start /wait "%CMD%" -radio on

rem Remove default profile (active profile cannot be deleted)
start /wait "%CMD%" -config -delete Default
```
## Command line options
### ``-?``
Display the copyright and version information for SDCset.
### ``-ip``
Displays the current status of the RF card, the terminal IP and MAC information, and the associated access point IP and MAC information if any.
### ``-radio [on|off]``
Enables or disables the radio.
### ``-power [cam|fast|max]``
Set the currently active configuration to the specified power mode.  

### ``-config [-active|-delete] <profile  name>``

 - ``-active`` – set the currently active wireless profile to the given profile
 - ``-delete`` – delete the given profile as long as it is not the currently active profile

----------

### ``-add <profile name>``
Add or modify the given profile using the options below.  Note that adding a profile will not automatically set it to be the currently active profile.
#### ``-ssid <SSID>``
Service set identifier (SSID) for WLAN to which radio will connect
#### ``-client  <client name>``
Name assigned to the Summit radio and client device that uses it

#### ``-username <user name>``
#### ``-userpwd <user password>``

#### ``-rfmode [b_only|bg|g_only|bg_subset|a_only|abg|bga|gn|an|abgn|bgna|adhoc]``

Use of Radio modes when interacting with AP or another peer:

 - ``b_only`` – 1, 2, 5.5 and 11 Mbps; 2.4GHz band only
 - ``bg`` – all 802.11b and 802.11g rates; 2.4GHz band only (Default for 802.11bg radios)
 - ``g_only`` – 6, 9, 12, 18, 24, 36, 48 and 54 Mbps; 2.4GHz band only
 - ``bg_subset`` – 1, 2, 5.5, 6, 11, 24, 36 and 54 Mbps; 2.4GHz band only
 - ``a_only`` – 6, 9, 12, 18, 24, 36, 48 and 54 Mbps; 5GHz band only
 - ``abg`` – all 802.11b and 802.11a/g rates; 5GHz band preferred (Default for 802.11abg radios)
 - ``bga`` – all 802.11b and 802.11a/g rates; 2.4GHz band preferred
 - ``gn`` – all 802.11g/n rates; 2.4GHz band only
 - ``an`` – all 802.11a/n rates; 5GHz band only
 - ``abgn`` – all 802.11b and 802.11a/g/n rates; 5GHz band preferred
 - ``bgna`` – all 802.11b and 802.11a/g/n rates; 2.4GHz band preferred
 - ``adhoc`` – peer to peer mode

#### ``-pwr [cam|fast|max]``
Power save mode for the radio:

 - ``cam`` – constantly awake mode
 - ``fast`` – fast power save mode
 - ``max`` – maximum power savings

#### ``-auth [open|shared|leap]``
802.11 authentication type
#### ``-eap [none|leap|eapfast|peapmschap|peapgtc|eaptls|eapttls|peaptls]``
Extensible Authentication Protocol type used for 802.1X authentication to the AP
#### ``-encrypt [none|dynamic|static|wpapsk|wpatkip|wpa2psk|wpa2aes|cckmtkip|ckip|autockip|cckmaes]``
Set the encryption mode for the specified configuration to the given mode.  If static is selected, the wep key(s) must be specified along with the transmit key number.  When using psk encryption, use the –psk option below to enter the preshared key.

 - ``none`` – no encryption
 - ``dynamic`` – WEP with key generated during EAP authentication
 - ``static`` – WEP with up to four static keys -- 40-bit or 128-bit in ASCII or hex -- defined under WEP/PSK Keys
 - ``ckip`` – WEP with Cisco Key Integrity Protocol
 - ``autockip`` – WEP with Cisco Key Integrity Protocol (?)
 - ``wpapsk`` – TKIP with PSK -- ASCII passphrase or hex PSK -- defined under WEP/PSK Keys
 - ``wpatkip`` – TKIP with key generated during EAP authentication
 - ``wpa2psk`` – AES with PSK -- ASCII passphrase or hex PSK -- defined under WEP/PSK Keys
 - ``wpa2aes`` – AES with key generated during EAP authentication
 - ``cckmtkip`` – TKIP with key generated during EAP authentication and with Cisco key management protocol for fast reauthentication
 - ``cckmaes`` – AES with key generated during EAP authentication and with Cisco key management protocol for fast reauthentication

#### ``-key1 <wep key>``
#### ``-key2 <wep key>``
#### ``-key3 <wep key>``
#### ``-key4 <wep key>``
When specifying static wep mode, 1 to 4 static wep keys must be entered.  Keys must be 5 or 13 characters long if ASCII.  Keys may also be entered in hex and must be 10 or 26 characters long.
#### ``-tx {1-4}``
Wep transmit key number 
#### ``-txpwr [max|50|30|20|10|5|1]``
Transmitter power in milliwatts.
#### ``-brate [auto|1|2|5.5|11|12|18|24|36|48|54]``
The allowed bit rate (leave this set to auto normally).

#### ``-psk <preshared key>``
When specifying wpapsk or wpa2psk encryption modes, the preshared key must be entered with this option.  Note that the preshared key may not contain spaces or begin with ‘-‘.

#### ``-eapuser <user name>``
Username or Domain/Username

#### ``-eappassword <password>``
Password if applicable 

#### ``-eapcacert <cert file name/server cert friendly name>``
If eapvalidateserver and eapusemsstore are both on, eapcacert is evaluated as the certificate name to use with the server.  Otherwise eapcacert is the name of the certificate file to use.

#### ``-eapusercert <user cert friendly name>``

#### ``-eappac <PAC filename>``
Used when eap mode is set to eapfast.

#### ``-eappacpassword <PAC password>``
Used when eap mode is set to eapfast.

#### ``-eapvalidateserver [on|off]``
Set on when using a CA certificate to validate an authentication server.

#### ``-eapusemsstore [on|off]``
Set on if the Microsoft certificate store should be used for a CA certificate if the eapvalidateserver is on.


----------


### ``-global``
Modify the global WLAN card settings using the options below.
#### ``-roam_trig [50|55|60|65|70|75|80|85|90]``
When moving average RSSI from current AP is weaker than Roam Trigger, radio does a roam scan where it probes for an AP with a signal that is at least Roam Delta dBm stronger
#### ``-roam_delta [5|10|15|20|25|30|35]``
When Roam Trigger is met, second AP's signal strength (RSSI) must be Roam Delta dBm stronger than moving average RSSI for current AP  before radio will attempt to roam to second AP
#### ``-roam_per [5|10|15|20|25|30|35|40|45|50|55|60]``
After association or roam scan (with no roam), radio will collect RSSI scan data for Roam Period seconds before considering roaming
#### ``-dfs_chan [on|off]``
DFS channels support for 5GHz (802.11a/n)
#### ``-aggressive [on|off]``
When this setting is On and the current connection to an AP becomes tenuous, the radio scans for available APs more aggressively. Aggressive scanning complements and works in conjunction with the standard scanning that is configured through the Roam Trigger, Roam Delta, and Roam Period settings. Summit recommends that the Aggressive Scan global setting be On unless there is significant co-channel interference because of overlapping coverage from APs that are on the same channel.
#### ``-ccx [optimized|on|off]``

 - on – Use Cisco IE and CCX version number; support all CCX features
 - optimized – Use Cisco IE and CCX version number; support all CCX features except AP-assisted roaming, AP-specified maximum transmit power, and radio management
 - off – Do not use Cisco IE and CCX version number

#### ``-wmm [on|off]``
Use of Wi-Fi Multimedia Extensions, also known as WMM
#### ``-auth_srvr [1|2]``
Auth Server type (1 – ACS, 2 – SBR)
#### ``-txdiver [main|aux|on]``
Use main antenna only, use aux antenna only, or use diversity.
#### ``-rxdiver [main|aux|startmain|startaux]``
Use main antenna only, use aux antenna only, use diversity starting with the main antenna or use diversity starting with the aux antenna.
#### ``-frag {256-2346}``
If packet size (in bytes) exceeds threshold, then packet is fragmented
#### ``-rts {0-2347}``
Packet size above which RTS/CTS is required on link
#### ``-cert_path <path name>``
The directory path to search for security certificates. Maximum of 64 characters.
#### ``-opt_chan <channels>``
When using b/g optimized mode (bg_lrs), setting this value restricts the radio to using and searching the specified channels.  The value is given in hex, with bit 0 representing channel 1, and bit 12 representing channel 13. So to configure channels 1, 6 and 11, the channel value would be “0421”
#### ``-avg_rssi {2-8}``
The number of samples to be averaged together when computing RSSI
#### ``-probe {2-60}``
delay before sending out probes when AP's aren't located
#### ``-tray [on|off]``
Display the signal strength icon in the System Tray.
#### ``-opmk [on|off]``
Enable or disable OPMK caching (OKC).
#### ``-disppwd [on|off]``
Should sensitive information such as passwords be displayed (on) or hidden with asterisks (off).
#### ``-adminrequire [on|off]``
Is the administration password required to edit the radio settings with the Summit Configuration Utility.
#### ``-adminpwd <password>``
The password required if adminrequire is on.


----------


### NOTES:

 - Any string may be quoted to allow it to contain spaces or special characters, except that quote itself is not allowed in the string.
 - As long is it is not within a quoted string, the following two special sequences may be used to insert the given value:
  - ``%s`` – insert the terminal serial number. So to set the client name to the serial number of the terminal, you would use ``-client %s``
  - ``%m`` – insert the terminal RF MAC address. So to set the client name to DLM followed by the MAC address, you would use ``–client DLM%m``

