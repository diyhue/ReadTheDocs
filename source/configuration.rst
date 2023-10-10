.. _config:

Configuration
=============

Most configuration can be done either in the Hue app or in the diyHue web UI.

Pairing diyHue with the Hue app
-------------------------------

To pair a new device to diyHue, first login in the web interface and click the `Link Button` menu entry. The default username is `admin@diyhue.org` with a password of `changeme` also. At this point you should open the Hue app on your phone and start searching for hubs. To speed this up you can click the `Help` button and then enter the IP address of your diyHue device. Once the bridge has been detected, click the green `Set up` button. At this point, the app will prompt you to press the link button on your Hue bridge. To do so, click the `Link App` button on the web page you loaded at the start. The Hue app should now prompt you through the rest of the setup. If the Hue app asks you to update your bridge than you should change the software version of the DiyHue Bridge to the latest one. To do so you have to navigate to `Bridge` in the web interface on the navigation bar. Click on `check here for last version` and you will be navigated to the Philips Hue site where the version is located. Copy the latest version and paste it into the `Software Version` field and press `SAVE`. After that you can go back to the Hue app and just hit the update button and it will say that your bridge is on the latest version.

For specific details on how to setup specific lights, browse the lights section in the navigation bar to the left.

Configuration Files
------------------

The entire configuration is saved in dedicated yaml files. Changes to the bridge are saved when a new object is created or every hour automatically. In case of state change of lights or sensors the configuration is not saved to avoid extensive writes to the SD card on Raspberry Pi's. If you want to save the configuration manually, you can do so by accessing ``http://{emulator ip}/save``. If you want to manually edit the configurations file to change any kind of resource, I recommend backing up the files first. In case the configurations file are corrupted, you can look for backups that are done automatically every Sunday night.

Adding lights
-------------

Depending on which lights you plan to use, the setup method will vary. For detailed setup instructions, please chose the correct type of light you wish to setup from the menu on the left.



