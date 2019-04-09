Getting Started
===============

Setup is very quick and easy with two main installation methods currently supported. diyHue can be installed eaither directly on the host machine, or via a docker image. Currently, we recommend installation via Docker as it's much easier to maintain, update and modify!

Please note that although running diyHue on Windows is theoretically possible, many scripts and functions rely on Linux commands. As such, using Windows is not recommended! Installation on OpenWrt is also supported.

It is best to have at least one compatible light ready in order to setup and test the system with.

Docker Install
--------------

Currently the docker image has been tested with x86 systems and ARMv7 systems (Raspberry Pi 2 and later). Currently the ARM image has only been tested with a Raspberry Pi 3b+ If you have other ARM based devices and can test the image, please let us know on our Slack chat or in an issue. The images can be run with both host and bridge network modes. I recommend using the host network mode for ease, however this will give you less control over your docker networks. Using bridge mode allows you to control the traffic in and out of the container but requires more options to setup.

To run the container with the host network mode::

    docker run -d --name "diyHue" --network="host" -v '/mnt/hue-emulator/export/':'/opt/hue-emulator/export/':'rw' diyhue/core:latest

When running with the bridge network mode you must provide the IP and MAC address of the host device. Four ports are also opened to the container. These port mappings must not be changed as the hue ecosystem expects to communicate over specific ports.

To run the container with bridge network mode::

    docker run -d --name "diyHue" --network="bridge" -v '/mnt/hue-emulator/export/':'/opt/hue-emulator/export/':'rw' -e MAC='XX:XX:XX:XX:XX:XX' -e IP='XX.XX.XX.XX' -p 80:80/tcp -p 443:443/tcp -p 1900:1900/udp -p 2100:2100/udp -p 1982:1982/udp diyhue/core:latest

These commands will run the latest image available, however if you have automated updates enabled with a service such as `watchtower <https://github.com/v2tec/watchtower>`_, then using latest is not recommended. The images are automatically rebuilt upon a new commit to this repo. As such, larges changes could occur and updates will be frequent. Each image is also tagged with the comit hash. For example ``diyhue/core:391cc642072aac70d544fd428864f74bf9eaf636``. It is then suggested you use one of these images instead and manually update every so often.

The mount directory ``/mnt/hue-emulator/export/`` can be changed to any directory you wish. Backups of the config.json and cert.pem are saved here when changes are made to these files. They are then restored upon container reboot. If you need to make manual changes to these files, do so with the files mounted on the host (rather than the files in the container) and then restart the container to import your changes. To perform a manual export at any time, visit ``http://{emualtor ip}/save``. If there are no files in the mounted directory then they will be regenerated at container start.

Host Install
------------

When installing diyHue directly on the host, you have two installation methods available. An easy automatic installation script or the commands to install it manually. The automatic install is highly recommended and is kept most up to date.

Automatic install
~~~~~~~~~~~~~~~~~

Just run the following command to install::

    curl -s https://raw.githubusercontent.com/diyhue/diyHue/master/BridgeEmulator/easy_install.sh | sudo bash /dev/stdin


Once complete, diyHue is installed and running. It will also automatically start on boot. diyHue can also be stopped, started and restarted with the following command::

    sudo systemctl [start/stop/restart] hue-emulator.service

Manual install
~~~~~~~~~~~~~~

* Download or clone the mirror with git (recommended) ``git clone https://github.com/diyhue/diyHue.git``
* Install nmap package for lights autodiscover ``sudo apt install nmap``. On windows nmap utility is missing and the lights must be added manually in configuration witch is not simple.
* Create startup systemd file based on the following example::

    sudo nano /lib/systemd/system/hue-emulator.service

* Paste the following code and edit the path of the emulator script::

    [Unit]
    Description=Hue Emulator Service
    After=multi-user.target

    [Service]
    Type=idle
    Restart=always
    RestartSec=30
    StartLimitInterval=200
    StartLimitBurst=5

    WorkingDirectory=/home/pi
    ExecStart=/home/pi/HueEmulator.py

    [Install]
    WantedBy=multi-user.target

* Save and execute the following commands::

    sudo chmod 644 /lib/systemd/system/hue-emulator.service
    sudo systemctl daemon-reload
    sudo systemctl enable hue-emulator.service
    sudo systemctl start hue-emulator.service

If you want to disable logging to syslog you must add in systemd file ``StandardOutput=null``.
you can check the service status with ``sudo systemctl status hue-emulator.service``

OpenWrt Install
---------------

First, run following command::

    opkg update && opkg install wget ca-bundle nano

You will need to change to the temporary directory::

    cd /tmp

It is also necessary to change 3 lines of code from port 80 to 82::

    nano /etc/config/uhttpd

Change... ::

    list listen_http	0.0.0.0:80
    list listen_http	[::]:80

to... ::

    list listen_http	0.0.0.0:82
    list listen_http	[::]:82


and also::

    nano /etc/lighttpd/lighttpd.conf

Change this... ::

    server.port = 80

to this... ::

    server.port = 82


Finally, run the following command to run the install::

    wget --no-check-certificate https://raw.githubusercontent.com/diyhue/diyHue/master/BridgeEmulator/easy_openwrt.sh && sh easy_openwrt.sh

The installation in OpenWrt requires a change to the configuration file for the GUI of luci since it runs on port 80 by default, and diyHue must run on port 80, so it was changed to port 82 following the instructions above. Therefore to enter the OpenWrt configuration you must access: ``http://192.168.8.1:82/cgi-bin/luci`` instead.

Demo
~~~~

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="https://www.youtube.com/embed/JL5JlRtZFKY" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>
