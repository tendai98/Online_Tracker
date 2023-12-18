# ESP8266-based Tracking Device Prototype 🛰️

This project showcases a simple prototype for a tracking device powered by an ESP8266 microcontroller, NeoV6 GPS module, ADXL345  accelerometer and IR sensor. The aim is to transmit location data, accelerometer data and IR sensor state.

## Project Structure 📁

```
- 🔄 accelerometer.h (Accelerometer)
- 🖥️ client.h (HTTP Client)
- 🧱 const.h (Constants)
- 🌐 gps.h (GPS Module)
- 📡 ir.h (IR Sensor)
- 🛰️ Online_Tracker.ino (Sketch)
- 📶 wifi.h (Wi-Fi)
```

## Code Overview 📋

### Accelerometer Module (`accelerometer.h`) 🌀

Initializes and retrieves data from the ADXL345 accelerometer.

### Client Module (`client.h`) 🖥️

Manages HTTPS requests to send data to the backend with requests carrying location data and accelerometer data.

### Constants Module (`const.h`) 🧱

Defines various constants utilized throughout the code, like the hostname, SSID name and SSID password

### GPS Module (`gps.h`) 🛰️

Manages the initialization and retrieval of location data from the NeoV6 GPS module.

### IR Sensor Module (`ir.h`) 📡

Manages the state of the Infrared (IR) sensor.

### Online Tracker Sketch (`Online_Tracker.ino`)

Integrates modules to fetch accelerometer and GPS data, sends HTTP requests to a host, and handles device setup.

### Wi-Fi Module (`wifi.h`) 📶

Manages the Wi-Fi connection and initiates connection using provided credentials.

## Functionality 🚀🔧

- **Accelerometer**: Collects X, Y, and Z-axis accelerometer data.
- **HTTP Requests**: Sends location, accelerometer and IR sensor state data via HTTPS requests.
- **GPS Tracking**: Retrieves GPS location data for the device.
- **IR Sensor State**: Monitors the state of the Infrared (IR) sensor.
- **Wi-Fi Connection**: Manages the connection to the specified Wi-Fi network.

### Initial Setup 🛠️

The `setup()` function initializes the Wi-Fi connection, GPS module, accelerometer, and IR sensor.

### Main Loop 🔁

The `loop()` function continuously gathers accelerometer and GPS data, constructs a data payload, sends it to the specified host via an HTTPS GET request, and adds a slight delay for processing.

### Code

Certainly! Here's an example of how you might structure a section in the README file that includes function names and crucial code snippets:

### 🛰️ Key Functions and Features

#### Accelerometer Data Retrieval
```
void initAccelerometer() {
  if (!accel.begin()) {
    while (1);
  }
  accel.setRange(ADXL345_RANGE_4_G);
}

void getAccelerationData() {
  sensors_event_t event;
  accel.getEvent(&event);

  x = (event.acceleration.x) / 9.8;
  y = (event.acceleration.y) / 9.8;
  z = (event.acceleration.z) / 9.8;
}
```

#### GPS Location Tracking
```
void initGPS(int bitrate) {
  ss.begin(bitrate);
}

void getLocationData() {
  bool newData = false;
  
  for (unsigned long start = millis(); millis() - start < 1000;) {
    while (ss.available()) {
      char c = ss.read();
      if (gps.encode(c)) {
        newData = true;
      }
    }

    if (newData) {
      gps.f_get_position(&flat, &flon, &age);
    } 
  }
}
```

#### Wi-Fi Connectivity
```
void startWiFi(char *ssid, char *password) {
  WiFi.hostname(TRACKER_NODE_ID);
  WiFi.mode(WIFI_STA);
  pinMode(FLASH_LED_GPIO, OUTPUT);
  
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    digitalWrite(FLASH_LED_GPIO, LOW);
    delay(100);
    digitalWrite(FLASH_LED_GPIO, HIGH);
    delay(100);
  }

  digitalWrite(FLASH_LED_GPIO, HIGH);
}
```
