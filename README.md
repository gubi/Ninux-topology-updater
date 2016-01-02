# Remote Ninux.org topology updater
If you are a disconnected _island_, these scripts allow the MapServer to retrieve your Ninux topology.

## How it works
The shell script ask your local ground router (or a single antenna, depending on your preference) for a topology map, then store data in a variable and sends it to a remote php script in your server.<br />
The remote script checks the sha1 token and if matched write received data in a file.<br />
That's it!

Note that the remote script will receive a simple POST data with `token` and `data` params, whereas:
* `token` is your sha1 secret key
* `data` is topology map text

I chose the PHP language for the remote script and the SHA1 as token engine, simply for my convenience, but you can change/configure/extend as you prefer (keep me updated and we will include your integrations in this repo ;) ).

## Installing and configuring
Before use this script keep in mind that you will need:
* A personal SHA1 secret token<br />You can generate one here: http://www.sha1-online.com/
* **Local**
 * A Ninux.org antenna or a ground router with [OLSR](https://it.wikipedia.org/wiki/Optimized_Link_State_Routing_Protocol) installed
 * A local server with [cURL](https://it.wikipedia.org/wiki/Curl) installed
 * A stable Internet connectivity
* **Remote**
 * A remote server that hosts the script


Put the `send_ninux_topology.sh` in your local server, change variables as described above and add the cronjob (described below).<br />
On the remote server, add the file PHP and set the right permissions

====

## The cronjob
```bash
# Send every 30 minutes the Ninux topology
0,30 * * * * /root/send_ninux_topology.sh 2>&1
```
