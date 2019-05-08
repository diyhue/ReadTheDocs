Command Line Arguments
======================

diyHue has several options that can be passed in at run time. These options allow easy, and more complex configuration of diyHue.

IP
--
The IP argument can be used to provide the IP address of the host machine running diyHue. If you use this argument, you must also provide a value for the MAC argument. This argument is typically only used with docker.
If you are running diyHue directly on the host (easy install), then use the following format::

    --ip XX.XX.XX.XX

If you are running diyHue with our docker image, then add the following environment variable to your docker run command::

    -e "IP=XX.XX.XX.XX"

MAC
---
The MAC argument can be used to provide the MAC address of the host machine running diyHue. If you use this argument, you must also provide a value for the IP argument. This argument is typically only used with docker.
If you are running diyHue directly on the host (easy install), then use the following format::

    --mac XX:XX:XX:XX

If you are running diyHue with our docker image, then add the following environment variable to your docker run command::

    -e "MAC=XX:XX:XX:XX"

IP Range
--------
The IP Range argument can be used to set the IP range for light discovery. The default range is 0 to 255.
If you are running diyHue directly on the host (easy install), then use the following format::

    --ip_range <START_IP>,<STOP_IP>

If you are running diyHue with our docker image, then add the following environment variable to your docker run command::

    -e "IP_RANGE=<START_IP>,<STOP_IP>"

Deconz
------
The Deconz argument can be used to provide the IP address of your Deconz instance if it is not hosted on the same device as diyHue. 
If you are running diyHue directly on the host (easy install), then use the following format::

    --deconz XX:XX:XX:XX

If you are running diyHue with our docker image, then add the following environment variable to your docker run command::

    -e "DECONZ=XX:XX:XX:XX"

Debug
-----
For instructions on the debug argument please see :ref:`debug`