
# Osquery

This is my write-up for working through the [Osquery room](https://tryhackme.com/room/osqueryf8).






## Interacting with the Osquerry Shell

### What is the Osquerry version?

Running `.show`, we can see the osquerry version is `4.6.0.2`.

![Osquery Version](./Screenshots/version.png?raw=true "Osquery Version")

### What is the SQLite version?

We can see the SQLite version is `3.34.0` with the `.show` command.

![SQLite Version](./Screenshots/sqlite-version.png?raw=true "SQLite Version")

### What is the default output mode?

With the same command, we can see the default output mode is `pretty`.

![Default Output Mode](./Screenshots/output-mode.png?raw=true "Default Output Mode")

### What is the meta-command to set the output to show one value per line?

The meta-command is `.mode line`.

![Line Meta-Command](./Screenshots/line-command.png?raw=true "Line Meta-Command")

### What are the 2 meta-commands to exit osqueryi?

Both the `.exit` and `.quit` meta-commands will exit osqueri.

![Exit](./Screenshots/exit.png?raw=true "Exit")


## Schema Documentation

Head over to the schema documentiation [here](https://osquery.io/schema/4.7.0/).

### What table would you query to get the version of Osquery installed on the Windows endpoint?

We can query `osquery_info` to find the version of Osquery installed.

![osquery_info](./Screenshots/osquery_info.png?raw=true "osquery_info")

### How many tables are there for this version of Osquery?


After finding out the osquery version, we can see that there are `266` tables for this version.

![Version](./Screenshots/get-version.png?raw=true "Version")

![Tables](./Screenshots/tables.png?raw=true "Tables")

### How many of the tables for this version are compatible with Windows?

We can select to show only Tables compatible with Windows and see that there are `96` tables.

![Windows Compatible](./Screeenshots/windows.png?raw=true "Windows Compatible")

### How many tables are compatible with Linux?

There are `155` tables compatible with Linux.

![Linux Compatible](./Screenshots/linux.png?raw=true "Linux Compatible")

### What is the first table listed that is compatible with both Linux and Windows?

`arp_cache` is the first table listed when we show tables compatible with both Linux and Windows

![Linux & Windows](./Screenshots/linux-windows.png?raw=true "Linux & Windows")



## Creating queries

### What is the query to show the username field from the users table where the username is 3 characters long and ends with 'en'? (user single quotes in your answer)

When can use `.schema users` to see the schema of the users table. Now we can create our query. `SELECT username FROM user WHERE username LIKE '_en'`

![Query Username](./Screenshots/query-username.png?raw=true 'Query Username')

## Using Kolide Fleet

In this task, we will look at an open-source Osquery Fleet Manager known as [Kolide Fleet](https://github.com/kolide/fleet). 

With Kolide Fleet, instead of using Osquery locally to query an endpoint, you can query multiple endpoints from the Kolide Fleet UI. 

**Note:** The open-source repo of Kolide Fleet is no longer supported and was retired on November 4th, 2020. A commercial version, known as Kolide K2, is available. You can view more about it here. There is a more recent repo called fleet, a fork of the original Kolide Fleet, and as per the creators of Kolide Fleet, "it appears to be the first of many promising forks."

### What is the Osquery Enroll Secret?

`k3hFh30bUrU7nAC3DmsCCyb1mT8HoDkt`

![Enroll](./Screenshots/enroll-secret.png?raw=true 'Enroll Secret')

### What is the Osquery version?

`4.2.0`

![OSQuery Version](./Screenshots/osquery_info.png?raw=true 'OSQuery Version')


### What is the path for the running osqueryd.exe process?

We can query `Select * FROM processes` and then filter the results.

`C:\Users\Administrator\Desktop\launcher\windows\osqueryd.exe`

![Path](./Screenshots/path.png?raw=true 'Path')


## Osquery extensions

Extensions add functionality/features (i.e., additional tables) that are not included in the core Osquery. Anyone can create extensions for Osquery. The official documentation on this subject is [here](https://osquery.readthedocs.io/en/latest/deployment/extensions/). 

If you perform a search, you'll find some interesting ones that can be downloaded and implemented with Osquery with little hassle. Others might require extra steps, such as setting up additional dependencies and compiling the extension before use. 

Below are 2 repos of Osquery extensions that you can play with. 

    https://github.com/trailofbits/osquery-extensions
    https://github.com/polylogyx/osq-ext-bin

The Polylogyx extension is available in the attached VM, and you will load and interact with this extension in the upcoming tasks.

### According to the polylogyx readme, how many 'features' does the plug-in add to the Osquery core?

`23`

***Note:*** Current version adds 25 'features'.

## Linux and Osquery

### What is the 'current_value' for kernel.osrelease?

We can use the query
`SELECT * FROM kernel_info;`.

![Kernel Version](./Screenshots/kernel_version.png?raw=true 'Kernel Version')

### What is the uid for the bravo user?

We can user the query
`SELECT username, uid FROM users WHERE username='bravo'`

![Bravo UID](./Screenshots/bravo_uid.png?raw=true 'Bravo UID')

### One of the users performed a 'Binary Padding' attack. What was the target file in the attack?

After some digging around, I found a yara file in `/var/osquery/yara/`. We can pass the yara file in our query.

`SELECT * FROM yara WHERE path LIKE '/home/tryhackme/%%' AND sigfile='/var/osquery/yara/scanner.yara';`

![Binary Padding](./Screenshots/binary_padding.png?raw=true 'Binary Padding')

`notsus`

### What is the hash value for this file?

We can get the hash value with
`md5sum /home/tryhackme/notsus`

![hash](./Screenshots/hash.png?raw=true 'hash')

`3df6a21c6d0c554719cffa6ee2ae0df7`

### Check all file hashes in the home directory for each user. One file will not show any hashes. Which file is that?

After looking at the schema for the hash table, we can build our query.

![schema](./Screenshots/hash_schema.png?raw=true 'Hash Schema')

`SELECT md5, path FROM hash WHERE path LIKE '/home/tryhackme/%';`

![Hash Query](./Screenshots/hash_query.png?raw=true 'Hash Query')

### There is a file that is categorized as malicious in one of the home directories. Query the Yara table to find this file. Use the sigfile which is saved in '/var/osquery/yara/scanner.yara'. Which file is it?

`SELECT * FROM yara WHERE path LIKE '/home/%%' AND count > 0 AMD sigfile='/var/osquery/yara/scanner.yara';`

We can see the file and the rules it matched..

![Bad File](./Screenshots/malicious_file.png?raw=true 'Bad File')

### What were the 'matches'?

Same query as above.

### Scan the file from Q\#3 with the same Yara file. What is the entry for 'strings'?

We can select the `path` to verify it is the correct file and `strings` to get our answer.

`SELECT path, strings FROM yara WHERE path='/home/tryhackme/notsus' AND sigfile='/var/osquery/yara/scanner.yara';`

![Notsus](./Screenshots/notsus.png?raw=true 'notsus')




## Windows and Osquery



For this exercise, use either Kolide Fleet or the Windows CMD/PowerShell. 

Note: For the questions which involve the Polylogyx osq-ext-bin extension, you'll need to interact with Osquery via the command line. 

To load the extension: `osqueryi --allow-unsafe --extension "C:\Program Files\osquery\extensions\osq-ext-bin\plgx_win_extension.ext.exe"`

Wait for the command prompt to reflect the phrase `Done StartDriver`. This will indicate that the extension is fully loaded into the session.

Tip: If the phrase doesn't appear after a minute or so, hit the ENTER key. It should appear right after. 

Resources for Polylogx osq-ext-bin:

    https://github.com/polylogyx/osq-ext-bin/blob/master/README.md
    https://github.com/polylogyx/osq-ext-bin/tree/master/tables-schema

### What is the description for the Windows Defender Service?

Take a look at the services schema to build the query.

![Services Schema](./Screenshots/services_schema.png?raw=true 'Services Schema')

The query will look something like this:
`SELECT name, description FROM services WHERE display_name='Windows Defender Antivirus Service';`

![WinDefend Query](./Screenshots/windefend_query.png?raw=true 'WinDefend Query')

### There is another security agent on the Windows endpoint. What is the name of this agent?

`SELECT name, publisher FROM programs;`

![Security Agent](./Screenshots/security_agent.png?raw=true 'Security Agent')

### What is required with win_event_log_data?

First load the extension: `osqueryi --allow-unsafe --extension "C:\Program Files\osquery\extensions\osq-ext-bin\plgx_win_extension.ext.exe"`.
Now query win_event_log_data: `SELECT * FROM win_event_log_data;`

![Required](./Screenshots/win_event_log_data.png?raw=true 'Required')

### How many sources are returned for win_event_log_data?

`SELECT COUNT(*) FROM win_event_log_channels;`

![Sources](./Screenshots/sources.png?raw=true 'Sources')

### What is the schema for win_event_log_data?

`.schema win_event_log_data`

![Schema](./Screenshots/schema.png?raw=true 'Schema')

### The previous file scanned on the Linux endpoint with Yara is on the Windows endpoint.  What date/time was this file first detected? (Answer format: YYYY-MM-DD HH:MM:SS)

Looking at https://docs.microsoft.com/en-us/windows/security/threat-protection/microsoft-defender-antivirus/troubleshoot-microsoft-defender-antivirus, we can see that the event ID will be `1116`.

`SELECT eventid, datetime FROM win_event_log_data WHERE source='Microsoft-Windows-Windows Defender/Operational' AND eventid='1116';`

![Time](./Screenshots/time.png?raw=true 'Time')

### What is the query to find the first Sysmon event? Select only the event id, order by date/time, and limit the output to only 1 entry.

First find the sysmon source: `SELECT * FROM win_event_log_channels WHERE source LIKE '%sysmon%';`

Then build your query.

![Sysmon ID](./Screenshots/sysmon.png?raw=true 'Sysmon ID')

