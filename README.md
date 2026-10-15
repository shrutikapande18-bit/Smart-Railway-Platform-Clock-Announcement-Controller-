# 🚆 Smart Railway Platform Clock & Announcement Controller

> **🔧 Embedded C | LPC2148 (ARM7) | ⏰ RTC | 🖥️ LCD | 🔢 4×4 Keypad | 🚦 LEDs | 🔔 Buzzer**

A microcontroller-based railway platform information system designed to monitor train schedules, display time and train details, and provide visual or audible alerts based on programmed train status.

---

## 📌 Project at a Glance

| Item | Details |
|---|---|
| 🏷️ Project Domain | Embedded Systems / Microcontroller Interfacing |
| 🧠 Controller | LPC2148 (ARM7) |
| 💻 Programming Language | Embedded C |
| 🖥️ User Interface | 16×2 LCD and 4×4 Matrix Keypad |
| ⏰ Time Source | RTC module |
| 🚦 Status Outputs | Green, yellow, and red LEDs |
| 🔔 Audio Output | Buzzer |
| 🛠️ Development Tools | Keil and Flash Magic |

## 📖 Overview

The **Smart Railway Platform Clock & Announcement Controller** is an embedded systems project intended to reduce the need for continuous manual monitoring of platform information.

The proposed system combines an LPC2148 microcontroller with an RTC, LCD, matrix keypad, LEDs, and a buzzer. It is designed to compare the current time against stored train schedules, display relevant train details, and indicate configured train statuses.


## 🎯 Objectives

- ⏰ Display the current date and time using an RTC.
- 🔢 Enter and edit train schedules through a 4×4 matrix keypad.
- 🚆 Store train numbers, names, destinations, arrival/departure times, and platform numbers.
- 🔄 Compare current time with stored train schedules.
- 🖥️ Display upcoming train information on the LCD.
- 🚦 Indicate on-time, approaching, or delayed status using LEDs.
- 🔔 Provide buzzer alerts for configured events.
- 🔐 Support authorized schedule and clock configuration, if implemented.

## ✨ System Features

### ⏰ RTC-Based Timekeeping
Uses the RTC as the time reference for comparing the current time against train schedules. The exact RTC model and communication interface must be confirmed from the actual hardware.

### 🚆 Train Schedule Management
The project includes a `TrainInfo_t` structure containing train information, scheduled and updated arrival/departure times, platform number, and delay minutes.

### 🖥️ LCD Information Display
A 16×2 character LCD can display the current time and selected train information, depending on the implemented firmware.

### 🔢 Keypad Input
A 4×4 matrix keypad provides 16 key positions for entering data and navigating menus.

### 🚦 LED Status Indication

- 🟢 **Green LED:** Train on time
- 🟡 **Yellow LED:** Train approaching
- 🔴 **Red LED:** Train delayed

The firmware must define the conditions and timing thresholds for each state.

### 🔔 Audible Alerts
A buzzer can provide audible notifications for an approaching train or an important schedule change, if the corresponding control logic is implemented.

### 🔐 Administrative Mode
The workflow describes an administrator-edit switch or interrupt and authorization before changing schedules or RTC settings. Confirm the actual implementation before marking this feature complete.

---

## 🧰 Hardware Components

| Component | Purpose |
|---|---|
| 🧠 LPC2148 | Runs the Embedded C program and controls peripherals |
| 🖥️ 16×2 LCD | Displays time and train information |
| ⏰ RTC Module | Supplies current date and time |
| 🔢 4×4 Matrix Keypad | Accepts user input |
| 🟢 Green LED | Indicates on-time status |
| 🟡 Yellow LED | Indicates approaching status |
| 🔴 Red LED | Indicates delayed status |
| 🔔 Buzzer | Provides audible notifications |


### 🧠 1. LPC2148 Microcontroller

The LPC2148 is an ARM7-based 32-bit microcontroller. It is intended to execute the main application, process train records, scan the keypad, and control output devices.

### 🖥️ 2. 16×2 LCD

The LCD has two rows, with 16 character positions per row. It can display the current time, train number, destination, arrival/departure times, platform, and status, subject to the implemented screen layout.

### ⏰ 3. Real-Time Clock (RTC)

The RTC maintains date and time information used by the schedule-comparison logic. Confirm the exact RTC model and interface from the circuit before documenting the final wiring.

### 🔢 4. 4×4 Matrix Keypad

The keypad consists of four rows and four columns, providing 16 key positions. A scanning routine identifies the pressed key by determining the active row and column.

#### ⌨️ Keypad Lookup Table

```c
u8 kpmLUT[4][4] =
{
    {'1', '2', '3', 'B'},
    {'4', '5', '6', '/'},
    {'7', '8', '9', '-'},
    {'C', '0', 'E', '+'}2
};
```

| Row / Column | C1 | C2 | C3 | C4 |
|---|---:|---:|---:|---:|
| R1 | 1 | 2 | 3 | B |
| R2 | 4 | 5 | 6 | / |
| R3 | 7 | 8 | 9 | - |
| R4 | C | 0 | E | + |

- `kpmLUT[4][4]` stores the character assigned to each row/column position.
- `u8` is a project-specific type alias; check the project headers for its definition.
- The meaning of keys such as `B`, `C`, `E`, `/`, `-`, and `+` depends on the code handling the keypad input.

### 🚦 5. LED Indicators

Three LEDs provide visual status indications. Each LED must be connected through an appropriate current-limiting circuit. Actual GPIO connections must match the circuit and source code.

### 🔔 6. Buzzer

The buzzer provides sound alerts. The driver circuit depends on whether the selected buzzer is active or passive and on its current requirements.

---

## 📍 Pin Configuration

### 🧷 Example LPC2148 Pin Assignment

| Peripheral | Signal | Example Pin | Purpose |
|---|---|---|---|
| LCD | RS | P0.5| Register select |
| LCD | EN | P0.6 | Enable |
| LCD | D4 | P0.7 | Data bit 4 |
| LCD | D5 | P0.8 | Data bit 5 |
| LCD | D6 | P0.9 | Data bit 6 |
| LCD | D7 | P0.10 | Data bit 7 |
| Keypad | R1–R4 | P0.16–P0.19 | Row connections |
| Keypad | C1–C4 | P0.20–P0.23 | Column connections |
| 🟢 Green LED | Control | P0.16 | On-time indicator |
| 🟡 Yellow LED | Control | P0.17 | Approaching indicator |
| 🔴 Red LED | Control | P0.18 | Delay indicator |
| 🔔 Buzzer | Control | P0.22 | Audio alert |


## 🧠 Train Data Structure

| Field | Purpose |
|---|---|
| `trainNumber` | Train number |
| `trainName` | Train name |
| `destination` | Destination name |
| `arrivalHour`, `arrivalMinute` | Scheduled arrival time |
| `departureHour`, `departureMinute` | Scheduled departure time |
| `updatedArrivalHour`, `updatedArrivalMinute` | Updated arrival time |
| `updatedDepartureHour`, `updatedDepartureMinute` | Updated departure time |
| `platform` | Platform number |
| `delayMinutes` | Delay in minutes |

## 🚆 Sample Train Records

| Train No. | Train Name | Destination | Scheduled Arrival | Scheduled Departure | Platform |
|---:|---|---|---|---|---:|
| 12627 | Karnataka Express | New Delhi | 09:30 | 09:35 | 1 |
| 12028 | Shatabdi Express | Chennai | 10:15 | 10:20 | 2 |
| 12785 | Kacheguda Express | Hyderabad | 11:00 | 11:05 | 3 |

The project gives the following updated timings for Kacheguda Express:

- 🕗 Updated arrival: **08:20**
- 🕗 Updated departure: **08:25**
- ⏳ Delay: **20 minutes**


---

## 🔄 System Workflow

<img width="1278" height="1958" alt="System Workflow" src="https://github.com/user-attachments/assets/c82f9e8b-7138-4705-872e-9f25c289bfe3" />



## 🧭 Program Execution Flow

1. 🚀 **Startup:** Initialize the LPC2148 and the peripherals used by the application.
2. ⏰ **Read Time:** Obtain the current date and time from the RTC.
3. 🚆 **Load Schedule Data:** Access train records stored in the program or configured storage.
4. 🔍 **Compare Schedule:** Compare the current time with scheduled or updated train timings.
5. 🎯 **Select Train:** Determine which train information should be displayed.
6. 🖥️ **Update LCD:** Display the current time and selected train information.
7. 🚦 **Update Indicators:** Control the LEDs according to the implemented timing and delay rules.
8. 🔔 **Generate Alerts:** Activate the buzzer when a configured alert condition occurs.
9. 🔢 **Check Keypad/Admin Input:** Scan the keypad and check for administrative requests, if implemented.
10. 🔐 **Validate Changes:** Validate inputs before updating train records or RTC settings.
11. 🔄 **Repeat:** Continue monitoring and refreshing the display.


## 🧱 Block Diagram
<img width="560" height="466" alt="Block diagram" src="https://github.com/user-attachments/assets/5b6e0d4f-51a2-4025-ae5c-e5c6104c6b3f" />


## 🛠️ Software and Development Tools

- 💻 **Embedded C:** Used to write the firmware.
- 🧰 **Keil:** Used to edit, build, and debug compatible embedded projects.
- 🔌 **Flash Magic:** Used to program supported NXP microcontrollers.

## 🗂️ Project Structure and File Organization


```text
Smart-Railway-Platform-Controller/
├── README.md
├── project-documentation.pdf
├── src/
│   ├── main.c                 # Main application / scheduler
│   ├── lcd.c                  # LCD driver, if separate
│   ├── keypad.c               # Keypad scanning, if separate
│   ├── rtc.c                  # RTC functions, if separate
│   ├── train_schedule.c       # Train records and comparison, if separate
│   └── indicators.c           # LED and buzzer control, if separate
├── inc/
│   ├── project.h
│   ├── lcd.h
│   ├── keypad.h
│   ├── rtc.h
│   ├── train_schedule.h
│   └── indicators.h
├── images/
│   ├── block-diagram.png
│   ├── pin-configuration.png

```


## 🚀 Possible Future Enhancements

📱 Mobile application integration
💾 EEPROM/Flash-based train storage
🖥️ Graphical LCD/TFT display
📢 Voice-based railway announcements
🌐 IoT-based remote schedule updates
🛰️ GPS-based train tracking
🔄 Automatic railway schedule synchronization

## 👩‍💻 Project Information

- **📌 Project Title:** Smart Railway Platform Clock & Announcement Controller
- **🧠 Controller:** LPC2148
- **💻 Programming Language:** Embedded C
- **🔧 Domain:** Embedded Systems and Microcontroller Interfacing

Conclusion
The Smart Railway Platform Clock & Announcement Controller demonstrates the integration of multiple embedded peripherals into a practical railway information prototype.

The project combines real-time clock management, train scheduling, keypad input, LCD display, LED status indication, buzzer alerts, and interrupt handling in a modular Embedded C application.

🚆 Smart Railway Platform Clock & Announcement Controller
LPC2148 ARM7 • Embedded C • RTC • LCD • Keypad • LEDs • Buzzer • Interrupts • Proteus
