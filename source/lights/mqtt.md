# MQTT - zigbee2mqtt

diyHue has support for lights that are controlled over mqtt. It uses the auto discovery feature described [here](https://www.home-assistant.io/docs/mqtt/discovery/).
This emulator connects directly to mqtt (after you set the correct parameters), and subscribes to the defined auto discovery prefix.

You'll still need to look for lights (in the app), but any lights that emit an auto discovery message are added once you search for lights.

For this to work you'll need to setup a mqtt server, that is out of the scope of this documentation.

## Configuration

You will need to start the bridge once (this will generate/update the config) and then you'll need to edit the `config.json` file.

If you're using **docker** to run diyHue, make sure you **stop the container** before changing the config file.

This is only a small portion of the config, but you should look for the `emulator` => `mqtt` section. And fill in the correct values.

```json
{
   "emulator": {
    "alarm": {
        "email": "",
        "lasttriggered": 100000,
        "on": false
    },
    "mqtt": {
        "discoveryPrefix": "homeassistant",
        "enabled": false,
        "mqttPassword": "",
        "mqttPort": 1883,
        "mqttServer": "mqtt",
        "mqttUser": ""
    }
   },
}
```

## Zigbee2mqtt

[Zigbee2mqtt](https://zigbee2mqtt.io) is a really nice application that uses a (pretty) cheap zigbee device and lets you control it from a mqtt server. To use zigbee2mqtt with diyHue, you'll need to make sure the you turned on `homeassistant` in the [configuration](https://www.zigbee2mqtt.io/information/configuration.html). This will make zigbee2mqtt start sending the auto discovery messages.

```yaml
homeassistant: true
```

By using zigbee2mqtt and a compatible zigbee device together with diyHue you could replace the original bridges for zigbee lights. Hue bridge and/or TradFri.

## Other mqtt lights

We have only tested it with zigbee2mqtt, but other lights that are controlled by mqtt can maybe also work. You have to publish the correct auto discovery message to mqtt for diyhue to be able to pick it up.

### Protocol

Auto discovery messages look like this. Topic `homeassistant/light/{light-id}/light/config`

```json
{
  "brightness" : true,
  "xy" : true,
  "schema" : "json",
  "command_topic" : "zigbee2mqtt/naam-van-lamp/set",
  "state_topic" : "zigbee2mqtt/naam-van-lamp",
  "json_attributes_topic" : "zigbee2mqtt/naam-van-lamp",
  "name" : "naam-van-lamp_light",
  "unique_id" : "0x0017xxxec5e_light_zigbee2mqtt",
  "device" : {
    "identifiers" : [ "zigbee2mqtt_0x0017xxxec5e" ],
    "name" : "naam-van-lamp",
    "sw_version" : "Zigbee2mqtt 1.11.0",
    "model" : "Hue Bloom (7299760PH)",
    "manufacturer" : "Philips"
  },
  "availability_topic" : "zigbee2mqtt/bridge/state"
}
```

diyHue subscribes to `{discoveryPrefix}/light/+/light/config` and uses the `brightness`, `ct` and `xy` values to determine the light type. The `state_topic` is used to get data from the light and the `command_topic` is used to send a new state to the light.

Sample state message, topic: `zigbee2mqtt/naam-van-lamp`

```json
{
  "state" : "ON",
  "brightness" : 200,
  "color" : {
    "x" : 0.7006,
    "y" : 0.2993
  }
}
```

The light is controlled by sending a message like the state message to the `command_topic` like `zigbee2mqtt/naam-van-lamp/set`.

```json
{
  "state" : "ON",
  "brightness" : 200,
  "ct": 200,
  "color" : {
    "x" : 0.7006,
    "y" : 0.2993
  }
}
```

MQTT lights are only available since [this pr](https://github.com/diyhue/diyHue/pull/350), so no support for sensors yet. But you can always contribute to this feature.
