# Tronbyt Manager

The homepage of Tronbyt Manager is where you can manage devices and add apps to your devices.
Each device has its own app list.
It's useful to remember that each app is sent to the device as an image file.

## Adding apps

Click "Add app" to see the app listings.
Click an entry to see any configuration options and Save to start showing it on your device.

## Device / App settings

Each device is listed by the Name property created when adding the device.

**Brightness**: How bright the device displays.
0 for off.
You can click "Edit device" to set up Night Mode, which allows for dimming at specific hours daily or to only show 1 specific app at night without rotating through other apps.

**App Cycle Time**: How long the device waits before asking for the next app.
If there's only one app, it is how long it waits before asking for the next image for that app.

**Auto Refresh**: This is a Tronbyt server setting and does not impact the device.
When on, it will render the current app image file in time with the device.
When off, it will allow the webpage's image file to grow stale, no longer matching the up-to-date image on the device.

**Enable/Disable**: Every app can be enabled or disabled.
Disabling the app prevents its code from running on the server.
It can still run if it is selected as the Night Mode app in device settings.

**Render interval**: How often the app is run to update the image.
Every time the app comes up to display, if the time elapsed is less than the render interval, then the image is reused without running additional app code.
This can result in stale data, which might be OK for some apps.
However, it's not ideal for things like clocks.
If the render interval is 0, the code is run every time the app is displayed, which is equal to (App Cycle Time * Number of apps enabled on device). So if you have 3 apps running with a cycle time of 45 seconds, the time elapsed between an app repeating is 45s.
However, the image sent by the server will be fresh if the app's render interval is 0 and it's the app's turn to display.
If you have 1 app running with a cycle time of 15s, the image will be updated every 15s.

## Admin Settings

**Create User**: The administrator can create new users, delete users, and update or change the system apps repo.
New users cannot do these things, but they can add devices and apps.

**System app repo**: This is the "master list" of apps.
Default is <https://github.com/tronbyt/apps>.

Refresh will refresh the app listings and code for the apps that are currently deployed.

**Custom app repo**: This is used to supplement the master list.
If you are developing a new app, you can point Tronbyt here and your new app will show up in the app listing without having to be accepted into the system app repo.
The repo should have apps in an `apps` subdirectory and each app in its own directory.
For an example, see [HomeAssistant-Tidbyt](https://github.com/motoridersd/HomeAssistant-Tidbyt).

Refresh will refresh the app listings and code for the apps that are currently deployed.

**API Key**: This can be used to allow other apps like HomeAssistant to control aspects of your account or devices.