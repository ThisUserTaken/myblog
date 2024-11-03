# Connect with Wifi

To connect a Raspberry Pi Pico W to Thonny using WebREPL, you need to go through a few setup steps. WebREPL is an interface that allows you to access and interact with your MicroPython device over Wi-Fi. Here’s a step-by-step guide to set this up:

#### Prerequisites

1. **MicroPython Firmware**: Make sure MicroPython is installed on your Raspberry Pi Pico W.
   * Download the MicroPython firmware for the Pico W from the official site and follow installation instructions if needed.
2. **Thonny IDE**: Ensure you have the latest version of Thonny installed (Thonny version 3.3.3 or later supports Pico).

#### Steps to Connect Pico W with WebREPL in Thonny

1. **Set Up Wi-Fi on the Pico W**:
   * Connect your Pico W to your computer via USB.
   * Open Thonny and select the Pico W as your MicroPython device in Thonny:
     * Go to **Tools > Options > Interpreter**.
     * Set the interpreter to **MicroPython (Raspberry Pi Pico)**.
     * Select the correct USB port where the Pico W is connected.
   *   Write and run the following code in Thonny to connect the Pico W to Wi-Fi:

       ```python
       pythonCopy codeimport network
       import time

       ssid = "your-SSID"  # Replace with your Wi-Fi name
       password = "your-PASSWORD"  # Replace with your Wi-Fi password

       wlan = network.WLAN(network.STA_IF)
       wlan.active(True)
       wlan.connect(ssid, password)

       # Wait until connected
       while not wlan.isconnected():
           print("Connecting to WiFi...")
           time.sleep(1)
           
       print("Connected to WiFi", wlan.ifconfig())
       ```
   * Run this code to confirm your Pico W can connect to Wi-Fi. The output should show the IP address once connected.
2. **Enable WebREPL on Pico W**:
   *   Write the following code in Thonny to enable WebREPL:

       ```python
       pythonCopy codeimport webrepl
       webrepl.start()
       ```
   * You’ll be prompted to set a password. This is for securing your WebREPL connection, so choose a strong password.
3. **Download and Install WebREPL Client on Your Computer**:
   * Download the WebREPL client provided by MicroPython.
   * Open the `webrepl.html` file in a web browser.
4. **Connect to Pico W Using WebREPL**:
   * In the WebREPL client in your browser, enter the IP address of the Pico W (as displayed in step 1) and the password set in step 2.
   * Click **Connect**. You should see a REPL prompt if the connection is successful.
5. **Connect Thonny to WebREPL**:
   * Open Thonny and go to **Tools > Options > Interpreter**.
   * Set the interpreter to **MicroPython (WebREPL)**.
   * In the **WebREPL address** field, enter `ws://<Pico_W_IP_address>:8266` (replace `<Pico_W_IP_address>` with the IP address displayed in step 1).
   * Enter the WebREPL password, then press **OK**.
6. **Test the Connection**:
   * You should now be able to run code on the Pico W via Thonny over Wi-Fi.
   * Try running a simple script like `print("Hello from WebREPL!")` to confirm the connection.

#### Troubleshooting Tips

* **Check IP Address**: Make sure the IP address of the Pico W hasn’t changed after each reboot.
* **WebREPL Not Starting**: If WebREPL doesn’t start, try restarting the Pico W and re-running the `webrepl.start()` command.
* **Connection Issues**: Ensure both the computer and the Pico W are on the same network.

This setup lets you interact with your Pico W over Wi-Fi, providing flexibility in working with wireless devices in MicroPython.
