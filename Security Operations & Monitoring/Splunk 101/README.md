
# Splunk 101

This is my walk through of the [Splunk 101](https://tryhackme.com/room/splunk101) room on tryhackme.com


## Splunk Apps

There is a Splunk add-on on the desktop. Upload this add-on into the Splunk instance. Restart Splunk when prompted to.

### What is the 'Folder name' for the add-on?

Once we upload the add-on we can see the information in the apps manager.

![Folder](./Screenshots/3_1-2.png?raw=true 'Folder')

### What is the Version?

![Version](./Screenshots/3_1-2.png?raw=true 'Version')
## Adding Data

Upload the Splunk tutorial data on the desktop

### How many event are in the source?

![Events](./Screenshots/4_1.png?raw=true 'Events')


## Splunk Queries

### Use Splunk to Search for the phrase 'failed password' using tutorialdata.zip as the source.

`* "failed password"`

### What is the sourcetype?

![Sourcetype](./Screenshots/5_2.png?raw=true 'Sourcetype")

### Look at the Patterns tab. What is the last username in this tab?

![Last User](./Screenshots/5_4.png?raw=true 'Last User')

### Search for failed password events for this specific username. How many events are returned?

We can use a query like: `"failed password" myuan`

![Failed Events](./Screenshots/5_5.png?raw=true 'Failed Events')
## Sigma Rules

### Use the **Select document** [feature](https://uncoder.io/). What is the Splunk query for 'sigma:APT29'?

![Splunk Query](./Screenshots/6_1.png?raw=true 'Splunk Query')

### Use the Github Sigma [repo](https://github.com/SigmaHQ/sigma). What is the Splunk query for 'CACTUSTORCH Remote Thread Creation'?

*Hint: Check the Windows rules. Copy and paste into uncoder.io*

If we go into *rules > windows > create_remote_thread_sysmon_cactustorch.yaml* we can copy and past that into
uncoder.io

![CACTUSTORCH](./Screenshots/6_2.png?raw=true 'Cactustorch')



## Dashboards & Visualizations

### What is the highest EventID?

`11`