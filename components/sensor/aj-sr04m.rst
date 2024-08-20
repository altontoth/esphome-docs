AJ-SR04M Waterproof Ultrasonic Range Finder
============================================

.. seo::
    :description: Instructions for setting up AJ-SR04M waterproof ultrasonic distance sensor in ESPHome.
    :image: jsn-sr04t-v3.jpg
    :keywords: AJ-SR04M

Specification:
    - Operating Voltage: DC 3-5.5V
    - Operating range of 20cm to 450cm.
    - Total Current Draw:30mA
    - Frequency: 40KHZ
    - Resolution : about 0.5cm
    - Beam Angle: less than 50 degrees
    - Working Temperature:-10~70° C
    - Storage temperature:-20~80° C

This sensor allows you to use the AJ-SR04M Waterproof Ultrasonic Range Finder **in Mode 1 and 2**
with ESPHome to measure distances. This sensor can measure ranges between 25 centimeters and 
450 centimeters with a resolution of 5 millimeters. This sensor is very similar in design and 
form factor to the JSN-SR04T

The device has multiple operating modes –

    1) Traditional Trigger and Echo Mode
    2) Low Power Traditional Trigger and Echo Mode
    3) Automatic Serial Port Mode
    4) Low Power Serial Mode
    5) ASCII Mode

Configure the AJ-SR04M for mode 1: Traditional Trigger and Echo Mode (same as the popular HCSR04):
  - Clear Jumper at R19

  This mode uses Trigger and Echo pins. You will have to allocate two digital pins on your microcontroller, and a software timer to calculate the distance.

  Note that the communication sequence is the same as per HC SR04, but make sure your high signal on the Trigger pin is at least 10 ms 
  wide. Some users have reported getting good reading only after increasing the trigger width to 20m.

Configure the AJ-SR04M for mode 2: Low Power Traditional Trigger and Echo Mode:
  - Install 300KΩ resistor at R19

  This mode operates similar to Mode 1 above, but only consumes around 40uA of current during sleep, so is idea for use when powered by a battery

  This module remains in sleep mode until a trigger pulse of more than 1mS is received. At this point the module activates a measurement cycle.

Configure the AJ-SR04M for mode 3: Automatic Serial Mode:
  - Install 120KΩ resistor at R19

  In this mode, the module continuously takes measurements approximately every 120mS and outputs the distance on the TX pin at 9600 baud.
  In this mode :ref:`sensor-filters` are highly recommended.

  AJ-SR04m transmits bytes per measurement on the echo pin, as below:
    - Byte 1: Start Byte
    - Byte 2: Upper Byte
    - Byte 3: Lower Byte
    - Byte 4: Checksum

Configure the AJ-SR04M for mode 4: Low Power Serial Mode:
  - Install 47KΩ resistor at R19

  In this mode, the module only consumes 20uA in low power sleep mode. When a trigger signal is received, the sensor wakes up, performs the measurement,
  and responds on the echo line. The sensor then goes back to sleep after transmitting the data.

Configure the AJ-SR04M for mode 5: Computer Printing Mode:
  - Install 0KΩ jumper (bridge) at R19

  In this mode, the module does the distance calculations on board and transmits output data on the serial interface after receiving an input pulse. 

AJ-SR04M Waterproof Ultrasonic Range Finder.

.. code-block:: yaml

    # Example configuration entry
    sensor:
      - platform: ultrasonic
        trigger_pin: GPIO12
        echo_pin: GPIO13
        name: "Ultrasonic Sensor"
        pulse_time: 15us
        update_interval: 5s
        timeout: 5.0m
        filters:
          - filter_out: nan

See Also
--------

- :ref:`uart`
- :ref:`sensor-filters`
- :apiref:`jsn_sr04t/jsn_sr04t.h`
- :ghedit:`Edit`
