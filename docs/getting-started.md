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

### Update firmware

1. On the "Home" page, click "New Tronbyt".
2. Give it a name (this will be displayed on the Home screen), select the type of device you have, and hit "Save".
3. Back on the Home screen, select "Firmware".
4. Enter your WiFi network name and password.

    !!! note

        If you have a Tidbyt Gen1, note the "Swap Colors" checkbox. Don't check it yet, but remember it's here.
        You might need to redo this step if the colors don't look right on your device.

5. Press "Generate Firmware file". This will download a file with a name like `firmware_tidbyt_gen1_cfb4bf45.bin` to your downloads folder.
6. Download [ESP flasher](https://github.com/Jason2866/ESP_Flasher/releases).
7. Install USB drivers if needed:

    **Windows:** Download [CP210x Drivers](https://www.silabs.com/developer-tools/usb-to-uart-bridge-vcp-drivers?tab=downloads). Extract the archive, then right click `silabser.inf` and select "Install". (You might need to turn on [Show File Extensions](https://support.microsoft.com/en-us/windows/common-file-name-extensions-in-windows-da4a4430-8e76-89c5-59f7-1cdbbc75cb01) to see the `.inf`)

    **Mac:** Download [CH34x Drivers](https://github.com/WCHSoftGroup/ch34xser_macos)

8. Connect your display device to a computer via USB. It does NOT need to be the computer running the Tronbyt software.
9. Open ESP flasher and go through the different serial ports until you see something appear in the console.
10. Press "Browse" next to "Firmware" and select the firmware you downloaded from Tronbyt.
11. Press "Flash ESP". It will take a minute or two, but eventually it will say "Done! Flashing is complete!"

    !!! tip

        If you have trouble with this step and you have a Tidbyt Gen 1, see [Error when trying to flash new firmware](device-notes.md#error-when-trying-to-flash-new-firmware).

12. Check your device! It should be displaying something now!

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
