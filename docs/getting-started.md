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

On the "Home" page, click "New Tronbyt".

Give it a name (this will be displayed on the Home screen), select the type of device you have, and hit "Save".

This will take you back to the Home screen. Select "Firmware".

Enter your WiFi network name and password.

!!! note

    If you have a Tidbyt Gen1, note the "Swap Colors" checkbox. Don't check it yet, but remember it's here.
    You might need to redo this step if the colors don't look right on your device.

Press "Generate Firmware file".
This will download a file with a name like `firmware_tidbyt_gen1_cfb4bf45.bin` to your downloads folder.

Download [ESP flasher](https://github.com/Jason2866/ESP_Flasher/releases).

**Windows:** Download [CP210x Drivers](https://www.silabs.com/developer-tools/usb-to-uart-bridge-vcp-drivers?tab=downloads)

1. Download the latest "Universal Windows Drivers" file. Find it in your downloads folder, right click and select "Extract all". This will create a new folder with the same name as the file.
2. Go into the newly created folder and right click `silabser.inf` and select "Install" from the menu options. (You might need to turn on [Show File Extensions](https://support.microsoft.com/en-us/windows/common-file-name-extensions-in-windows-da4a4430-8e76-89c5-59f7-1cdbbc75cb01) to see the `.inf`)

**Mac:** Download [CH34x Drivers](https://github.com/WCHSoftGroup/ch34xser_macos)

Connect your display device to a computer via USB.
It does NOT need to be a computer running the Tronbyt software.

Open ESP flasher.

Go through the different serial ports until you see something appear in the console.

Press "Browse" next to "Firmware" and select the firmware you downloaded from Tronbyt.

Press "Flash ESP".

It will take a minute or two, but eventually it will say "Done! Flashing is complete! Showing logs" and then a bunch more information will scroll past in the console window.
You might need to scroll up to confirm.

!!! tip

    If you have trouble with this step and you have a Tidbyt Gen 1, see [Error when trying to flash new firmware](device-notes.md#error-when-trying-to-flash-new-firmware).

Check your device! It should be displaying something now!

If you need to change your WiFi credentials or image URL after flashing, you can achieve this by using the WiFi config portal. This video shows how to do it: [Tronbyt WiFi Setup Portal](https://www.youtube.com/watch?v=OAWUCG-HRDs)
