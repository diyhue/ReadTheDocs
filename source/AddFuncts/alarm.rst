Alarm
=====

It is possible to receive email notifications when a motion sensor is triggered while the alarm is active. At this moment the alarm can be configured manually by editing the file /opt/hue-emulator/config.json while the service is stopped. You need to set just your email address and to switch the "on" value between true or false to enable / disable the alarm.

```
  "emulator": {
    "alarm": {
        "email": "example@gmail.com",
        "lasttriggered": 1585935069,
        "on": false
    }
  }
```
