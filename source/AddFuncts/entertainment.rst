Hue Entertainment
=================

The DTLS server used by Hue Entertainment is a 3rd party service build in C. Precompiled binaries are provided for X86_64 and armv7 (Raspberry Pi 2 & 3) architectures. For any other architecture the service must be compiled from source. It would be good if the compiled executable could be uploaded with a pull request for other architectures like armv6 (Raspberry Pi 1)

Compilation Commands
--------------------

::

    wget https://github.com/ARMmbed/mbedtls/archive/1ab9b5714852c6810c0a0bfd8c3b5c60a9a15482.zip
    unzip 1ab9b5714852c6810c0a0bfd8c3b5c60a9a15482.zip
    cd mbedtls-1ab9b5714852c6810c0a0bfd8c3b5c60a9a15482/
    wget https://raw.githubusercontent.com/diyhue/diyHue/master/BridgeEmulator/ssl_server2_diyhue.c
    make no_test
    gcc -I./include ssl_server2_diyhue.c -o ssl_server2_diyhue -L./library -lmbedtls -lmbedx509 -lmbedcrypto

