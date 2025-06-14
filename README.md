# MQB Cluster Mileage Roller — Free Tool to Adjust Mileage in VAG Clusters (Passat, Golf, Octavia) [Open Source]
A project that turns an Arduino Nano with an MCP2515 into a travel emulator.

## Purpose
As there is no official way to adjust mileage on MQB platform instrument clusters that have been used before (have mileage above 1000km), this tool will allow you to increase the mileage to match your original one after fitting a used part in your car. This will allow you to display correct mileage during resale and pass the MOT without explanations and extra paperwork. It is supposed to be as lightweight as possible, with no running computer. The whole logic is on a cheap Arduino Nano with minimal consumption. The backlight is automatically dimmed when the tool is running.

## What this utility CANNOT do?
- Most prominently, roll the mileage back. I have no knowledge or desire to implement this feature. Please do not ask for it.
- Set the mileage straight away. While it would be nice, this is beyond my capabilities
- Work on other vehicle platforms. It is designed and tested specifically around the VW MQB platform. It MAY work on other platforms, but it will likely need to be adjusted. I have no equipment to perform tests on anything else.

## What would I need to use it?
- Arduino nano, whether genuine or clone
- MCP2515 CAN bus module
- 12V power supply to power up everything
- A PC with a suitable compiler to program the Arduino, you can get Arduino IDE here: https://www.arduino.cc/en/software

- Alternatively to Arduino nano and MCP2515 shield, you can use CANduino, which basically integrates both boards into a single one.

## How to use it?
First of all, wire up your Arduino with the MCP2515 module:
![](https://europe1.discourse-cdn.com/arduino/original/4X/b/a/1/ba15508f980ed52b6d1021f23f05c1b2498698c7.jpeg)

The next step is to program your Arduino using the sketch from this repository. Check what crystal your CAN module uses (mine is 8MHz), if needed, adjust it in the code (search for "CAN.begin"). Then program the Arduino.
Mine, and I believe all MQB clusters use an 18-pin connector. Pinout is following:
1) KL30a - Here comes positive lead from your 12V PSU
10) KL31 - Here comes negative lead from your 12V PSU
17) CAN-L - Here comes the CAN-L bus from the CAN bus module
18) CAN-H - Here comes the CAN-H bus from the CAN bus module

You also need to power your Arduino. For testing and configuration, you can run it off your PC's USB port, but you probably want to power it from your 12V PSU too. Connect 12V to the VIN pin on your nano, and GND to the GND pin next to it.
Note on the power supply: Arduino Nano uses 5V as a power supply. For situations when both USB and VCC is connected, to protect the USB port, VCC should be 6V or greater. Due to the fact that Arduino Nano stabilises the voltage using a linear regulator, using voltages over 9V is generally not recommended, as the regulator works by dissipating voltage as a heat. In our use case, with just CAN bus module connected, this is not a concern, as the power draw is way below 100mA - we can afford to power the Nano from 12V power supply directly without adding extra complexity or a dedicated PSU for Nano.

Now you can configure your Arduino. This is done via its serial port. You can use PuTTY or a serial monitor within Arduino IDE (baud rate 115200). Arduino will also report the current mileage there.

Once all of the above is done, you can plug your PSU into a power outlet, sit back and relax.

If you need to interrupt the job for any reason, the Arduino will remember its configuration and resume the operation once powered up. This is handy if you need to roll a substantial distance on your odometer and also need to use your car from time to time. Note that (at least for now) the Arduino doesn't remember the last state, so you may need to adjust the target mileage later depending on the distance your car actually travelled.

## Credits
Most of the heavy work and research used here is just a copy-paste of an excellent project [r00li/CarCluster/](https://github.com/r00li/CarCluster/), which aims to utilise actual vehicle clusters as an interface to display in-game data. I merely took all the research done there and created a skeleton code with hardcoded values, rather than obtaining them from a game.
Without r00li's work, this little tool would take much more time than a single evening, or may not have even existed.

## License
This work is licensed under GNU GPL version 3. See LICENSE file for more details
