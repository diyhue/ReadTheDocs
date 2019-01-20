Uninstall
=========

Why on earth would you want to uninstall diyHue! All joking aside, uninstall is super easy if you need a fresh install, your moving over to new hardware or maybe diyHue just isnt for you.

Docker Install
--------------

You can easily remove the diyHue docker container and image with one command::

    sudo docker rm diyHue && sudo docker rmi diyhue/core

Host Install
------------

Automatic install
~~~~~~~~~~~~~~~~~

Just run the following command to uninstall::

    curl -s https://raw.githubusercontent.com/diyhue/diyHue/master/BridgeEmulator/easy_uninstall.sh | sudo bash /dev/stdin


This will stop diyHue, remove the astral python library, remove the diyHue files and remove the diyHue service.
It will leave the packages installed by diyHue in case they are used by other services. These packages are:

* unzip
* nmap
* python3
* python3-requests
* python3-ws4py
* python3-setuptools

Remove these at your own discression.
