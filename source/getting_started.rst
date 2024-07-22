Getting Started
===============

Setup is very quick and easy with two main installation methods currently supported. diyHue can be installed either directly on the host machine, or via a docker image. Currently, we recommend installation via Docker as it's much easier to maintain, update and modify!

.. note::
    While the host and OpenWrt installation methods are currently supported, there are plans to depreciate these installation methods in the future. They will continue to be available, however, they will be community maintained instead of offically supported. For all new installations, we highly recommend using the Docker installation method.

Please note that although running diyHue on Windows is theoretically possible, many scripts and functions rely on Linux commands. However, with Docker this is possible! Installation on OpenWrt is also supported.

It is best to have at least one compatible light ready in order to setup and test the system with.

Docker Install
--------------

Currently the docker image has been tested with x86 systems and ARMv7 systems (Raspberry Pi 2 and later). Currently the ARM image has only been tested with a Raspberry Pi 3b, 3b+, 4b and 5 If you have other ARM based devices and can test the image, please let us know on our Slack chat or in an issue. The images can be run with both host and bridge network modes. I recommend using the host network mode for ease, however this will give you less control over your docker networks. Using bridge mode allows you to control the traffic in and out of the container but requires more options to setup.

Install docker + docker compose on raspberry(this might take some time)::
    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh
    sudo apt-get -y install libffi-dev libssl-dev python3-dev python3 python3-pip

To find the IP and MAC of your raspberry in terminal type ``ifconfig``.
Find your active connection and next to ``ether`` is your MAC address and next to ``inet`` is your IP-Address.

To run the container with the host network mode::

    docker run -d --name diyHue --restart=always --network=host -e MAC=XX:XX:XX:XX:XX:XX -v /mnt/hue-emulator/config:/opt/hue-emulator/config diyhue/core:latest

.. note::
    Edit -e MAC=XX:XX:XX:XX:XX:XX to your specific Interface Mac you are using to connect to your network! This has to be correct to generate a valid certificate for genuine Hue App connection


To run the container with bridge network mode::

    docker run -d --name diyHue --restart=always --network=bridge -v /mnt/hue-emulator/config:/opt/hue-emulator/config -e MAC=XX:XX:XX:XX:XX:XX -e IP=XX.XX.XX.XX -p 80:80/tcp -p 443:443/tcp -p 1900:1900/udp -p 2100:2100/udp -p 1982:1982/udp diyhue/core:latest

.. note::
    When running with the bridge network mode you must provide the IP and MAC address of the host device. Four ports are also opened to the container. These port mappings must not be changed as the hue ecosystem expects to communicate over specific ports.

These commands will run the latest image available, however if you have automated updates enabled with a service such as `watchtower <https://github.com/v2tec/watchtower>`_, then using latest is not recommended. The images are automatically rebuilt upon a new commit to this repo. As such, larges changes could occur and updates will be frequent. Each image is also tagged with the commit hash. For example ``diyhue/core:391cc642072aac70d544fd428864f74bf9eaf636``. It is then suggested you use one of these images instead and manually update every so often.

Alternative tags are also available. Images are tagged as follows:

* ``branch-commitid`` : This will be a particular commit from the specified branch.
* ``branch`` : This is always the latest image from that particular branch.
* ``runid`` : This can come from any branch. The run id is determined from our `Github Actions page <https://github.com/diyhue/diyHue/actions>`_ which will indicate the commit, branch, and run id of this image.
* ``latest`` : This will always be the latest update from the ``master`` branch.

The mount directory ``/mnt/hue-emulator/config/`` can be changed to any directory you wish. Backups of the config.json and cert.pem are saved here when changes are made to these files. They are then restored upon container reboot. If you need to make manual changes to these files, do so with the files mounted on the host (rather than the files in the container) and then restart the container to import your changes. To perform a manual export at any time, visit ``http://{emualtor ip}/save``. If there are no files in the mounted directory then they will be regenerated at container start.

To update the container:

* Delete the existing container (Don't worry! This won't delete your configuration as this is stored in the mounted directory.)

.. code-block:: bash

    docker rm -f diyHue

* Recreate the container using the commands above, changing the tag if necessary.

Alternatively, you may utilize the `docker-compose <https://github.com/diyhue/diyHue/blob/master/.build/docker-compose.yml>`_ file found in the .build directory of the repository to setup diyHue.
In this file change the values to your setup.(IP, MAC, network mode, time zone)
Place this file in a folder where you can find it and this is the only docker compose file.
Once copied and in the folder where the file is located, it may be started using ``sudo docker compose up -d`` and stopped using ``sudo docker compose down``.
For older docker compose versions use ``sudo docker-compose up -d`` and ``sudo docker-compose down``
Updates can be achieved by changing the tag in the configuration and restarting ``sudo docker compose restart`` or ``sudo docker-compose restart``.

The container will auto-start on host boot or a container crash.


Beta install
~~~~~~~~~~~~~~~~~

Startin from 2022/03/23 we merged all current development branches (Beta) into a stable "Master" release.
Host and Docker install methods are updated and are now using the latest version of diyhue.

.. note::
    If you have currently a "old" diyhue image running (without the new UI) you have to start from scratch as the changes are huge and even config has a        complete new structure internally. We hope you enjoy the new Look and feel of diyhue and pardon the fact that you have to start over.



Home Assistant Addon
--------------------

Installation

NOTE: Installation will take some Minutes! Be Patience =)

The installation process is pretty easy and straight forward, like for any other third-party Home Assistang Add-on.

Add the repository URL under **Supervisor → Add-on store → ⋮ → Manage add-on repositories**:

    https://github.com/diyhue/hassio-addon


.. note::
    After Install & before starting diyHue, edit the Config according to the Docs below. If you start diyHue with the default MAC or a wrong MAC the Certificate for Linking    Official Apps is generated wrong and you get a error upon linking.

In that case:

- Stop diyHue
- delete cert.pem in /config/diyhue dir
- edit mac
- restart diyHue

## Configuration

**Note**: Remember to restart the add-on when the configuration is changed.

Example add-on configuration:

```yaml
config_path: /config/diyhue
mac: "XX:XX:XX:XX:XX:XX"
debug: true
no-serve-https: false
deconz_ip: 192.168.0.0
```

**Note**: This is just an example, don't copy and paste it! Create your own!

*Option:* `config_path`

The `config_path` option controls the folder where your diyHue config gets stored. It has to start with **/config** and i highly recommend to name the folder **/config/diyhue**.

*Option:* `mac`

The mac-address of your interface (device) you use to connect to you network.
**You can not fake a Mac here, since it is used for original software (APP) to authenticate the Emulated Bridge!**

**Note**: You have to stick to this format `XX:XX:XX:XX:XX:XX`.

*Option:* `debug`

If you turn the debug option to true you will get extended logs in the output section of the add-on.

Valid values: `false`, `true`.

*Option:* `no-serve-https`

You have to set this value to true if you are running hassio under https. Hassio doesn't allow the usage of SSL on the websocket at the moment. So you have to force diyhue to not use https.

Valid values: `false`, `true`.

*Option:* `deconz_ip`

Here you can enter the IP-Address of your Deconz instance.


Sidepanel Link

Add the following to your HA _configuration.yaml_ to get a Navigation Link in the Sidepanel

.. code-block:: yaml
    
	panel_iframe:
	  diyhue:
	    title: "diyHue"
	    icon: mdi:home-lightbulb
	    url: "http://homeassistant:80"

Host Install
------------

When installing diyHue directly on the host, you have two installation methods available. An easy automatic installation script or the commands to install it manually. The automatic install is highly recommended and is kept most up to date.

Automatic install
~~~~~~~~~~~~~~~~~

Just run the following command to install::

    curl -fsSL https://get.diyhue.org -o install.sh
    sudo bash install.sh
.. curl -s https://raw.githubusercontent.com/diyhue/diyHue/master/BridgeEmulator/easy_install.sh | sudo bash /dev/stdin

.. note::
    Choose Branch to be installed:
    Master (recommended for normal usage)
    Dev (latest features and fixes)
    Beta (Work in Progress - latest API implementation)



Choose your Network Interface (if you have multiple)

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

    wget --no-check-certificate https://raw.githubusercontent.com/diyhue/diyHue/master/BridgeEmulator/install_openwrt.sh && sh install_openwrt.sh

The installation in OpenWrt requires a change to the configuration file for the GUI of luci since it runs on port 80 by default, and diyHue must run on port 80, so it was changed to port 82 following the instructions above. Therefore to enter the OpenWrt configuration you must access: ``http://192.168.8.1:82/cgi-bin/luci`` instead.

OpenWrt Update
---------------

Run the following command to update::

    cd /tmp && wget --no-check-certificate https://raw.githubusercontent.com/diyhue/diyHue/master/BridgeEmulator/update_openwrt.sh && sh update_openwrt.sh

After the update has finished, the system will restart automatically.

Demo
~~~~

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="https://www.youtube.com/embed/JL5JlRtZFKY" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>
