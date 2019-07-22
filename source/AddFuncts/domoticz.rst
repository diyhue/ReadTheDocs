Domoticz
========

diy-hue includes native support for Domoticz lights/switches. 
As there is no discover process provided by domotics, they must be added manually into ''config.json''.

Add a new light(s)
-------------------
1. Stop the hue-emulator service::

    sudo systemctl stop hue-emulator

2. Save ''config.json''::
    sudo cp /opt/hue-emulator/config.json /opt/hue-emulator/config.json.bak

3. Edit config.json::

    sudo nano /opt/hue-emulator/config.json

- Add light adress::

    "lights_address": {
        "3": {
            "ip": "192.168.1.100:8080",
            "light_id": "281",
            "protocol": "domoticz"
        },
    },

''ip'' is the Domoticz host
''light_id'' is the light/switch IDX
''"protocol": "domoticz"'' allow diyhue to format the correct request

- Add light details for hue app::

  "lights": {
        "3": {
            "manufacturername": "domoticz",
            "name": "Eettafel",
            "state": {
                "alert": "none",
                "on": true,
                "reachable": true
            },
            "swversion": "V1.04.12",
            "type": "On/Off plug-in unit",
            "uniqueid": "xx:xx:xx:xx:xx:xx:xx:xx-xx"
        },

4. Restart the hue-emulator service::

    sudo systemctl start hue-emulator
