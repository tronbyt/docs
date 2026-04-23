# Getting Started

There are multiple ways to run Tronbyt.
First, you must decide if you want to run this on your own computers ("self-hosting"), or if you want to use a free webservice to host for you.
Then, you must update the code on the device itself to talk to the Tronbyt server by connecting it to your computer via USB.
Finally, you will be able to configure Tronbyt to show the desired apps on your device.

## Deploy the server

The first step is to start running the Tronbyt code.
You need to decide on what computer this code will run.
You can choose a computer you own or you can use a free webserver from Render.com.

### Self-Hosting vs Webservice

You will have more control when self-hosting, but you must have a computer that will be on whenever you want to use the display device.
Using a free service might be easier initially. However, Render.com will reboot your Tronbyt instance at unpredictable times, which will lose your app configuration. You must remember to save your configuration and restore it when this occurs.

### Self-hosting

Running Tronbyt yourself requires some expertise, but nothing overly complex.
See the [self-hosting instructions](https://github.com/tronbyt/server?tab=readme-ov-file#prerequisites).
If you are planning to run Tronbyt on a Raspberry Pi, specific instructions can be found at [Device Specific Notes](device-notes.md#raspberry-pi).

Your server can be accessed by opening a web browser and typing:

- `localhost:8000` if you are on the computer running the code
- The name of the computer running the code followed immediately by `:8000` if you are running it on another computer in your network (such as a Raspberry Pi)

If you get a message like, "Your connection to this site is not secure" or "Your connection is not private", that is expected.
Click "Show advanced" and "Proceed (unsafe)".
This message is shown because there isn't proof your computer's identity has been verified, which is not needed in this specific case.

### Render.com Free server

[Video tutorial to deploy on Render.com](https://youtube.com/watch?v=2WfVaeX7BTk&feature=youtu.be)

!!! warning

    When using Render.com, the Tronbyt server will restart if a device doesn't talk to it for a long time.

    - This will erase your device and app list. Be sure to back them up by going into "Edit device" and "Export configuration" to save the configuration and "Restore configuration" to bring it back.
    - It can also restart if more than 500MB of memory is used, but this should be much rarer.

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
