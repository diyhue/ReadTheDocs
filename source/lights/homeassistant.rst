Home Assistant
==============

diyHue has support for lights that are controlled via `Home Assistant <https://www.home-assistant.io/>`_.

You setup the lights you wish to include/exclude in Home Assistant. You then search for lights (in the app) and the lights will be added.

For this to work you need to have a working Home Assistant setup with the Rest API enabled. This is out of scope of this documentation - please check the Home Assistant documentation.

Configuration
-------------

.. note::
    Once you have completed the setup below you will need to search for lights in Hue Essentials/Hue App or you can navigate to http://diyhue_ip/scan


You will need to start the bridge once (this will generate/update the config) and then you'll need to edit the ``config.json`` file.

If you're using **docker** to run diyHue, make sure you **stop the container** before changing the config file.

This is only a small portion of the config, but you should look for the ``emulator`` => ``homeassistant`` section. It should look like this:

.. code-block:: JSON

    {
        "emulator": {
          "homeassistant": {
            "enabled": false,
            "homeAssistantIp": "127.0.0.1", 
            "homeAssistantPort": 8123, 
            "homeAssistantToken": "",
            "homeAssistantIncludeByDefault": true
          },
      }
    }

Now fill in the correct values:

* ``enabled``: **"true"**
* ``homeAssistantIp``: set to the IP address of your Home Assistant install
* ``homeAssistantPort``: By default this is 8123 but change it here if you are running on a different port.
* ``homeAssistantToken``: This is a Home Assistant long lived access token - details are on the `Home Assistant website <https://developers.home-assistant.io/docs/auth_api/#long-lived-access-token>`_ on how to create one
* ``homeAssistantIncludeByDefault``: If set to **"true"** all lights from Hoem Assistant will be included unless you explicity exclude them. If you don't want all the lights in Home Assistant to be included then set this to **"false"** and explicity include the lights you want to use in diyhue

Most of the other lights that diyhue supports could also be added directly to Home Assistant. 
If you are using the Home Assistant integration you may wish to disable the other integrations so that they don't appear in diyHue directly (e.g. Shelly, Tasmota) if you plan to expose these devices by Home Assistant. 
This can be done by editing the ``config.json`` file. 
Look for the emulator section and set any you wish to disable to "false"

.. code-block:: JSON

    {
        "emulator": {
          "esphome": {
            "enabled": false
          },
          "hyperion": {
            "enabled": false
          },
          "network_scan": {
            "enabled": false
          },
          "shelly": {
            "enabled": false
          },
          "tasmota": {
            "enabled": false
          },
          "yeelight": {
            "enabled": false
          }
      }
    }



Including/Excluding Lights in Home Assistant
--------------------------------------------

To include or exclude individual lights you do this in Home Assistant by customizing the entity.

You can do this in several ways - one way is to use the UI using the customize entities page : <https://www.home-assistant.io/docs/configuration/customizing-devices/#customization-using-the-ui>

You need to add an attribute called ``diyhue`` and set it to ``include`` to include the light and ``exclude`` to exclude the light.

Another way is to enter it manually into the Home Assistant config files: <https://www.home-assistant.io/docs/configuration/customizing-devices/#manual-customization>

.. code-block:: yaml

    light.my_light_1:
      diyhue: include
    light.my_light_2:
      diyhue: exclude
  

Configure Rooms/Zones/Groups in Home Assistant
----------------------------------------------

You can set Home Assistant lights to go into a Room/Zone/Group by customizing entities. Which will reduce the requirment to set these up via the apps.

In the same way as you Include/Exclude lights you do this by customizing the entity in Home Assistant.

You can add the following key and the value is the name you want to use:

 * ``diyhue_room``
 * ``diyhue_zone``
 * ``diyhue_group``

The integration will add it to a room with exactly the same name, if one doesn't exist then it will create one.

Whilst a light can be added to multiple zones/groups using the Hue App or Hue Essentials when the configuration is done in Home Assistant it only supports a single group/zone/room for each light.
However this doesn't stop you using the apps to add the light to additional Groups/Zones.

You can also set the icon if a room is created by setting ``diyhue_class``.
If the room already exists then this will be ignored. It won't change an existing icon. 

The class has to be a valid icon string otherwise the Hue app will continually crash so be careful.
To find valid icon strings set an icon on a room and look in the Hue Essentials app for the group and look at it's "class". 

If the room already exists then this will be ignored. It won't change an existing icon. 

Some valid examples:

 * "Attic"
 * "Barbecue"
 * "Bathroom"
 * "Bedroom"
 * "Garden"
 * "Hallway"
 * "Kids bedroom"
 * "Kitchen"
 * "Living room"
 * "Lounge"
 * "Nursery"
 * "Staircase"
 * "Storage"
 * "Studio"
 * "Terrace"

Full example of adding a light to a room with an icon in Home Assistant using the customize.yaml

.. code-block:: yaml
    
    light.shower_room_spot_lights
      diyhue: include
      diyhue_class: "Bathroom"
      diyhue_room: "Shower Room"
    

