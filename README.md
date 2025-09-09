# OpenSpeedo
A solution for CAN (bus) based ECUs to display their live data on a user-configurable dashboard. Doubles as an ADC extender for aftermarket ECUs. Built for Raspberry Pi GPIO (non-Pico) & written in Python for compatibility.


## Hardware Requirements
- Decent Raspberry Pi capable of supporting several hats. I used a Pi Zero 2 W. Buy genuine - these are too cheap to need to save a few cents on a clone. 
- Waveshare RS485 CAN hat (or equivalent). Found here: https://www.amazon.com/gp/product/B07DNPFMRW
- HDMI display compatible with your Rpi and powered by your vehicle. (Lower resolution = higher framerate, choose wisely. 720p recommended for now.) These are all over AliExpress. 
- USB car charger, or other 5V power supply (such as from ECU). This will power your Rpi and any other accessories. CANbus typically is paired with 12V battery voltage - do NOT use this unregulated. I recommend getting a trusted name-brand; don't skimp here. 
- (Optional) ADC hat that you will build yourself (instructions below)!
  
### Other Requirements
- Knowledge of your vehicle or ECU's addressing (header, bit length, etc)

## (Optional) Analog-Digital Converter (ADC)
This will enable you to send commands back down the CAN rail to expand the input sensor quantity of your aftermarket ECU. I'm using a Haltech 2500 Elite for testing. 

Needed:
- Knowledge of addressing for your ECU. Haltech offers their own expansion modules, so the example is preset to emulate these module addresses. 
- Up to **4x** ADS1115 boards. Readily found on AliExpress for under $2 a pop. This will get you up to 16 new inputs for your ECU (4 per board). It is important that these use I2C so that we can get the most from the SPI bus for the CAN board.

These boards support 4 different addresses, thus, 4 is the max without multiplexing. Luckily, it is easy and cheap to duplicate this project multiple times if you need more inputs and don't care about the gauge/display part. 
- Resistor divider network. Recommend 1% tolerance resistors or better. Needed for stepping down voltage inputs to 0 - 3.3V to not damage your ADC. 

The R values are up to you; I recommend planning for 18-20V spikes on a 12.8V battery; over 24V is overkill unless you have a special case: so that will be a factor of 0.165, or something like R1 = 10k; R2 = 2.2k to regulate up to 18V safely. I also recommend surface mount components, and I will be assuming you can solder. I will provide a PCB file that will assume one of the larger SMD sizes, like 2512 or similar (something a human can solder with tweezers). You will need a pair of resistors per input. 

### WARNING/DISCLAIMER
Using the ADC is dangerous if there is a fault in either hardware, or code, or power supply. By using this software, you acknowlege the risks and agree that I am not responsible for any damage caused. UNDERSTAND THE RISKS AND ALWAYS MONITOR YOUR VEHICLE. If something seems off, it probably is! Drop a comment if you have a concern or question!

That said, I recommend only using the inputs for non-critical applications. Examples of what NOT to use it for would include crank and cam shaft sensors, vehicle speed sensor (VSS), accelerator position sensor (e-throttle), throttle position sensor (TPS), etc - anything life threatening if it were to fail or misinterpret a read. 
