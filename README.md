# hw2
1 Read all four of the Multitasking Resources in resources.

2 Read both of the Arduino Tone Resources in resources. Pay particular attention to section Code 3: Generating a sound with a button and Code 4: Sounds depending on different keys in the second resource.

3 Build the circuit described in section Code 4: Sounds depending on different keys of the second Arduino Tone Resource in resources and verify that it works properly.

The page you are looking for is temporarily unavailable.
Please try again later. 
16176c53160056e5a569c7babd419688 9758e8e4f46d90b48aac15822fa578ea 7f620722b24c8b0821770004ed5e73b6


4.Musical instrument project of any sort using Arduino:

Mug Music: Turn Water Into an Instrument with Arduino and ChucK
http://www.instructables.com/id/Mug-Music-Turn-Water-Into-an-Instrument-with-Ardui/


Illumaphone: Light-based Musical Instrument with Arduino
http://www.instructables.com/id/Illumaphone-Light-based-Electronic-Musical-Instrum/

Coke Piano and Launchpad
This project is sort of a two-in-one: two different applications that are based on the same concept. The gist of it is that you hook up a dozen or so aluminum cans to an Arduino, and each can produces a different sound or clip when touched.
https://youtu.be/Ttm62RBdOuo



4.Read about 3 of the sensors on the Adafruit Sensors guide listed in resources. Describe (briefly) what you've learned in your Github READ.md file.

Force sensitive resistor 
- Used to detect physical pressure such as pinching, squeezing, pushing, brushing

Size: 1/2" (12.5mm) diameter active area by 0.02" thick (Interlink does have some that are as large as 1.5"x1.5")
Price $7.00 from the Adafruit shop
Resistance range: Infinite/open circuit (no pressure), 100KΩ (light pressure) to 200Ω (max. pressure)
Force range: 0 to 20 lb. (0 to 100 Newtons) applied evenly over the 0.125 sq in surface area
Power supply: Any! Uses less than 1mA of current (depends on any pullup/down resistors used and supply voltage)


Photocells 
- Used to detect light/dark, breakbeams, simple object detection
Size: Round, 5mm (0.2") diameter. (Other photocells can get up to 12mm/0.4" diameter!)
Price: $1.00 at the Adafruit shop
Resistance range: 200KΩ (dark) to 10KΩ (10 lux brightness)
Sensitivity range: CdS cells respond to light between 400nm (violet) and 600nm (orange) wavelengths, peaking at about 520nm (green).
Power supply: pretty much anything up to 100V, uses less than 1mA of current on average (depends on power supply voltage)


Temperature
- Used to determine environmental temperature
These stats are for the temperature sensor in the Adafruit shop, the Analog Devices TMP36 (-40 to 150C). Its very similar to the LM35/TMP35 (Celsius output) and LM34/TMP34 (Farenheit output). The reason we went with the '36 instead of the '35 or '34 is that this sensor has a very wide range and doesn't require a negative voltage to read sub-zero temperatures. Otherwise, the functionality is basically the same.
Size: TO-92 package (about 0.2" x 0.2" x 0.2") with three leads
Price: $2.00 at the Adafruit shop
Temperature range: -40°C to 150°C / -40°F to 302°F
Output range: 0.1V (-40°C) to 2.0V (150°C) but accuracy decreases after 125°C
Power supply: 2.7V to 5.5V only, 0.05 mA current draw

Using the TMP36 is easy, simply connect the left pin to power (2.7-5.5V) and the right pin to ground. Then the middle pin will have an analog voltage that is directly proportional (linear) to the temperature. The analog voltage is independant of the power supply.

To convert the voltage to temperature, simply use the basic formula:
Temp in °C = [(Vout in mV) - 500] / 10
So for example, if the voltage out is 1V that means that the temperature is ((1000 mV - 500) / 10) = 50 °C
If you're using a LM35 or similar, use line 'a' in the image above and the formula: Temp in °C = (Vout in mV) / 10

Tilt sensors
- Used to detect motion/vibration and orientation.
These stats are for the tilt sensor in the Adafruit shop which is very much like the 107-2006-EV . Nearly all will have slightly different sizes & specifications, although they all pretty much work the same. If there's a datasheet, you'll want to refer to it
Size: Cylindrical, 4mm (0.16") diameter & 12mm (0.45") long.
Price: $2.00 at the Adafruit shop
Sensitivity range: > +-15 degrees
Lifetime: 50,000+ cycles (switches) 
Power supply: Up to 24V, switching less than 5mA


PIR sensors
- Used to detect motion activity such as animals or people
These stats are for the PIR sensor in the Adafruit shop which is very much like the Parallax one . Nearly all PIRs will have slightly different specifications, although they all pretty much work the same. If there's a datasheet, you'll want to refer to it
Size: Rectangular
Price: $10.00 at the Adafruit shop
Output: Digital pulse high (3V) when triggered (motion detected) digital low when idle (no motion detected). Pulse lengths are determined by resistors and capacitors on the PCB and differ from sensor to sensor.
Sensitivity range: up to 20 feet (6 meters) 110° x 70° detection range
Power supply: 5V-12V input voltage for most modules (they have a 3.3V regulator), but 5V is ideal in case the regulator has different specs

Thermocouple 
- Used for temperature measurements, usually those above 150°C
Size: 24 gauge, 1 meter long (you can cut it down if desired)
Price: $10 at the adafruit store
Temperature range: -100°C to 500°C / -150 to 900°F (After this the glass overbraiding may be damaged)
Output range: -6 to +20mV
Precision: +-2°C
Requires an amplifier such as MAX31855
Interface: MAX6675 (discontinued) MAX31855, or AD595 (analog)

IR receivers
- Used to detect IR signals from remote controls

Size: square, 7mm by 8mm detector area
Price: $2.00 at the Adafruit shop
Output: 0V (low) on detection of 38KHz carrier, 5V (high) otherwise
Sensitivity range: 800nm to 1100nm with peak response at 940nm. Frequency range is 35KHz to 41KHz with peak detection at 38KHz
Power supply: 3-5V DC 3mA
