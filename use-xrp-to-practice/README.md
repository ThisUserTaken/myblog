# Use XRP to practice

### Program RP2040 with Arduino IDE

First, install the Arduino IDE. Then, navigate to **File -> Preferences** and paste the link below into **Additional Board Manager URLs**. If the field is initially blank, just paste the link in and press **OK**. If there are already one or more URLs there, add a comma to the last one paste the link there, and press **OK**.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

The link to copy and paste: https://github.com/earlephilhower/arduino-pico/releases/download/global/package\_rp2040\_index.json&#x20;

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Click “OK” to save these preferences. Then, go to **Tools -> Board -> Board Manager** and type **pico** into the search bar, and hit enter. Select **Raspberry Pi Pico/RP2040** by **Earle F. Philhower, III** and press **Install**. Then press **close** and you should be all set to connect your RP2040.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Now that you've successfully installed the core, you can move on to connecting your RP2040 to the Arduino IDE.

Then in the Arduino IDE, go to **Tools -> Board -> Raspberry Pi RP2040 Boards** and select the board you are using&#x20;

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Note that the COM port number may be different and the interface will be different for macOS

XRP connections:&#x20;

{% embed url="https://docs.sparkfun.com/SparkFun_XRP_Controller/assets/hardware_files/XRP_Controller.pdf" %}

### XRP hardware connections

{% embed url="https://docs.sparkfun.com/SparkFun_XRP_Controller/hardware_overview/#solder-jumpers" %}

### Pinout Reference Table

The table below offers a quick reference for the complete pinout on the XRP Controller Board and which pins they connect to on the Pico W.

| Pico W GPIO Pin | Connector Label | Pin Function                                     |
| --------------- | --------------- | ------------------------------------------------ |
| GPIO0           | Motor 3         | Motor 3 Encoder A                                |
| GPIO1           | Motor 3         | Motor 3 Encoder B                                |
| GPIO2           | Motor 3         | Motor 3 Phase Pin                                |
| GPIO3           | Motor 3         | Motor 3 Enable Pin                               |
| GPIO4           | Motor L         | Left Motor Encoder A                             |
| GPIO5           | Motor L         | Left Motor Encoder B                             |
| GPIO6           | Motor L         | Left Motor Phase Pin                             |
| GPIO7           | Motor L         | Left Motor Enable Pin                            |
| GPIO8           | Motor 4         | Motor 4 Encoder A                                |
| GPIO9           | Motor 4         | Motor 4 Encoder B                                |
| GPIO10          | Motor 4         | Motor 4 Phase Pin                                |
| GPIO11          | Motor 4         | Motor 4 Enable Pin                               |
| GPIO12          | Motor R         | Right Motor Encoder A                            |
| GPIO13          | Motor R         | Right Motor Encoder B                            |
| GPIO14          | Motor R         | Right Motor Phase Pin                            |
| GPIO15          | Motor R         | Right Motor Enable Pin                           |
| GPIO16          | Servo 1         | Servo 1 Signal Pin                               |
| GPIO17          | Servo 2         | Servo 2 Signal Pin                               |
| GPIO18          | Qwiic           | Qwiic Data Signal for the IMU & Qwiic Connector  |
| GPIO19          | Qwiic           | Qwiic Clock Signal for the IMU & Qwiic Connector |
| GPIO20          | Range           | Range Trigger Pin                                |
| GPIO21          | Range           | Range Echo Pin                                   |
| GPIO22          | Extra           | User Button/Extra                                |
| GPIO26          | Line            | Line Follower Left Signal                        |
| GPIO27          | Line            | Line Follower Right Signal                       |
| GPIO28          | Extra           | VIN\_Meas/Extra                                  |



### Uploading a Sketch&#x20;

Make sure all your Arduino settings are correct and you've selected the correct serial port.

In the Arduino IDE, navigate to **File -> Examples -> Examples for Raspberry Pi Pico** and select the **Blink** example. Then press the upload button and your code should start running in a few seconds.
