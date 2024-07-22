FAQ
===

What lights can I use with this project?
----------------------------------------

* MiLight
* Yeelight
* LYT8266
* Phillips Hue
* Ikea Tradfri

The following can be controlled with an ESP8266 or ESP32:

* WS2812B and SK6812 smart led strips
* Pwm RGB-CCT LEDs
* Pwm RGBW LEDs
* Pwm RGB LEDs
* Pwm CCT LEDs
* Pwm Dimming (up to 6 lights for every esp8266)
* On/Off plugs/lights (up to 6 lights for every esp8266)
* On/Off 433Mhz devices (multiple devices for every esp8266)

Do I need a Phillips Hue Bridge to control genuine Phillips Hue bulbs with diyHue?
----------------------------------------------------------------------------------

Hue bulbs use the zigbee radios to comunicate so you either need a genuine Phillips Hue Bridge or a raspbee module.

Unable to Connect - diyHue Bridge is detected but i can´t pair!
---------------------------------------------------------------

In order to connect with diyHue, you need to pair the APP with the Bridge Emulator.
Press Link APP Button of diyHue found here `http://{IP_ADDRESS}/#linkbutton`.

If Hue Essentials app (or other 3rd party hue apps) can pair with the bridge emulator but official Hue app not most likely you have a [bad https certificate](https://diyhue.readthedocs.io/en/latest/AddFuncts/debug.html#https-certificate-check)

For further Info see the :ref:`config` page

My Phillips TV detects diyHue Bridge but I am unable establish a connection
---------------------------------------------------------------------------

Follow the instructions for pairing. In addition it can help to disconnect your `Internet` for a short period to make sure the TV performs a local Network search instead of using remote API of Phillips.

What do I need to use IKEA Tradfri devices?
-------------------------------------------

IKEA Tradfri bulbs can be controlled when paired with an IKEA hub. If you wish to use IKEA switches then you must use the Raspbee module. This will enable you to control both the IKEA lights and switches without an IKEA hub.

App XYZ doesn't work!
---------------------

Not all apps for Phillips Hue work out of the box with diyHue. If you are unable to pair the app with diyHue automatically, try looking for a manual pair option in the app. If that fails then open an issue here on GitHub. We will try our best to to get your app to work but due to the lack of support for the remote API with diyHue, some apps will never work.

My Google Home / Google Assistant doesn't find diyHue!
------------------------------------------------------

Google Assistant uses the remote API to find your Hue bridge. The remote API is not supported by diyHue. At the moment it looks like it never will. However, don't despair. There is an option to get it to work. [Home Assistant](https://www.home-assistant.io/) is software used to provide a unified hub for all your smart devices. It has support for diyHue and Google Assistant, so you can import your diyHue lights into Home Assistant and use Google Assistant to control them via Home Assistant. There are plenty of instructions on the Home Assistant website however details on Phillips Hue with home assistant can be found [here](https://www.home-assistant.io/components/hue/) and details on Google Assistant [here](https://www.home-assistant.io/components/google_assistant/) If you need more help, ask on our Slack chat or on the Home Assistant forums.

Philips Hue App complains about Bridge v1 detected and not supported anymore
----------------------------------------------------------------------------
The most common reason for this is because your diyHue Installation hasn't go created a valid certificate. 

How to check this?

Just type curl https://<diyHue Server IP-Adress>/api/nouser/config -v -k

If this request is failing, your certificate is not valid.

Here's how to solve this:

1. Delete the cert.pem file in the mounted directory /mnt/hue-emulator/config/ 
2. If you're using Docker, you maybe missed to start the Container with the correct IP and MAC-Adress (see Documentation). Stop and remove the container and start it with the correct parameters.
