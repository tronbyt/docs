# What is Tronbyt?

There was once a device called a Tidbyt, which was a nice pixel display board.
The company that made these devices was acquired.

Tidbyts require a server to work.
The server generates the image which is sent to the device.
The server that Tidbyt originally talked to is at risk of going down.
Additionally, the Tidbyt company ran an app store and a mobile app for managing your Tidbyt.

**Tronbyt** is a replacement server for the Tidbyt server and app store.
It can also manage other devices.

## Requirements

In order to use Tronbyt, you need:

**A running version of the Tronbyt server software** (called an "instance").
If the server is not running, the device will not update with new images.
This can be in the form of:

1. Software you run yourself at home on an always-on computer, like a Raspberry Pi (see [Device Specific Notes](device-notes.md)) or a PC that never turns off.
2. Software running on a hosting service on the internet, such as [Railway](https://railway.com/deploy/tronbyt).

**A device that can talk to the Tronbyt server and display images**.

Supported devices are:

- Tidbyt Gen 1
- Tidbyt Gen 2
- Raspberry Pi connected to a 64 x 32 matrix LED
- Tronbyt S3 (See [Device Specific Notes](device-notes.md) for additional configuration information)

This process will involve:

- Opening and executing commands via terminal (Windows/Mac)
- Installing drivers for USB firmware flashing
- Knowing the WiFi name and password for the network your device will be connecting to
- Some trial and error

