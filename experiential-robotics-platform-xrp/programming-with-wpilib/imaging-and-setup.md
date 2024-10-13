# Imaging and Setup



* ### Imaging your XRP

The XRP uses a Raspberry Pi Pico W as its main processor. A special firmware will need to be installed so that the robot operates properly.

#### Download

The XRP firmware must be downloaded and written to the Pico W. Click on `Assets` at the bottom of the description to see the available image files:

[XRP-WPILib Firmware](https://github.com/wpilibsuite/xrp-wpilib-firmware/releases)

#### Imaging

To image the XRP, perform the following steps:

1. Extract the contents of the firmware ZIP file. You should end up with a `.uf2` file.
2. Plug the XRP into your computer with a Micro-USB cable. You should see a red power LED that lights up.
3. While holding the `BOOTSEL` button (the white button on the green Pico W, near the USB connector), quickly press the reset button (circled below), and then release the `BOOTSEL` button.

![XRP Reset button](https://docs.wpilib.org/en/stable/\_images/xrp-reset-button.png)

4. The board will temporarily disconnect from your computer, and then reconnect as a USB storage device named `RPI-RP2`.
5. Drag the `.uf2` firmware file into the `RPI-RP2` drive, which will automatically update the firmware.
6. Once complete, the `RPI-RP2` USB storage device will disconnect. You can disconnect the XRP board from your computer and run it off battery power.

#### First Boot

Perform the following steps to get your XRP ready for use:

1. Ensure that you have 4 AA batteries installed
2. Turn the XRP on by sliding the power switch (circled below) on the XRP board to the on position. A red power LED will turn on.

![XRP Power switch](https://docs.wpilib.org/en/stable/\_images/xrp-power-switch.png)

3. Using your computer, connect to the XRP WiFi network using the SSID `XRP-<IDENT>` (where `<IDENT>` is based on the unique ID of the Pico W) with the WPA2 passphrase `xrp-wpilib`.

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption><p>This is what the WiFi of the XRP robot looks like.</p></figcaption></figure>

### Booting up the XRP

Upon start-up (when power is applied to the XRP either via battery or USB), the following will happen:

1. The IMU will calibrate itself. This lasts approximately 3-5 seconds and will be indicated by the green LED blinking rapidly. Ideally, the XRP should be placed on a flat surface before powering up, and if necessary, users can hit the reset button to restart the firmware and IMU calibration process.
2. The network will be configured, depending on the configuration settings. See the section on [the Web UI](https://docs.wpilib.org/en/stable/docs/xrp-robot/web-ui.html) for more information on how to configure the network settings. By default, the XRP will broadcast its own WiFi Access Point.
3. After this, the XRP is ready for use.

###

### The XRP Web GUI

The XRP provides a simple Web UI for configuration. It is accessible at `http://<IP Address of XRP>:5000`. By default, this is `http://192.168.42.1:5000`.

The XRP configuration is a simple JSON object that allows a user to configure the network settings of the XRP.

![XRP Web UI JSON configuration](https://docs.wpilib.org/en/stable/\_images/xrp-webui-json.png)

### Switching Network Modes

Box 3 in the image above shows the field that needs to be changed in order to switch the XRP from Access Point mode to/from Station mode. In Access Point (`AP`) mode, the XRP will broadcast a WiFi network. In Station (`STA`) mode, the XRP will connect to an existing WiFi network. Update the `mode` field with the appropriate value (`AP`/`STA`).

### Setting up a default Access Point (AP)

By default, the XRP will operate in Access Point mode, where it broadcasts a WiFi network. Box 1 in the image above shows which fields control the settings for the AP SSID and passphrase.

If the operating mode is set to`AP`, the access point information will be used to create the WiFi Access Point. If the mode is set to `STA` (station) and the XRP is unable to connect to any of the listed WiFi networks, then it will fall back to AP mode, again, using the information specified in box 1.

### Connecting to an existing WiFi network

Box 2 in the image above shows an example of listing a WiFi network that you want the XRP to connect to. the `networkList` array can be populated with as many preferred networks as you would like (following the same format as Box 2). When set to `STA` mode, the XRP will attempt to connect to each listed network in order. If none of the networks are available, the XRP will fall back into AP mode.

Note

If you are unsure about what mode the XRP is operating in, or which WiFi network it is connected to, you can connect the XRP to a computer via a USB cable. A USB storage device named `PICODISK` will appear, and the `xrp-status.txt` file within it will list the appropriate network information.

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt="" width="192"><figcaption></figcaption></figure>

now [http://192.168.68.125:5000/](http://192.168.68.125:5000/)

<figure><img src="../../.gitbook/assets/image (5).png" alt="" width="431"><figcaption></figcaption></figure>

