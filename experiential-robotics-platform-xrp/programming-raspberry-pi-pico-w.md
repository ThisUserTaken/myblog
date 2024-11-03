# Programming Raspberry Pi Pico W

### How to factory reset Raspberry Pi Pico W (not needed for Arduino)

Download the _**flash\_nuke.uf2**_ file to your computer from this link [https://datasheets.raspberrypi.com/soft/flash\_nuke.uf2](https://datasheets.raspberrypi.com/soft/flash\_nuke.uf2).

Press the BOOTSEL button on Pico and while keeping it pressed, connect it to your computer via a USB cable. The Pico should now appear as a mass storage device on your computer with the label RPI-RP2. Copy and paste the flash\_nuke.uf2 file into this new drive. Your Pico/Pico W will now restart and all the files in its Flash memory will be erased.



### Setting Up the XRP Bot Firmware for MicroPython

1.  **Download MicroPython for Raspberry Pi Pico:**

    Go to the MicroPython download page for Raspberry Pi Pico: [MicroPython for Pico](https://micropython.org/download/rp2-pico/) and download the latest version UF2 file.
2. **Load MicroPython onto Pico:**
   * Press and hold the BOOTSEL button on the Pico.
   * Connect the Pico to your computer using a USB cable.
   * Release the BOOTSEL button; the Pico should appear as a drive named `RPI-RP2`.
   * Drag and drop the MicroPython UF2 file ( [XRP Bot MicroPython](https://github.com/Open-STEM/XRPCode/tree/main/micropython)) onto the `RPI-RP2` drive. The Pico will reboot with MicroPython.

### Setting Up the XRP Robot for WPILib Usage

1. **Install WPILib:**
   * Download and install WPILib from the [WPILib Installation Guide](https://docs.wpilib.org/en/stable/docs/zero-to-robot/introduction.html).
   * Follow the platform-specific instructions for Windows, macOS, or Linux.
2. &#x20;Download the firmware
   * Download the firmware from [https://github.com/wpilibsuite/xrp-wpilib-firmware/releases/tag/v1.1.0](https://github.com/wpilibsuite/xrp-wpilib-firmware/releases/tag/v1.1.0) and use the same method as before to load it onto the Pico









