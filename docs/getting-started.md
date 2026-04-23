---
icon: lucide/rocket
---
# Getting Started

First, you must deploy the Tronbyt server software.
Then, you must update the code on the device itself to talk to the Tronbyt server by connecting it to your computer via USB.
Finally, you will be able to configure Tronbyt to show the desired apps on your device.

## Install the server

### Docker (recommended)

Docker Compose is the recommended installation method.
See the [server repository](https://github.com/tronbyt/server) for the Compose file and full instructions.

```bash
docker compose up -d
```

For Raspberry Pi setup, see [Device Specific Notes](device-notes.md#raspberry-pi) or the [video walkthrough](https://youtu.be/UeHzD0uFxRo).

### Homebrew

```bash
brew install tronbyt-server
brew services start tronbyt-server
```

### Home Assistant

Install via the [Home Assistant add-on](https://github.com/kaffolder7/ha-app-tronbyt-server).

### From source

```bash
go build -o tronbyt-server ./cmd/server
./tronbyt-server
```

### Cloud hosting

You can also deploy to [Railway](https://railway.com/deploy/tronbyt) instead of self-hosting.

### Accessing the server

Once installed, access the web interface at `http://localhost:8000`.

If you're connecting from another device on your network, use the hostname or IP of the machine running Tronbyt followed by `:8000`.

If you see a "connection not secure" warning, that is expected for HTTP connections. Click through to proceed. See [Configuration](configuration.md#https-tls) for setting up HTTPS.

## Device Setup

After deploying the Tronbyt server, you will use it to generate firmware (code for the device) which will allow it to connect to your WiFi and therefore to the Tronbyt server.

### Logging in and changing the default password

Log in to your Tronbyt server with the username/password `Admin`/`password`.

!!! warning

    Change the password immediately by going into "Admin".

### Generate firmware

1. On the "Home" page, click "New Tronbyt".
2. Give it a name (this will be displayed on the Home screen), select the type of device you have, and hit "Save".
3. Back on the Home screen, select "Firmware".
4. Enter your WiFi network name and password.

    !!! note

        If you have a Tidbyt Gen1, note the "Swap Colors" checkbox. Don't check it yet, but remember it's here.
        You might need to redo this step if the colors don't look right on your device.

5. Press "Generate Firmware file". This will download a file with a name like `firmware_tidbyt_gen1_cfb4bf45.bin` to your downloads folder.

### Initial flash (USB)

After generating and downloading the firmware file:

1. Connect your device to your computer with a data USB cable
2. Open one of these web-based flashers in Chrome or Edge:
    - [Espressif Web Flasher](https://espressif.github.io/esptool-js/)
    - [ESPConnect Web Flasher](https://thelastoutpostworkshop.github.io/ESPConnect/)
3. Click Connect and select your device's serial port
4. Click Flash Tools if needed
5. Set the flash address to `0x0`
6. Select your downloaded firmware file and click Program/Flash
7. Reboot the device if it doesn't restart automatically

!!! note

    macOS and Windows users may need to install a serial driver:

    - [CP210x Drivers](https://www.silabs.com/developer-tools/usb-to-uart-bridge-vcp-drivers?tab=downloads)
    - [CH34x Drivers](https://github.com/WCHSoftGroup/ch34xser_macos)

!!! tip

    If you have trouble flashing a Tidbyt Gen 1, see [Error when trying to flash new firmware](device-notes.md#error-when-trying-to-flash-new-firmware).

Check your device! It should be displaying something now!

### Update firmware (OTA)

If your device is already running Tronbyt firmware, you can update it over the air. Click "Edit Device" on the Home screen and scroll to the "Firmware Update" section.

If you need to change your WiFi credentials or image URL after flashing, you can use the WiFi config portal. See the [Tronbyt WiFi Setup Portal](https://www.youtube.com/watch?v=OAWUCG-HRDs) video for a walkthrough.

## Updating

For Docker installations:

```bash
docker compose pull && docker compose up -d
```

For Homebrew installations:

```bash
brew upgrade tronbyt-server && brew services restart tronbyt-server
```
