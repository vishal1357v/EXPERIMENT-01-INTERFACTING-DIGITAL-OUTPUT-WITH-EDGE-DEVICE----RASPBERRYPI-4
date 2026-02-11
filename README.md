# EXPERIMENT-01- INTERFACTING DIGITAL OUTPUT WITH EDGE DEVICE---(RASPBERRYPI-PI4)
### NAME : VISHAL P
### DEPARTMENT : IOT
### REG NO : 212224230306
### DATE OF EXPERIMENT : 04.02.2026

### AIM
To interface a digital output device (LED) with the Raspberry Pi 4 and control it using Python.

## APPARATUS REQUIRED
Raspberry Pi 4
LED (Light Emitting Diode)
330Ω Resistor
IR Sensor
Breadboard
Jumper Wires
USB Cable
 ## THEORY

![Raspberry Pi Pin](https://github.com/user-attachments/assets/19e5a1e7-cb46-4909-ba59-e4f4560cae03)





 
 
 
 ### FIGURE-01 RASPI PI 4 PINOUT DIAGRAM 


The Raspberry Pi 4 Model B is built around a Broadcom BCM2711 system-on-chip that integrates a quad-core ARM Cortex-A72 (64-bit) CPU, VideoCore VI GPU, memory controller, and peripheral interfaces, forming a compact yet complete computer architecture where the SoC connects internally to RAM, USB 3.0 controller, Gigabit Ethernet, HDMI display, and wireless modules. Its 40-pin GPIO header provides a flexible pin configuration consisting of power pins (5 V and 3.3 V), multiple ground pins, and general-purpose input/output pins that operate at 3.3 V logic and can be programmed for digital I/O or alternate functions. Key alternate functions include I²C (SDA, SCL) for sensor communication, SPI (MOSI, MISO, SCLK, CS) for high-speed peripheral interfacing, UART (TX, RX) for serial communication, and PWM for control applications.  For communication, I2C (SDA, SCL), SPI (MOSI, MISO, SCK), and UART (TX, RX) interfaces are mapped across different GPIO pins, allowing seamless connectivity with sensors and peripherals. All GPIO pins support PWM (Pulse Width Modulation), making it useful for motor control, LED brightness adjustment, and sound applications. The BOOTSEL button enables USB mass storage mode for firmware flashing, while the DEBUG pins (SWD interface) provide debugging capabilities. With its low power consumption, flexible GPIO options, and rich interface support, the Raspberry Pi Pico is widely used for IoT, embedded systems, robotics, and automation projects.This architecture and pin multiplexing allow the Raspberry Pi 4 to act as both a general-purpose computing platform and an embedded controller, supporting rapid prototyping, hardware interfacing, and IoT applications.


## Working Principle:
Experiment 1A
The LED is connected to one of the GPIO pins of the Raspberry Pi 4.
The Python script sets the GPIO pin HIGH to turn the LED ON and LOW to turn it OFF.
CIRCUIT DIAGRAM
Connect the anode (longer leg) of the LED to GP15 via a 330Ω resistor.
Connect the cathode (shorter leg) of the LED to GND (ground).

Experiment 1B
The LED is connected to one of the GPIO pins of the Raspberry Pi 4.
The IR sensor is connected one of the GPIO pins in Raspberry Pi 4.
The Python script sets the GPIO pin HIGH to turn the LED ON and LOW to turn it OFF based on the IR sensor.
CIRCUIT DIAGRAM
Connect the anode (longer leg) of the LED to any one GPIO via a 330Ω resistor.
Connect the cathode (shorter leg) of the LED to GND (ground).
Connect the IR sensor Vcc to any +5V.
Connect the IR sensor GND to any GND.
Connect the IR sensor OUT to any one GPIO. 

## Experiment 1A – LED Blinking using Raspberry Pi 4

## PROGRAM(python):
```
import RPi.GPIO as GPIO
import time
import urllib.request

# ThingSpeak details
WRITE_API_KEY = "PP7MT4WH6UL6P1GJ"
CHANNEL_ID = 3249432
THINGSPEAK_URL = "https://api.thingspeak.com/update"

# Set GPIO numbering mode
GPIO.setmode(GPIO.BCM)

# Define LED pin
LED_PIN = 18

# Set GPIO18 as output
GPIO.setup(LED_PIN, GPIO.OUT)

def send_to_thingspeak(value):
    url = f"https://api.thingspeak.com/update?api_key=PP7MT4WH6UL6P1GJ&field1={value}"
    urllib.request.urlopen(url)
    print("Sent to ThingSpeak:", value)

try:
    while True:
        # LED ON
        GPIO.output(LED_PIN, GPIO.HIGH)
        print("LED ON")
        send_to_thingspeak(1)
        time.sleep(15)

        # LED OFF
        GPIO.output(LED_PIN, GPIO.LOW)
        print("LED OFF")
        send_to_thingspeak(0)
        time.sleep(15)

except KeyboardInterrupt:
    print("Program stopped")

finally:
    GPIO.cleanup()
```
## OUTPUT 1A:
## LED OFF:

![WhatsApp Image 2026-02-05 at 2 30 41 PM](https://github.com/user-attachments/assets/f3cbed13-7a83-4aba-8200-93c1d464bbdc)

![WhatsApp Image 2026-02-05 at 2 30 46 PM](https://github.com/user-attachments/assets/4541f067-d096-49bd-bd49-95a14318fae5)

<img width="1919" height="1027" alt="Screenshot 2026-02-04 111027" src="https://github.com/user-attachments/assets/fcf82c4b-d97c-443f-8035-ecc99817bf50" />


# LED ON:

![WhatsApp Image 2026-02-05 at 2 31 03 PM](https://github.com/user-attachments/assets/996e6892-6f27-4a15-9dd7-62fcd06fe1e3)

![WhatsApp Image 2026-02-05 at 2 31 04 PM](https://github.com/user-attachments/assets/7f19ceed-10bb-42a8-ab1b-91cb28fda34a)


<img width="1909" height="1023" alt="Screenshot 2026-02-04 110736" src="https://github.com/user-attachments/assets/b84b1e64-b149-47a1-a544-747340d8d7cb" />


## Experiment 1B – LED Control using IR Sensor

## PROGRAM(python):

```
import RPi.GPIO as GPIO
import time
import urllib.request

# ThingSpeak details
WRITE_API_KEY = "PP7MT4WH6UL6P1GJ"
CHANNEL_ID = 3249432
THINGSPEAK_URL = "https://api.thingspeak.com/update"



# Pin setup
SENSOR_PIN = 23   # Input from sensor
LED_PIN = 18      # Output to LED

# GPIO mode
GPIO.setmode(GPIO.BCM)

# Setup pins
GPIO.setup(SENSOR_PIN, GPIO.IN)
GPIO.setup(LED_PIN, GPIO.OUT)

def send_to_thingspeak(value):
    url = f"https://api.thingspeak.com/update?api_key=PP7MT4WH6UL6P1GJ&field2={value}"
    urllib.request.urlopen(url)
    print("Sent to ThingSpeak:", value)


print("Sensor + LED system running...")

try:
    while True:
        sensor_value = GPIO.input(SENSOR_PIN)

        if sensor_value == 0:   # Many IR sensors give LOW when object detected
            print("Object Detected! LED ON")
            GPIO.output(LED_PIN, GPIO.HIGH)
            send_to_thingspeak(1)

            time.sleep(15)
        else:
            print("No Object. LED OFF")
            GPIO.output(LED_PIN, GPIO.LOW)
            send_to_thingspeak(0)

            time.sleep(15)

        time.sleep(0.1)

except KeyboardInterrupt:
    print("Stopped by user")

finally:
    GPIO.cleanup()
```
## OUTPUT 1B:
# LED OFF(NO OBJECT DETECTED):
![WhatsApp Image 2026-02-05 at 2 37 18 PM](https://github.com/user-attachments/assets/9e78b002-2a80-4099-891e-c513ba2b20fd)

![WhatsApp Image 2026-02-05 at 2 37 29 PM](https://github.com/user-attachments/assets/683ea5fc-ae03-435d-a7b7-475477daaa78)


<img width="1919" height="1019" alt="Screenshot 2026-02-04 115926" src="https://github.com/user-attachments/assets/516d984f-6a34-4b64-a738-c421130864ae" />


 # LED ON(OBJECT DETECTED):

![WhatsApp Image 2026-02-05 at 2 37 34 PM](https://github.com/user-attachments/assets/643ee7e6-bb83-403d-ae00-9041bde44bed)

![WhatsApp Image 2026-02-05 at 2 38 00 PM](https://github.com/user-attachments/assets/25940bc9-357d-4310-9c07-562827ad79aa)


<img width="1919" height="1029" alt="Screenshot 2026-02-04 115956" src="https://github.com/user-attachments/assets/778962d0-20a3-48c9-a792-ce17cb6b6c43" />

## RESULTS
The LED connected to the Raspberry Pi 4 successfully turns ON and OFF at  user defined time  confirming the proper interfacing of a digital output.
