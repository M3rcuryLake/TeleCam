# TeleCam

This project integrates an ESP32-CAM module and a PIR motion sensor (HC-SR501) to develop a motion-activated camera system with Telegram bot integration for remote control and notifications. The system employs the **Universal Telegram Bot Library** and **ArduinoJson** for interfacing with the Telegram API.  


### _Setup:_
- **Setting Up Network Credentials** : Insert your network credentials in the following variables 
  ```
  const char* ssid = "*******"; 
  const char* password = "*********";
  ```

- **Setting Up Telegram :** Create a new bot in telegram with [BotFather](t.me/botfather) with `/newbot` and follow the instructions to create your bot. Give it a name and username. If your bot is successfully created, you’ll receive a message with a link to access the bot and the bot token. Save the bot token because you’ll need it so that the ESP32 can interact with the bot.
In your Telegram account, search for “IDBot” or open this link t.me/myidbot in your smartphone.
Start a conversation with that bot and type `/getid`. You will get a reply back with your user ID, save that for later.
now change the Bottoken and ChatID variables with your own Telegram bot token and ChatID

    ```
    String BOTtoken = "********:***************************";
    String CHAT_ID = "*********";
    ```


- **Pinout and Interfacing:** Now follow the given pinout table to properly interface the ESP32-CAM with HC-SR501 using a FT232RL FTDI to TTL Serial Converter Module (remember that one jumper will be present in the FT232RL, blocking the 3.3V PIN and another at the PIR HC-SR501 at the 'H' position enabling multiple trigger mode, blocking the 'L' connection)

    |ESP-32 CAM |FT232RL    |
    |--------   |-------    |
    |5V         |5V         |
    |RX         |UOT        |
    |TX         |UOR        |
    |GND        |GND        |
    
    |ESP-32 CAM |PIR (HC-SR501) |
    |-------    |-------        |
    |3.3V       |VCC            |
    |GPIO13     |OUT            |
    |GND        |GND            |
    
    |ESP-32 CAM |ESP-32 CAM |
    |-------    |-------        |
    |GND       |GPIO0           |
    

- **Installing Requirements** : To interact with the Telegram bot, we’ll use the Universal Telegram Bot Library created by Brian Lough which provides an easy interface for the Telegram Bot API. The ArduinoJSON library will be installed automatically with the Universal Telegram Bot Library, if not, you will also have to install the ArduinoJson library. You can install both of them through library through the Arduino Library Manager.

    
- **Final Steps :** Flash the code with Arudino IDE, then remove the `GND GPIO0` Jumper Wire. Tune in to the 115200 baud band and press the `RST` button on the ESP32-CAM. After 20 seconds of warming up the surveillance setup should be up and running.
To communicate with the ESP32-CAM, send a `/init` to the Telegram Bot we made earlier. It must output the following :
   ```
   /init: Displays a help menu to guide users.  
   /start: Activates the camera server, enabling motion detection and image capture when movement is detected.  
   /stop: Halts the camera server to conserve resources.  
   ```

**Hardware Configuration**:  
- **PIR Sensor**: Configured with minimal sensitivity and delay in multiple trigger mode (H mode). Its output is connected to GPIO13 with a 45kΩ internal pulldown resistor with `pinMode(pin, mode);` where mode is `INPUT_PULLDOWN`  ([more info here](https://docs.espressif.com/projects/arduino-esp32/en/latest/api/gpio.html#pinmode)) to ensure stable low signals in idle states.  
- **Status Indicators**:  
  - GPIO33 controls a red LED to indicate active operation.  
  - GPIO4 flashes the built-in flashlight, mimicking a camera torch, during image capture.  

**Software Dependencies**:  
The system leverages critical libraries such as `"wifi.h"` for connectivity and `"esp_camera.h"` for camera operations.  

This project provides a simple and efficient solution for remote motion detection and image capture using a Telegram bot interface, offering practical applications in home security and automation.


_Disclaimer : Made as a first semester ECE project, sorry for any errors caused within. Thanks._

