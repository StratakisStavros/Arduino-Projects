# Tweak the Arduino Logo – Modified

## Overview

This project combines an Arduino Uno and a Processing sketch to create an interactive visualization driven by a potentiometer.  Rotating the potentiometer changes the background colour of a window and updates a gauge bar that displays the sensor value as a percentage.  You can also log sensor readings to a CSV file for later analysis.

This work was **inspired by** the **Tweak the Logo** example from the *Arduino Projects Book* (Project 14), which changes the logo colour based on a potentiometer reading.  Rather than simply reproducing the book’s code, I used the concept as a starting point and wrote a bespoke implementation that extends the idea.  Key differences include reading the **full 10‑bit analogue value** instead of compressing it to 8 bits, adding a real‑time gauge bar, scaling the logo, and implementing CSV logging for further data analysis.

## Purpose and learning outcomes

I built this project as a hands‑on exercise to deepen my understanding of **serial communication**, **sensor integration**, and **real‑time data visualization**.  The goal was not only to replicate a cool idea, but to extend it in ways that demonstrate my ability to:

* Integrate hardware and software by linking an Arduino sensor to a desktop application.
* Use the full 0–1023 ADC range (rather than dividing by four) to achieve smoother colour transitions and higher fidelity.
* Map raw analogue readings to meaningful visual elements (colour gradients and a gauge bar).
* Implement simple user controls, such as toggling data logging with a key press.
* Export sensor data in a structured format for later analysis.

By enhancing the original concept with a dynamic gauge and CSV logging, this project showcases the practical skills I’ve been developing.  It also serves as a portfolio piece to illustrate my learning journey and provides a conversation starter when sharing my work with potential employers or collaborators.

## Hardware

* **Microcontroller:** Arduino Uno (or compatible)
* **Sensor:** 10 kΩ potentiometer connected to analogue input `A0`
* **Connections:**
  * One end of the potentiometer to 5 V
  * Other end to GND
  * Wiper (middle pin) to `A0`
* **PC connection:** USB cable between Arduino and your computer

## Software

* **Arduino sketch** (`Arduino_Code/Arduino_Code.ino`)

  The Arduino reads the voltage on `A0`, converts it to an integer between 0 and 1023, and sends the value over the serial port at 9 600 baud.  Unlike the book example, this version does **not** divide the reading by four, allowing the Processing side to work with the full 10‑bit resolution.  A small delay is used to avoid overwhelming the serial buffer.

* **Processing sketch** (`Processing Code/Processing_Sketch.pde`)

  The Processing program opens a serial connection to the Arduino and listens for the sensor values.  It then:

  - Maps the sensor value (0–1023) directly to a hue (0–255) and sets the background colour accordingly.
  - Draws the Arduino logo on the screen, scaled to fit in the window.
  - Renders a horizontal gauge bar beneath the logo that fills according to the sensor percentage.
  - Allows you to start/stop logging by pressing the **`L`** key.  When logging is enabled, each reading is written to the CSV file along with the current time in milliseconds and the computed percentage.

## Folder structure

```
Tweak The Logo Modified/
├── Arduino_Code/
│   └── Arduino_Code.ino        → Arduino sketch (reads A0 and sends full 10‑bit value)
├── Processing Code/
│   ├── Processing_Sketch.pde   → Processing sketch (visualisation & logging)
│   └── Recording Example.csv    → Example of logged sensor data
├── data/
│   └── arduino_logo.png        → PNG image of the Arduino logo
└── README.md                   → This file
```

*Note:* The `Recording Example.csv` file shows the format of logged data.  The actual log file is created at runtime when you enable logging.

## How to run the project

1. **Upload the Arduino code**: Open `Arduino_Code/Arduino_Code.ino` in the Arduino IDE, select your board and COM port, and click **Upload**.
2. **Connect the potentiometer**: Ensure the potentiometer is wired to 5 V, GND, and `A0` as described in the Hardware section.
3. **Launch Processing**: Open `Processing Code/Processing_Sketch.pde` in the Processing IDE.  Make sure the image `arduino_logo.png` is inside the sketch’s `data` folder.
4. **Set the serial port**: In the Processing sketch, adjust the port string (`"COM3"` on Windows or `/dev/ttyACM0` on Linux/Mac) to match your Arduino’s port.
5. **Run the Processing sketch**: Click the **Run** button in Processing.  The window will display the Arduino logo with a colour‑changing background and gauge bar.
6. **Interact**:
   - Rotate the potentiometer to change the background hue and gauge fill.
   - Press **`L`** to start or stop logging.  A CSV file (named `Recording.csv`) will be created in the sketch folder with three columns: `Time(ms)`, `SensorValue` and `Percentage`.

## Logging format

Logged data is stored in semicolon‑separated format to ensure proper column separation in Excel and other spreadsheet programs that use comma as the decimal delimiter in European locales.  Each entry contains:

| Column        | Description                               |
|---------------|-------------------------------------------|
| `Time(ms)`    | Time since the Processing sketch started   |
| `SensorValue` | Raw ADC reading from 0 to 1023             |
| `Percentage`  | Sensor value mapped to 0 – 100 %           |

Example row:

```
1516;789;77
```

## Notes

* If the Processing sketch cannot find the correct serial port, print the list of available ports (`Serial.list()`) and select the correct one.
* When logging is enabled, the CSV file may be overwritten on each run.  If you need to keep multiple sessions, rename or move the file after each run.

## License

This project is provided for educational and demonstration purposes.  You are free to modify and reuse the code for personal projects.  See the main repository for licence details.
