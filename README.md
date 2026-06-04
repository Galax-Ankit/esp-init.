# esp-init.
Starting with esp.32
<br>
. TUTOR-Ankit Tiwari

1. ESP8266 (On-Board Computer)
   What it does

This is the brain of the satellite. All sensor data passes through it.

Your evidence

You successfully:

Uploaded code
Received telemetry
Communicated through USB
Working mechanism
Executes firmware stored in flash memory.
Reads sensors through GPIO and I2C interfaces.
Processes measurements.
Creates telemetry packets.
Sends packets to the ground station (currently through USB Serial).

In a real satellite, this role is called the On-Board Computer (OBC).

<br>
2. DHT11 (Temperature & Humidity Sensor)

DHT11

What it measures
Air temperature
Relative humidity

<br>
Working mechanism
Temperature

Inside the DHT11 is an NTC thermistor.

As temperature changes:

Resistance changes
Internal circuitry measures resistance
Converts it into temperature
Humidity

Inside is a moisture-sensitive polymer.

As humidity changes:

Water molecules are absorbed
Electrical properties change
Sensor calculates relative humidity

The sensor's internal microcontroller sends digital data to the ESP8266.
<br>
3. MPU6050 (GY-521)

MPU6050

What it measures
Accelerometer
AX
AY
AZ
Gyroscope
GX
GY
GZ
Working mechanism
Accelerometer

Tiny microscopic masses are suspended on silicon springs.

When acceleration occurs:

The mass shifts
Capacitance changes
Electronics convert this into acceleration

This is a MEMS device (Micro-Electro-Mechanical System).
<br>
Gyroscope

Contains vibrating MEMS structures.

When rotated:

Coriolis forces act on the structures
Tiny motion is detected
Rotation rate is calculated

Used in satellites to estimate attitude.
