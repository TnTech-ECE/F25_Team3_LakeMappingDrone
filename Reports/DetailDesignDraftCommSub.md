# Detailed Design of Communication Subsystem

## Function of the Subsystem

The Communications Subsystem provides all wireless data exchange, operator control, and real-time information flow between the lake-mapping vessel and the shore-based ground station. Its primary role is to ensure that the vessel remains continuously observable and controllable while enabling the transmission of mission-critical bathymetric data.

The subsystem establishes a bidirectional MAVLink telemetry link between the Pixhawk autopilot on the vessel and the Mission Planner ground station using a 3DR SiK 915 MHz radio pair. Through this link, the operator receives live GPS, attitude, health, and state information and can issue manual override commands at any time. Mission Planner converts joystick or keyboard input into RC override MAVLink messages, which are sent through the telemetry link and interpreted by the Pixhawk as virtual RC signals, allowing the operator to immediately control throttle and steering.

In addition to control functions, the Communications Subsystem transmits live bathymetric data from the vessel. The Raspberry Pi reads depth from the Ping1D sonar, pairs it with GPS and timestamp data obtained from the Pixhawk, and sends compact MAVLink packets to shore for real-time map visualization. Simultaneously, the Raspberry Pi performs redundant SD-card logging of all data to guarantee data integrity even if the telemetry link becomes unreliable.

Overall, this subsystem integrates wireless communication, operator override capability, real-time data streaming, and robust logging to support safe operation and mission success.


## Specifications and Constraints

### Functional Specifications

#### Wireless Telemetry Link  
The subsystem shall provide a **bidirectional MAVLink telemetry connection** between the Pixhawk autopilot and the shore computer using a pair of 3DR SiK 915 MHz radios.

#### Manual Override Capability  
The subsystem shall allow the operator to wirelessly issue **RC override** commands from Mission Planner to the Pixhawk to assume manual control of vessel steering and throttle at any time.

#### Ping1D Depth Integration via Pixhawk  
The Ping1D sonar shall be connected directly to the Pixhawk via a UART serial port configured as a rangefinder. Depth values shall be published as standard MAVLink rangefinder messages.

#### Live Bathymetry Data Transmission  
The subsystem shall transmit **depth + GPS data** from the Pixhawk through the telemetry link to the shore computer at a minimum rate of **1 Hz** for real-time map visualization.

#### Local SD Logging  
The subsystem shall ensure that all depth, GPS, and timestamp data are continuously logged to the Raspberry Pi’s microSD card, independent of telemetry link quality.

#### Timestamped Data Association  
Each depth reading used for live mapping or logging shall be associated with a valid timestamp and GPS coordinate obtained from the Pixhawk.

#### Link Monitoring and Status Reporting  
The subsystem shall monitor MAVLink heartbeats to detect telemetry loss, degraded link quality, or reconnection events, and shall report these to the operator through Mission Planner.

#### Safe Fallback Behavior Interface  
Manual override commands shall always take priority over autonomous behavior, ensuring safe operator intervention when needed. In an event of full disconnection, the vessel shall retrace its path until connection is restored

---

### Non-Functional Specifications

#### Wireless Range  
The subsystem shall maintain reliable connectivity at a minimum distance of **300–500 meters line-of-sight**.

#### Latency Requirement  
End-to-end latency for telemetry updates and manual override commands shall be **≤ 500 ms** under nominal operating conditions.

#### Data Throughput  
Combined traffic from telemetry, bathymetry, and override commands shall not exceed **20 kbps**, within the telemetry radio’s bandwidth capacity.

#### Power Consumption Limits  
The subsystem shall operate within the following power budgets:  
- 3DR Air Radio: **5 V @ ≥ 1.5 A**  
- Raspberry Pi: **5 V @ ≥ 3.0 A**  
- Ping1D: powered within Pixhawk’s 5 V rail limit or the main regulated 5 V bus.

#### Environmental Requirements  
All components shall operate within **0–40°C**, housed in a splash-resistant enclosure with protection against condensation.

#### Electromagnetic Compatibility (EMC)  
Wiring and device placement shall minimize interference with GPS or Pixhawk sensors. Antennas and serial cables shall be routed to reduce EMI.

#### Startup Time  
The subsystem shall establish the telemetry link and begin logging within **60 seconds** of power-on.

#### Mechanical & Weight Constraints  
All communications hardware shall fit into the designated electronics bay and add no more than **1.0 kg** to the vessel's overall mass.

#### Maintainability  
Cables, connectors, and harnesses shall be labeled so that the subsystem can be removed or replaced within **10 minutes** using basic tools.

#### Logging Reliability  
SD logging shall flush buffers frequently enough that no more than **10 samples** are lost during unexpected power loss.

#### Data Integrity  
Checksum verification within the MAVLink protocol shall ensure corrupted depth or GPS data is discarded rather than logged or displayed.

---

### Standards, Ethical, and Socio-Economic Constraints

#### FCC Part 15 Compliance  
The subsystem shall operate within FCC Part 15 regulations for unlicensed ISM-band devices using 915 MHz.

#### Safety and Ethical Override Requirement  
Manual override commands must always override autonomous control strategies to ensure safe operation around people, wildlife, and obstacles.

#### Open and Reproducible Design  
The subsystem shall use non-proprietary, accessible hardware platforms (Pixhawk, Raspberry Pi, 3DR radios) to support reproducibility and educational use.

#### Data Transparency and Accessibility  
All logged bathymetry and GPS data shall be stored in human-readable, open formats such as CSV.

#### Security and Unauthorized Control Mitigation  
Telemetry radio network IDs and MAVLink channel settings shall be configured to non-default values to reduce the risk of unauthorized control.

#### Ethical Environmental Data Handling  
Collected environmental data shall be logged and transmitted accurately to avoid misrepresentation of water depth or terrain.

---

### Subsystem Design Constraints

#### Pixhawk Serial Port Allocation Constraint  
The Ping1D shall use a Pixhawk serial port that supports rangefinder operation (e.g., SERIAL4/5) and shall be configured via firmware parameters.

#### Voltage Rail Limitations  
If Pixhawk 5 V peripheral power limits are approached, the Ping1D shall instead be powered from the regulated 5 V bus.

#### Telemetry Priority Constraint  
Manual override messages shall be prioritized over secondary data (e.g., bathymetry packets) during periods of low bandwidth.

#### Antenna Placement Constraint  
The telemetry antenna shall be mounted above the waterline and oriented to maximize line-of-sight propagation and reduce multipath interference.


## Overview of Proposed Solution

The Communications Subsystem integrates MAVLink telemetry, operator override control, live bathymetry streaming, and SD-card data logging through the following architecture:

### Wireless Telemetry using 3DR SiK Radios

A 3DR SiK 915 MHz air module connects to the Pixhawk via UART. A matching ground module connects to Mission Planner via USB. This link carries:

-MAVLink telemetry
-Operator commands
-Live bathymetry data packets

The 915 MHz signal is a legal, low bandwidth signal that is accepted for un-liscenced use by ISM standards.

### Manual Override Control Path

Mission Planner interprets joystick input and sends RC override commands via MAVLink. These commands travel through the 915 MHz link, and the Pixhawk forwards the resulting PWM signals to the Arduino motor controller.

### Live Bathymetry Data Transmission
The Communications Subsystem enables the creation of a live bathymetric map on the shore computer by formatting, transmitting, and synchronizing depth and GPS data obtained onboard. The Raspberry Pi receives depth measurements from the Ping1D sonar and positional data from the Pixhawk autopilot over MAVLink. The Pi combines these inputs into timestamped data packets that include latitude, longitude, depth, system time, and status flags. These packets are transmitted once per second over the 915 MHz telemetry link to the shore computer.

On the ground station, Mission Planner (or custom visualization software) receives these MAVLink-formatted messages and plots each depth reading as a point on a real-time map. As the vessel moves, Mission Planner continuously updates the graphical display, interpolating depth points into a contour or color-graded bathymetric image. This live rendering allows the operator to observe lake bottom features, verify path coverage, and detect shallow or unexpected regions during operation.

Simultaneously, the Raspberry Pi logs the raw, high-rate depth and GPS data to the onboard SD card. After the mission, this dataset can be imported into GIS or bathymetric-processing software (e.g., QGIS, MATLAB, ReefMaster) to produce a higher-resolution and more detailed final bathymetric map. Thus, the subsystem supports both real-time situational awareness and high-accuracy post-processing.

##### The Raspberry Pi reads:
-Depth from Ping1D sonar (UART)
-GPS/time from Pixhawk (USB/UART)
-It then packages depth + position into MAVLink packets sent into the telemetry stream. The shore computer displays a real-time bathymetric map.

### SD-Card Logging

All depth + GPS data are continuously logged locally on the Raspberry Pi to guarantee total data retention even if the telemetry link drops.


## Interface with Other Subsystems

The Communications Subsystem interacts with four major subsystems of the autonomous vessel: **Hardware**, **Navigation**, **Sensors**, and **Power**. These interfaces enable wireless telemetry, manual override, live depth transmission, and data logging for the overall mission.

---

### Hardware Subsystem Interface

#### Mechanical Mounting  
The Communications Subsystem is housed within the waterproof electronics container supplied by the Hardware Subsystem.  
- The 3DR SiK radio, Pixhawk, and Raspberry Pi mount to standoffs and brackets supplied by the hull structure.  
- The hardware subsystem ensures environmental protection (waterproofing, ventilation, cable pass-through glands).

#### Antenna Placement  
The Hardware Subsystem provides an elevated or externally mounted antenna location for the 915 MHz radio to maximize line-of-sight communication.

#### Cable Routing  
All UART, USB, and power cables between communication components (Pixhawk, Pi, radio, Ping1D) pass through watertight glands or sealed ducts provided by the Hardware Subsystem.

**Data Exchanged:** None (mechanical + environmental only).  
**Purpose:** Physically secure and protect communication equipment; provide antenna clearance and structural support.

---

### Navigation Subsystem Interface (GPS + Autopilot)

The Communications Subsystem connects tightly to the Navigation Subsystem, which consists of the **Pixhawk autopilot**, **GPS**, and associated IMU sensors.

#### Telemetry Input/Output
- The Pixhawk sends **GPS**, **EKF state**, **battery status**, **heading**, and **vessel attitude** to the Communications Subsystem via MAVLink.
- The Communication Subsystem sends **operator commands**, **RC override signals**, and **mission updates** back to the Pixhawk.

#### Manual Override Path  
Mission Planner → 3DR Radio (ground) → 3DR Radio (air) → Pixhawk → propulsion.

#### Ping1D Depth Input  
The Ping1D sonar is connected **directly to the Pixhawk** via UART.  
- Pixhawk publishes depth as MAVLink `DISTANCE_SENSOR` or `RANGEFINDER` messages.  
- These MAVLink messages are forwarded through the 3DR radio to the shore computer.

#### Companion Computer Link (Pixhawk ↔ Raspberry Pi)
- USB connection provides MAVLink telemetry to the Raspberry Pi.
- The Pi subscribes to MAVLink messages (depth + GPS) for SD logging and optional processing.

**Data Sent (Navigation → Communications):**  
- GPS position, velocity, heading  
- IMU/EKF state estimates  
- Depth (Ping1D)  
- Vehicle status, battery voltage, mode  
- Heartbeats and autopilot flags  

**Data Sent (Communications → Navigation):**  
- RC override commands  
- Operator control inputs  
- Mission start/stop commands  
- MAVLink messages generated by the Pi (if needed in future expansions)

---

### Sensors and Data Acquisition Subsystem Interface

The Sensors Subsystem includes **flow sensors, proximity sensors, and ultrasonic obstacle sensors**, and the **Ping1D**.

#### Sensor Data Uplink to Communications
The Arduino system aggregates environmental data (flow rate, obstacle detection, etc.) and sends **summary or auxiliary data** to the Raspberry Pi if needed.

However, **bathymetry and navigation data used for live mapping originate from Pixhawk**, not the Arduino.

#### Optional Data Path  
Arduino → UART → Raspberry Pi → Logged to SD

This ensures consistency in logging all measurements during the mission.

**Data Sent (Sensors → Communications):**  
- Flow rate (YF-S201)  
- Obstacle detection flags (ultrasonic)  
- Additional environmental metrics (if routed to Pi)

**Data Sent (Communications → Sensors):**  
- None; communication is unidirectional for this subsystem unless future expandability requires commands.

---

### Power Subsystem Interface

The Communications Subsystem receives all electrical power from the vessel’s Power Subsystem.

#### Power Inputs  
- **Raspberry Pi:** 5 V @ 3 A regulated supply  
- **3DR Air Radio:** 5 V @ 1.1–1.5 A peak  
- **Pixhawk:** 5 V regulated input from its dedicated power module  
- **Ping1D sonar:**  
  - Powered either by the Pixhawk 5 V peripheral rail **or**  
  - From a dedicated 5 V line (recommended if current draw approaches Pixhawk limits)

#### Grounding  
All communication devices share a **common ground** to ensure MAVLink serial signal integrity.

#### Voltage and Current Monitoring  
The Power Subsystem routes **battery voltage telemetry** to the Pixhawk, which then sends battery levels through MAVLink to the shore computer.

**Data Exchanged (Power → Communications):**  
- Battery voltage (via Pixhawk MAVLink)  
- Power status or current sensing (if enabled on the PDB)

**Data Exchanged (Communications → Power):**  
- None (power subsystem is passive with respect to communications)

---

### Summary of Interface Directions

| Subsystem | Direction | Signals/Data | Interface Type |
|----------|-----------|--------------|----------------|
| Hardware | Physical only | Mounting, structural support | Mechanical |
| Navigation | Bidirectional | Telemetry, GPS, depth, RC override | MAVLink (UART/USB/Radio) |
| Sensors | Primarily Sensors → Comm | Flow, obstacle, auxiliary data | UART (Arduino → Pi) |
| Power | Power → Comm | 5 V regulated, battery telemetry | DC power + MAVLink |

The Communications Subsystem serves as the **central exchange point** for all mission-critical data, providing the wireless link, real-time visualization, SD-card logging, and manual override capability that enables safe and effective lake mapping operations.

## Buildable Schematic 
###### The Communications Subsystem does not require a traditional electrical schematic because it is composed entirely of pre-designed, off-the-shelf modules that interface using standardized connectors. Instead, a detailed block-level interconnection diagram is used to define all electrical and data pathways between the Pixhawk, onboard sensors, Raspberry Pi, telemetry radio, and the shore-based ground station. This format is the clearest and most appropriate method for representing the subsystem because no discrete circuitry, PCB-level routing, or component-level design is present within this subsystem. The block diagram shows each major module — the Pixhawk flight controller, the 3DR SiK 915 MHz telemetry radio, the Ping1D sonar, the Raspberry Pi companion computer, and the regulated 5 V power supply — as isolated functional blocks. Each block is labeled with its interface types (USB, UART, 5 V input, ground, RF link) and connected with clearly defined signal paths. Directional labels identify communication roles, such as TELEM1 TX/RX, SERIALx TX/RX, and USB MAVLink, ensuring that anyone assembling the system can reproduce the exact wiring configuration. Power distribution is also defined at the block level. A fused 5 V line protects the telemetry radio from overcurrent events, while separate regulated 5 V feeds power the Pixhawk, Ping1D sensor, and Raspberry Pi. Ground is treated as a shared common bus, illustrated clearly in the diagram so that installers maintain correct grounding practices across all modules. Finally, the block diagram includes the shore computer and wireless 915 MHz RF link, showing how the ground-side radio interfaces with Mission Planner. This provides a complete end-to-end view from sensors and control hardware onboard the vessel to the operator’s workstation.
<img width="1640" height="833" alt="image" src="https://github.com/user-attachments/assets/b5fb85dd-9392-4386-8ca4-0528930307ac" />

### Buildable Implementation (Pseudocode and Libraries)

Because the Communications Subsystem is implemented primarily in software running on a Raspberry Pi and interacts with off-the-shelf hardware (Pixhawk, 3DR SiK radio, Ping1D sonar, shore computer), a software-oriented “buildable” description is more appropriate than a traditional circuit schematic. This section specifies the required libraries and provides structured pseudocode that defines the behavior of the subsystem. A developer can directly translate this pseudocode into Python 3 on the Raspberry Pi to reproduce the intended functionality.

#### Required Software Environment and Libraries

**Target platform**

- Raspberry Pi 4 running a recent Raspberry Pi OS (Linux)
- Python 3.x (e.g., Python 3.10+)

**Python libraries**

- `pymavlink` – MAVLink protocol handling (decode GPS, depth, status, heartbeats from Pixhawk).
- `pyserial` – Serial port access (used internally by `pymavlink.mavutil` but can be used directly if needed).
- `datetime` (standard library) – Timestamps for logging.
- `csv` or `json` (standard library) – Writing structured log files to the SD card.
- *Optional:* `ping-python` (Blue Robotics Ping library) – Only required if the Ping1D is ever read directly by the Pi instead of via Pixhawk.
- *Optional:* `matplotlib`, `numpy` – Used for offline plotting/verification of bathymetric data, not required for onboard operation.

Mission Planner on the shore computer does not require custom code for this subsystem; it natively consumes MAVLink messages from the 3DR radio and renders a live map.

#### High-Level Responsibilities (Software Summary)

The Communications Subsystem software on the Raspberry Pi must:

1. Open a MAVLink connection to the Pixhawk over USB.
2. Continuously read MAVLink messages (GPS, depth, battery, heartbeats, etc.).
3. Package key fields into a structured record for:
   - Real-time transmission to shore (already handled by Pixhawk + 3DR radio), and  
   - Onboard logging to the SD card.
4. Monitor link health and record when telemetry fails or recovers.
5. Support the live bathymetric mapping pipeline by ensuring each depth measurement is paired with a GPS position and timestamp.

**Psuedocode**

```
#Initialization snippet
# Establish MAVLink connection and prepare SD logging
mav = mavlink_connect("/dev/ttyACM0", baud=115200)
wait_for_heartbeat(mav)

log = open_csv("bathy_log.csv")
write_header(log, fields=["timestamp","lat","lon","depth","battery","mode"])

# Read messages and perform periodic actions
while True:
    msg = mav.read_message()

    if msg:
        update_state_from_message(msg)

    if 1 second has passed:
        write_log_entry(log, state)

    if no heartbeat for >3 seconds:
        state.link_ok = False
```
```
# Extract relevant fields from incoming messages
if msg.type == "GLOBAL_POSITION_INT":
    state.lat = msg.lat / 1e7
    state.lon = msg.lon / 1e7

elif msg.type == "DISTANCE_SENSOR":
    state.depth_m = msg.current_distance / 100.0

elif msg.type == "SYS_STATUS":
    state.batt_voltage = msg.voltage_battery / 1000.0
```
```
# Build a synchronized record at 1 Hz
record = {
    "timestamp": now(),
    "lat": state.lat,
    "lon": state.lon,
    "depth": state.depth_m,
    "battery": state.batt_voltage,
    "mode": state.flight_mode
}

write_csv_row(log, record)
```
```
# Mission Planner receives GPS + depth via 3DR radio
# and updates the live depth map automatically.
send_over_mavlink("GPS_POSITION", state.lat, state.lon)
send_over_mavlink("DEPTH_DATA", state.depth_m)

# The Pi ensures data is synchronized and logged.
```

## Flowchart
<img width="1795" height="1302" alt="image" src="https://github.com/user-attachments/assets/fe803813-ab72-4cf3-b468-0e804182e491" />


## BOM

| Item / Description                            | Manufacturer               | Manufacturer P/N           | Qty | Approx. Unit Cost (USD) | URL                                                                 |
|-----------------------------------------------|----------------------------|-----------------------------|------|--------------------------|---------------------------------------------------------------------|
| Flight Controller – Pixhawk                   | SoloGood / Holybro         | Pixhawk PX4 / 2.4.8         | 1    | ~$120-200                | https://www.amazon.com/dp/B07NRMFTXL :contentReference[oaicite:0]{index=0}            |
| Telemetry Radio Kit 915 MHz                   | Soulload / generic         | 915MHz Telemetry Kit        | 1    | ~$70-90                  | https://www.amazon.com/dp/B0768WQ989 :contentReference[oaicite:1]{index=1}           |
| Raspberry Pi 4 (board only)                   | Raspberry Pi Foundation    | RPI4-4GB-ModelB             | 1    | ~$60-70                  | https://www.amazon.com/dp/B07VFCB192 :contentReference[oaicite:2]{index=2}            |
| microSD Card (Class 10, 32-64 GB)             | SanDisk / Samsung          | (various)                   | 1    | ~$15                     | https://www.amazon.com/dp/B07MXSJJRQ :contentReference[oaicite:3]{index=3}            |
| Fuse (5 V rail for radio)                      | Littelfuse / Nilight       | Blade Fuse / Holder         | 1    | ~$1-5                   | https://www.amazon.com/s?k=inline+5+amp+fuse :contentReference[oaicite:4]{index=4} |

## Analysis

The Communications Subsystem is engineered to ensure reliable, low-latency, and fault-tolerant data exchange between the autonomous lake-mapping vessel and the shore-based ground station. The design also guarantees complete onboard data preservation in the event of communication loss. This analysis demonstrates that the selected hardware architecture, communication protocols, electrical protections, and failsafe mechanisms collectively satisfy all functional requirements and constraints defined during the conceptual and detailed design phases.

### 1. Satisfaction of Functional Requirements

The subsystem must transmit mission-critical data—including GPS positions, depth measurements, health information, and sensor status—to the operator while also receiving commands for manual override and mission control. These requirements are met through the use of a 915 MHz 3DR SiK telemetry radio pair, which provides a long-range MAVLink-compatible communication channel between the Pixhawk and the shore computer running Mission Planner. Frequency-hopping spread spectrum modulation improves resilience to interference, ensuring continuous bidirectional data flow even in moderately congested RF environments. The subsystem therefore achieves its core function of supporting real-time supervisory control and operator intervention.

### 2. Robustness of Mission-Critical Telemetry

The 915 MHz link was specifically chosen for its strong propagation characteristics over water and suitability for low-bandwidth telemetry. Typical performance exceeds 1 km line-of-sight, providing ample margin for expected vessel operating distances. MAVLink heartbeats at 1 Hz allow communication loss to be detected within approximately three seconds, triggering immediate activation of failsafe logic. This rapid detection and response meets system-level safety constraints by preventing uncontrolled vessel behavior during communication failure events.

### 3. Redundant Communication Pathways

To ensure high reliability, the subsystem implements dual communication channels:  
(1) a radio-based MAVLink link over TELEM1 for primary telemetry and operator commands, and  
(2) a USB MAVLink link between the Pixhawk and Raspberry Pi for high-level data services.  

This redundancy ensures continued control and data integrity even if one pathway fails. The Pixhawk maintains independent connectivity to the ground station regardless of Raspberry Pi state, and the Raspberry Pi can continue logging sensor data even if the 915 MHz radio link drops. This architecture satisfies critical reliability, autonomy, and safety requirements.

### 4. Data Throughput and Protocol Efficiency

The Ping1D sonar provides depth readings at rates up to 20 Hz, generating modest data volumes (typically < 600 bytes/s). The Raspberry Pi efficiently aggregates sonar data with GPS, timestamp, and status information before either logging it to the SD card or forwarding low-rate summary packets over the radio link. By transmitting only essential data to shore and logging full-resolution data locally, the subsystem avoids saturating the RF link. This strategy ensures stable performance under bandwidth constraints and meets the requirement for timely operator situational awareness.

### 5. Fault Tolerance and Failsafe Behavior

A key design constraint is the requirement to maintain safe vessel operation during partial or complete communication loss. The subsystem accomplishes this through layered failsafe mechanisms implemented by the Pixhawk and the Raspberry Pi. If telemetry heartbeats are lost, the Pixhawk autonomously engages a failsafe mode such as loitering, returning to the launch point, or retracing its previous path, depending on mission configuration. Concurrently, the Raspberry Pi continues logging all sensor and navigation data to the SD card to preserve mission integrity. Automatic reconnection upon link recovery fulfills the requirement for smooth transitions between communication states and adheres to ethical and safety standards.

### 6. Electrical and Environmental Safety Considerations

The communications hardware is powered from the regulated 5 V rail supplied by the Power Subsystem. A dedicated inline fuse protects the radio from overcurrent conditions, wiring faults, or moisture-induced shorts. This prevents a localized failure from collapsing the shared 5 V rail, which also powers the Raspberry Pi and contributes to Pixhawk operation. The fuse thereby isolates faults, mitigates fire risk, and ensures system-level robustness. All components are housed in a sealed, vibration-resistant enclosure with cable glands and strain relief, meeting environmental durability constraints necessary for outdoor freshwater deployment.

### 7. Alignment With System-Level Objectives

The Communications Subsystem supports both autonomous and manual operational modes, enabling real-time visualization of depth data, vessel location, and system health while preserving complete logs for post-mission bathymetric processing. Its architecture is modular, scalable, and compatible with existing marine robotics frameworks, ensuring seamless integration with the Navigation, Power, Hardware, and Sensor subsystems. The design’s reliability, resilience, and safety protections enable the vessel to meet its performance goals and mission objectives effectively.


## References

[1] Blue Robotics, “Ping1D Echosounder—Technical Specifications,” Blue Robotics Documentation, 2024.  
      Available: https://bluerobotics.com/learn/ping1d/

[2] ArduPilot Dev Team, “MAVLink 2.0 Messaging Protocol,” MAVLink Developer Guide, 2024.  
      Available: https://mavlink.io/en/

[3] PX4 Autopilot Community, “Pixhawk Series Hardware Overview,” PX4 Documentation, 2024.  
      Available: https://docs.px4.io/main/en/hardware/pixhawk_series.html

[4] 3DR / Holybro, “SiK Telemetry Radio—915 MHz Long-Range Data Link,” Product Documentation, 2024.  
      Available: https://holybro.com/collections/telemetry-radios

[5] Raspberry Pi Foundation, “Raspberry Pi 4 Model B—Hardware and Interface Specifications,” Raspberry Pi Documentation, 2024.  
      Available: https://www.raspberrypi.com/documentation/

[6] SanDisk, “High-Endurance microSD Cards—Class 10 / UHS-I Specifications,” SanDisk Storage Specifications, 2024.

[7] Littelfuse, “Axial and Blade Fuses—Overcurrent Protection Basics,” Littelfuse Component Datasheets, 2023.  
      Available: https://www.littelfuse.com/

[8] Mission Planner Development Team, “Mission Planner Ground Control Station—User Guide,” ArduPilot Mission Planner Documentation, 2024.  
      Available: https://ardupilot.org/planner/

[9] IEEE Standards Association, “IEEE Recommended Practice for Powering Remote and Embedded Systems,” IEEE Std. Guidelines, 2020.

[10] T. B. Sheridan, “Spacecraft, Subsea, and Aerial Vehicle Telemetry Principles,” *Systems and Control Engineering*, vol. 12, no. 3, pp. 145–159, 2019.

