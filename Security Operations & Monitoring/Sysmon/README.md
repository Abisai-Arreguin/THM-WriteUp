
# Sysmon
This is my write-up for working through the Sysmon room - https://tryhackme.com/room/sysmon

Event files used within this task have been sourced from the [EVTX-ATTACK-SAMPLES](https://github.com/sbousseaden/EVTX-ATTACK-SAMPLES/tree/master) and [SysmonResourcesGithub](https://github.com/jymcheong/SysmonResources) repositories.


## Investigation 1 - ugh, BILL THAT'S THE WRONG USB!
In this investigation, your team has received reports that a malicious file was dropped onto a host by a malicious USB. They have pulled the logs suspected and have tasked you with running the investigation for it.

Logs are located in `C:\Users\THM-Analyst\Desktop\Scenarios\Investigations\Investigation-1`

### What is the full registry key of the USB device calling svchost.exe in Investigation 1?

![Registry Key](./Screenshots/Registry_Key.png?raw=true "Registry Key")

`HKLM\System\CurrentControlSet\Enum\WpdBusEnumRoot\UMB\2&37c186b&0&STORAGE#VOLUME#_??_USBSTOR#DISK&VEN_SANDISK&PROD_U3_CRUZER_MICRO&REV_8.01#4054910EF19005B3&0#\FriendlyName`

### What is the device name when being called by RawAccessRead in Investigation 1?

![Device Name](./Screenshots/Device.png?raw=true "Device Name")

`\Device\HarddiskVolume3`

### What is the first exe the process executes in Investigation 1?

`rundll32.exe`


## Investigation 2 - This ins't an HTML file?

Another suspicious file has appeared in your logs and has managed to execute code masking itself as an HTML file, evading your anti-virus detections. Open the logs and investigate the suspicious file.

Logs are located in `C:\Users\THM-Analyst\Desktop\Scenarios\Invetigations\Investigation-2`

### What is the full path of the payload in Investigation 2?

![Payload Path](./Screenshots/payload_path.png?raw=true "Payload Path")

`C:\Users\IEUser\AppData]Local\Microsoft\Windows\Temporary Internet Files\Content.IE5\S97WTYG7\update.hta`

### What is the full path of the file the payload masked itself as in Investigation 2?

![Payload Mask](./Screenshots/Payload_mask.png?raw=true "Payload Mask")

`C:\Users\IEUser\Downloads\update.html`

### What signed binary executed the payload in Investigation 2?

![Signed Binary](./Screenshots/signed_binary.png?raw=true "Signed Binary")

`C:\Windows\System32\mshta.exe`

### What is the IP of the advesary in Investigation 2?

![Destination IP](./Screenshots/destination_ip.png?raw=true "Destination IP")

`10.0.2.18`

### What back connect port is used in Investigation 2?

![Port](./Screenshots/port.png?raw=true "Port")

`4443`
## Investigation 3.1 -3.2 - Where's the bouncer when you need him

Your team has informed you that the advesary has managed to set up a persistence on your endpoints as they continue to move throughout your network. Find how the adversary managed to gain persistence using logs provided.

Logs are located in `C:\Users\THM-Analyst\Desktop\Scenarios\Investigations\Investigation-3.1`
and `C:\Users\THM-Analyst\Desktop\Scenarios\Investigations\Investigation-3.2`.

### What is the IP of the suspected advesary in Investigation 3.1?

![Destination Port](./Screenshots/3.1-destination_port.png?raw=true "Destination Port")

`172.30.1.253`

### What is the hostname of the affected endpoint in Investigation 3.1?

![Hostname](./Screenshots/3.1-host.png?raw=true "Hostname")

`DESKTOP-O153T4R`

### What is the hostname of the C2 server connecting to the endpoint in Investiation 3.1?

![C2 Server](./Screenshots/3.1-destination_host.png?raw=true "C2 Server")

`empirec2`

### Where in the registry was the payload stored in Investigation 3.1?

![Registry](./Screenshots/3.1-registry.png?raw=true "Registry")

`HKLM\SOFTWARE\Microsoft\Network\debug`

### What PowerShell launch code was used to launch the payload in Investigation 3.1?

![Launch Code](./Screenshots/3.1-launch_code.png?raw=true "Launch Code")

`"C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -c "$x=$((gp HKLM:Software\Microsoft\Network debug).debug);start -Win Hidden -A \"-enc $x\" powershell";exit;`

### What is the IP of the adversary in Investigation 3.2?

![Destination IP](./Screenshots/3.2-destination_ip.png?raw=true "Destination IP")

`172.168.103.188`

### What is the full path of the payload location in investigation 3.2?

![Full Path](./Screenshots/3.2-path.png?raw=true "Full Path")

`c:\users\q\AppData:blah.txt`

### What was the full command used to create the scheduled task in Investigation 3.2?

![Full Command](./Screenshots/3.2-full_command.png?raw=true "Full Command")

`"C:\WINDOWS\system32\schtasks.exe" /Create /F /SC DAILY /ST 09:00 /TN Updater /TR "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -NonI -W hidden -c \"IEX ([Text.Encoding]::UNICODE.GetString([Convert]::FromBase64String($(cmd /c ''more < c:\users\q\AppData:blah.txt'''))))\""`

### What process was accessed by schtasks.exe that would be considered suspicious behavior in Investiation3.2?

![Process](./Screenshots/3.2 - process.png?raw=true "Process")

`lsass.exe`


## Investigation 4 - Mom look! I built a botnet!

### What is the IP of the adversary

![IP](./Screenshots/4-IP.png?raw=true "IP")

`172.30.1.253`

### What port is the adversary operating on in Investigation 4?

![Port](./Screenshots/4-port.png?raw=true "Port")

`80`

### What C2 is the adversary utilizing in Investigation4?

![C2](./Screenshots/4-C2.png?raw=true "C2")

`empire`
## Authors

- [@Abisai-Arreguin](https://github.com/Abisai-Arreguin)

  