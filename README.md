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
<br>
5. HMC5883L Magnetometer (GY-271)

HMC5883L

What it measures

Earth's magnetic field:

MX
MY
MZ
Working mechanism

Earth behaves like a giant magnet.

The sensor contains magnetoresistive elements.

When magnetic field strength changes:

Electrical resistance changes
Magnetic field direction is computed

This acts like a spacecraft compass.

Real satellites often use magnetometers for attitude determination.
<br>
6. LDR (Light Dependent Resistor)
What it measures

Light intensity.

Working mechanism

Made from a photoconductive material.

Bright light

Many electrons are released.

Resistance decreases.

Darkness

Few electrons move.

Resistance increases.

The ESP8266 reads the resulting voltage change through its analog input.
<br>


         DHT11
            │
            ▼
     Temperature
       Humidity

BMP180 ──► Pressure
            Altitude

MPU6050 ─► Motion
            Rotation

GY271 ───► Compass

LDR ─────► Light

            │
            ▼

        ESP8266
     (On-Board Computer)

            │
            ▼

     Telemetry Packet

            │
            ▼

      Ground Station
      (Your Laptop).


         codes ..
         #include <Wire.h>
#include <DHT.h>
#include <Adafruit_MPU6050.h>
#include <Adafruit_Sensor.h>
#include <Adafruit_BMP085.h>  // for BMP180 (GY-68)

#define DHTPIN D4
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE);

Adafruit_MPU6050 mpu;
Adafruit_BMP085 bmp;

int packetID = 0;

void setup() {
  Serial.begin(115200);
  Wire.begin();

  Serial.println("\nE-CUBE FULL MISSION START");

  dht.begin();

  if (!mpu.begin(0x68)) {
    Serial.println("MPU6050 NOT FOUND");
  }

  if (!bmp.begin()) {
    Serial.println("BMP180 NOT FOUND");
  }

  pinMode(A0, INPUT);

  Serial.println("ALL SYSTEMS READY\n");
}

void loop() {

  // DHT11
  float temp = dht.readTemperature();
  float hum = dht.readHumidity();

  // LDR
  int light = analogRead(A0);

  // MPU6050
  sensors_event_t a, g, t;
  mpu.getEvent(&a, &g, &t);

  // BMP180
  float pressure = bmp.readPressure() / 100.0;
  float altitude = bmp.readAltitude();

  // Telemetry packet (CubeSat style)
  Serial.print("{ID:");
  Serial.print(packetID++);
  Serial.print(",T:");
  Serial.print(temp);
  Serial.print(",H:");
  Serial.print(hum);
  Serial.print(",LDR:");
  Serial.print(light);
  Serial.print(",P:");
  Serial.print(pressure);
  Serial.print(",ALT:");
  Serial.print(altitude);
  Serial.print(",AX:");
  Serial.print(a.acceleration.x);
  Serial.print(",AY:");
  Serial.print(a.acceleration.y);
  Serial.print(",AZ:");
  Serial.print(a.acceleration.z);
  Serial.print(",GX:");
  Serial.print(g.gyro.x);
  Serial.print(",GY:");
  Serial.print(g.gyro.y);
  Serial.print(",GZ:");
  Serial.println(g.gyro.z);

  delay(2000);
}
