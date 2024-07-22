Home Kit
========
Follow the below instuctions to setup diyHue to work with Home Kit


1. Install homebridge
2. Install homebridge-hue
3. Prepare config.json for homebridge with an intial homebridge-hue plugin setup (need to be modified after the first run, see next step)::

    config.json initial:
    {
        "bridge": {
            "name": "HB_Diy_Hue",
            "username": "mac-address homebridge",
            "port": 51838,
            "pin": "whatyouwant"
        },
        "accessories": [],
        "platforms": [
            {
                "platform": "Hue",
                "users": {
                    "": ""
                },
                "hosts": "put_here_the_ip_diyhue_bridge",
                "sensors": false,
                "nativeHomeKitSensors": true,
                "nativeHomeKitLights": false,
                "excludeSensorTypes": ["Daylight", "CLIP", "Geofence"],
                "lights": true
            }
        ]
    }

4. Run home bridge for the first time (not via service). Run ``sudo homebridge -U /<path that contains config.json>`` (showed before), in my example::

    sudo homebridge -U /home/pi/.homediyhue

    [2/3/2019, 9:24:33 AM] [Hue] RpiHue: HA-Bridge v1810251352, api v1.24.0
    [2/3/2019, 9:24:33 AM] [Hue] RpiHue: bridge request 2: post / {"devicetype":"homebridge-hue#raspberrypi"}
    [2/3/2019, 9:24:33 AM] [Hue] RpiHue: bridge error 101: link button not pressed
    [2/3/2019, 9:24:33 AM] [Hue] RpiHue: press link button on the bridge to create a user

5. Go to diyhue webpage and simulate the link button pressed (http://{IP_ADDRESS}/#linkbutton)::

    [2/3/2019, 9:24:48 AM] [Hue] RpiHue: created user - please edit config.json and restart homebridge
    "platforms": [
        {
            "platform": "Hue",
            "users": {
                "B827EBFFFE959284": "470a0c52e89555f73b11904c84af6192"
            }
        }
    ]

6. Stop homebridge
7. Edit config.json with this user
8. Start homebridge (via service or manually)::

    [2/3/2019, 9:24:48 AM] [Hue] RpiHue: 5 accessories
    [2/3/2019, 9:24:48 AM] [Hue] masked debug info dumped to /home/pi/.homediyhue/homebridge-hue.json.gz
    [2/3/2019, 9:24:48 AM] [Hue] Initializing platform accessory 'RpiHue'...
    [2/3/2019, 9:24:48 AM] [Hue] RpiHue: 2 services
    [2/3/2019, 9:24:48 AM] [Hue] Initializing platform accessory 'Hue bulb Plug 01'...
    [2/3/2019, 9:24:48 AM] [Hue] Initializing platform accessory 'Hue bulb Plug 01 2'...
    [2/3/2019, 9:24:48 AM] [Hue] Initializing platform accessory 'Hue bulb Plug 01 3'...
    [2/3/2019, 9:24:48 AM] [Hue] Initializing platform accessory 'Hue bulb Plug 01 4'...