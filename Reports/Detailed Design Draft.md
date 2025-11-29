# Detailed Design

## Function of the Subsystem

The Sensors and Data Acquisition Subsystem provides the autonomous vessel with real-time awareness of both the underwater environment and nearby surface-level obstacles. Its primary role is to convert raw physical measurements into structured, time-aligned digital information that can be used by the vessel’s navigation, control, and communication subsystems. In the context of the overall conceptual design, this subsystem forms the foundational input layer for all mapping, autonomy, and safety-related behaviors. The subsystem fulfills four core functions:

#### 1. Underwater Depth Measurement
The subsystem determines water depth using the Blue Robotics Ping1D sonar transducer.[1] In the final system architecture, the Ping1D connects directly to the Pixhawk flight controller over a UART interface and is configured within ArduPilot as a rangefinder device.[2] This allows the Navigation Subsystem to ingest depth measurements natively as a rangefinder input, enabling real-time bathymetric awareness and depth-aware navigation.

#### 2. Short-Range Obstacle Detection
The subsystem detects nearby obstacles—such as docks, lake edges, debris, or other surface-level objects—using LM393 infrared (IR) sensors.[3] These sensors provide immediate short-range situational awareness for collision avoidance and safety. Their outputs are processed by the Arduino Uno R3.

#### 3. Local Data Acquisition and Logging
The Arduino Uno serves as the dedicated data acquisition node for all non-Pixhawk-managed sensors.[4] It reads IR sensor states, timestamps them, and logs them to a microSD card using an SPI-based SD-card interface module.[5] This ensures that obstacle data and subsystem status information are preserved for offline analysis, even if higher-level systems experience communication interruptions.

#### 4. Data Transfer to Higher-Level Systems
Processed IR and subsystem-status data are streamed from the Arduino to the Raspberry Pi 4 via USB serial.[6] The Raspberry Pi fuses this information with Pixhawk navigation and depth data, synchronizes it with GPS, and relays it to the base station. Depth data from the Ping1D reaches the Raspberry Pi indirectly through the Pixhawk over MAVLink telemetry.[7]

#### Summary

Overall, the Sensors and Data Acquisition Subsystem ensures that the autonomous vessel has continuous, reliable, and well-structured environmental data—both above and below the water surface—to support navigation, mapping, autonomy, and safety objectives defined in the conceptual design.

## Specifications and Constraints

#### 1. Functional and Performance Specifications

##### SDS-F-01 — Depth Measurement Range
- The subsystem shall measure water depth from 0.5 m to at least 100 m using the Blue Robotics Ping1D sonar.[1]
- Rationale: Limited by acoustic attenuation in freshwater and Ping1D hardware capabilities.[1],[2]

##### SDS-F-02 — Depth Accuracy and Resolution
- The system shall achieve ≤1 cm resolution and ≤10 cm typical absolute error within operational depth ranges.[1]
- Rationale: Required for meaningful bathymetric mapping and consistent with Ping1D timing precision and beam characteristics.[1],[2]

##### SDS-F-03 — Depth Update Rate
- The subsystem shall provide depth measurements at ≥10 Hz via the Pixhawk’s rangefinder interface.[2]
- Rationale: Ensures spatial continuity at typical vessel speeds (0.5–1.0 m/s) and is supported by Ping1D and ArduPilot rangefinder integration.[1],[2]

##### SDS-F-04 — Obstacle Detection Range
- IR sensors shall detect nearby objects within 2–30 cm.[3]
- Rationale: LM393 IR obstacle-avoidance modules are specified for short-range detection with typical adjustable detection distances from approximately 2 cm to 30 cm.[3]

##### SDS-F-05 — IR Data Logging Frequency
- The subsystem shall log IR obstacle data at ≥5 Hz.
- Rationale: Balances logging bandwidth, microcontroller limitations, and temporal fidelity; 5 Hz is well within Arduino and SD-card capabilities.[4],[5]

##### SDS-F-06 — Data Output Format
- Arduino-managed data shall be exported in CSV or JSON, while Ping1D depth shall be exported via Pixhawk’s MAVLink rangefinder-related messages.[2],[7]
- Rationale: Ensures interoperability with the Raspberry Pi 4 and compatibility with navigation and logging software that already understand MAVLink and text-based data formats.[6],[7]

⸻

#### 2. Electrical and Power Constraints

##### SDS-E-01 — Operating Voltages
- All components shall operate at:
    - 5 V: Arduino Uno, Ping1D (transducer interface), LM393 IR modules[1],[3],[4]
    - 3.3 V: SD card interface logic (via onboard regulator or level-shifting)[5]
- Rationale: Ensures conformance to manufacturer electrical requirements and avoids overvoltage damage.[1],[3]  

##### SDS-E-02 — Maximum Current Draw
- The subsystem shall not exceed 1.5 A total draw from the 5 V supply.
- Rationale: Ensures compatibility with the Power Subsystem and accounts for Ping1D peak demand (~900 mA) plus Arduino, IR sensors, and SD module margins.[1],[4],[5]

##### SDS-E-03 — Logic-Level Compatibility
- 5 V logic devices (Arduino, LM393) shall not interface directly with 3.3 V-only devices without level shifting.
- Rationale: Prevents electrical overstress of Raspberry Pi and SD interface hardware; Raspberry Pi GPIO and many SD interfaces are 3.3 V-only.[5],[6]

##### SDS-E-04 — Pixhawk UART Requirements
- Ping1D must use a UART port that supports the appropriate logic levels and configuration, or level shifting must be applied.[1],[2]
- Rationale: Ensures safe and reliable electrical interfacing with the Pixhawk’s SERIAL/TELEM port and correct operation of ArduPilot’s rangefinder driver.[2]

⸻

#### 3. Environmental and Mechanical Constraints

##### SDS-ENV-01 — Temperature Range
- The subsystem shall operate reliably between 0 °C and 40 °C.
- Rationale: Matches typical Tennessee lake environments and the approximate operating range of the consumer-grade electronics used (Arduino Uno, Raspberry Pi, Ping1D, and associated modules).[1],[4],[6]

##### SDS-ENV-02 — Water Exposure Requirements
- Only the Ping1D transducer may be submerged. All electronics must remain inside an enclosure rated IP54 or better.
- Rationale: The Ping1D transducer is designed for underwater use, whereas the other electronics are not water-sealed and must be protected from spray and splashes.[1]

##### SDS-ENV-03 — Mechanical Stability
- The Ping1D transducer must be mounted vertically downward with minimal relative vibration.
- Rationale: Ensures depth accuracy; acoustic beam geometry and echo timing are sensitive to orientation and mechanical vibration.[1],[2]

##### SDS-ENV-04 — Cable Routing and Strain Relief
- Sensor cables must be protected from abrasion and must include strain-relief at connection points.
- Rationale: Prevents connector fatigue and signal degradation during vessel motion and long-term operation.

⸻

#### 4. Standards-Based Constraints

##### SDS-STD-01 — Electrical Safety Standards
- Wiring, grounding, and insulation must follow IEC 61140 and general low-voltage marine wiring guidelines.[8]
- Rationale: Ensures safe operation, reduces shock hazards, and is consistent with recognized international electrical safety practices.[8]

##### SDS-STD-02 — Data Standardization
- All logged numeric values shall follow IEEE-754 floating-point representation and UTF-8 encoding.[9],[10]
- Rationale: Ensures portable, tool-agnostic storage of numeric and text data and compatibility with standard scientific and software tooling.[9],[10]

##### SDS-STD-03 — MAVLink Protocol Compliance
- Depth-related messages delivered via Pixhawk must conform to MAVLink standard rangefinder-related message definitions (e.g., common message set) where applicable.[7]
- Rationale: Ensures interoperability with autopilots and ground stations that rely on standardized MAVLink definitions.[7]

⸻

#### 5. Ethical Constraints

##### SDS-ETH-01 — Minimization of Environmental Disturbance
- Sonar emissions shall remain within manufacturer-specified safe acoustic levels for freshwater ecosystems.[1]
- Rationale: Complies with ethical environmental stewardship expectations and typical usage scenarios described for low-cost underwater sonar systems.[1],[2]

##### SDS-ETH-02 — Integrity of Logged Environmental Data
- Recorded sensor logs must remain unmodified and tamper-free.
- Rationale: Prevents data manipulation and supports honest scientific reporting and reproducible analysis.

⸻

#### 6. Socio-Economic Constraints

##### SDS-SOC-01 — Cost Limitation
- Total subsystem cost shall not exceed $400.
- Rationale: Ensures affordability for academic replication and meets overall project budgeting requirements.

##### SDS-SOC-02 — Reliance on Accessible Hardware
- All components must be open-source, low-cost, or commonly available (Arduino Uno, LM393 modules, Ping1D, etc.).[1],[3],[4],[5],[6]
- Rationale: Supports long-term maintainability and accessibility for future student teams and community users.

⸻

#### 7. Subsystem Interdependency Constraints

##### SDS-SYS-01 — Navigation Dependence
- Ping1D depth must be provided directly to the Pixhawk for use in navigation and bathymetry.[1],[2]
- Rationale: Ensures depth becomes part of the Navigation Subsystem’s internal state and improves real-time autonomy.

##### SDS-SYS-02 — Communication Dependence
- IR data and subsystem statuses shall be streamed from the Arduino to the Raspberry Pi for time alignment and integration.[4],[6]
- Rationale: Enables system-wide data fusion and ensures synchronized sensing across subsystems.

##### SDS-SYS-03 — Power Dependence
- The subsystem requires a regulated 5 V rail from the Power Subsystem and does not perform its own voltage regulation (aside from SD module).[4],[5]
- Rationale: Reduces subsystem complexity and places power-conditioning responsibility on the dedicated power distribution system.



## Overview of Proposed Solution

The proposed solution implements a cohesive Sensors and Data Acquisition Subsystem that fulfills all functional, performance, environmental, and system-integration specifications. The subsystem is designed around two complementary sensing pathways: a direct depth-to-navigation path through the Pixhawk and a local IR-to-Arduino path for obstacle awareness. These pathways allow the entire system to obtain continuous, high-resolution environmental data needed for safe autonomous navigation and lake-bottom mapping.

⸻

#### 1. Sensor Selection and Integration

The subsystem integrates two primary sensing technologies:

##### Blue Robotics Ping1D Sonar (Depth Sensing)

The Ping1D sonar provides accurate depth measurements up to approximately 100 meters with centimeter-level resolution, intended for use in underwater distance and depth measurement applications.[1],[2] In the finalized architecture, the Ping1D connects directly to a Pixhawk UART/TELEM port, allowing ArduPilot to interpret the sonar as a rangefinder.[2] This direct integration ensures:
- High-rate (≥10 Hz) depth acquisition
- Low-latency incorporation into the navigation stack
- Native MAVLink data formatting for downstream systems[2],[7]

##### LM393 IR Proximity Sensors (Obstacle Detection)

LM393 infrared sensors provide reliable short-range (about 2–30 cm) obstacle detection using an IR emitter/receiver pair and a comparator circuit.[3] Each IR module connects to the Arduino Uno through a digital pin, allowing for fast polling and timestamped event detection.[4] These sensors give the vessel real-time awareness of nearby objects, essential for avoiding docks, hull collisions, and shoreline contact.

⸻

#### 2. Data Acquisition Architecture

The subsystem uses a distributed acquisition strategy that assigns each type of sensor to the hardware best suited for processing it.

##### Pixhawk (Depth Path)
- Collects Ping1D depth and confidence values directly over UART.[1],[2]
- Integrates depth data into ArduPilot’s internal state via rangefinder drivers.[2],[7]
- Publishes synchronized depth, attitude, and position via MAVLink.[7]

##### Arduino Uno (IR + Subsystem Monitoring Path)

The Arduino Uno serves as the data acquisition unit for all non-Pixhawk sensors.[4]

It performs the following roles:
- Reads IR obstacle states from LM393 modules[3],[4]
- Timestamps and formats the IR measurements
- Logs IR data to a microSD card using SPI (≥5 Hz)[4],[5]
- Streams structured IR and subsystem-status messages via USB to the Raspberry Pi[4],[6]

This architecture matches the subsystem’s performance requirements while keeping the sensing pathways independent and robust.

⸻

#### 3. Data Management and Communication Strategy

The subsystem uses redundant and complementary data pathways:

##### Depth Data (Ping1D → Pixhawk)
- Native integration via ArduPilot’s rangefinder interface[2],[7]
- Automatically synchronized with GPS, IMU, compass, and velocity data
- Recorded on Pixhawk SD logs and forwarded to the Raspberry Pi via MAVLink[7]

##### IR + Subsystem Status Data (Arduino → Raspberry Pi)
- Streamed via USB CDC serial in CSV or JSON format[4],[6]
- Logged on the Raspberry Pi alongside navigation data
- Combined with Pixhawk depth for complete situational datasets

This two-path strategy satisfies the requirements for reliability, redundancy, and time-synchronized environmental sensing (SDS-F-05, SDS-ETH-02).

⸻

#### 4. Mechanical and Environmental Considerations

To fulfill environmental constraints and maintain measurement accuracy:

- The Ping1D is mounted vertically downward on a vibration-controlled bracket below the hull waterline.[1]
- The IR sensors are mounted at outward-facing angles near the bow and hull edges for optimal obstacle detection.[3]
- All electronic components except the Ping1D transducer are enclosed in an IP54-rated housing with strain-relieved cable pass-throughs.
- Power distribution and cable routing comply with IEC low-voltage wiring guidelines and minimize electrical noise.[8]

These design choices ensure durability, measurement fidelity, and safe operation in the aquatic environment.

⸻

#### 5. Power and Electrical Design

The subsystem draws power from the vessel’s regulated 5 V rail, supplied by the Power Subsystem. To meet the electrical specifications:

- Ping1D peak demand (~900 mA) is accounted for in the current budget[1]
- The Arduino and IR sensors share the same regulated 5 V line[3],[4]
- The SD card receives 3.3 V from its onboard regulator or logic-level converter[5]
- Decoupling capacitors near major loads (Ping1D, SD module) stabilize transient current spikes[1],[5]

This design ensures electrical stability, noise reduction, and compliance with SDS-E-01 through SDS-E-03.

⸻

#### 6. System-Level Integration

The proposed design integrates seamlessly with the broader autonomous vessel architecture:

##### Sensors Subsystem → Navigation Subsystem
- Ping1D depth delivered directly to Pixhawk for use in ArduPilot rangefinder processing[1],[2]
- IR data optionally forwarded to Pixhawk via Raspberry Pi if extended behaviors are implemented

##### Sensors Subsystem → Communication Subsystem
- Arduino’s IR and subsystem-status data delivered over USB[4],[6]
- Raspberry Pi merges Ping1D depth and IR data into unified logs[2],[6],[7]
- Telemetry streamed live to the base station

##### Sensors Subsystem → Mapping and Mission Software
- Depth + GPS → bathymetric map
- IR awareness → safety and obstacle avoidance

This cohesive integration ensures that the subsystem fulfills all functional requirements and contributes reliably to autonomous mission execution.

⸻

#### Summary

The proposed solution meets all specifications and constraints by:
- Using direct Pixhawk integration for high-rate depth measurement[1],[2]
- Using an Arduino for auxiliary IR sensors and local logging[3],[4],[5]
- Maintaining high reliability through redundant data paths
- Ensuring electrical and environmental robustness[1],[3],[4],[5],[8]
- Providing clean, structured data to higher-level systems[6],[7],[9],[10]

It is modular, maintainable, and precisely aligned with the conceptual design objectives of the overall autonomous vessel.



## Interface with Other Subsystems

The Sensors and Data Acquisition Subsystem interacts with four major subsystems—Communication, Navigation, Hardware, and Power—to ensure accurate sensing, reliable data flow, and safe operation of the autonomous vessel. This section defines each interface in terms of data, signals, physical wiring, and subsystem dependencies.

⸻

#### 1. Communication Subsystem — Raspberry Pi 4

##### Purpose of Interface

The Communication Subsystem receives IR-obstacle data and subsystem-status information from the Arduino, and also receives depth information indirectly from the Pixhawk via MAVLink. The Raspberry Pi fuses these data streams, logs them, and forwards relevant information to the base station.[2],[6],[7]

##### Physical Connection
- USB Type A → USB Type B cable (Raspberry Pi → Arduino Uno)
- Arduino enumerates as a USB CDC Serial Device[4],[6]

##### Data Flow (Arduino → Raspberry Pi)

The Arduino transmits structured sensor readings at ~10 Hz:

| Field | Description | Units | Source |
|---|---|---|---|
| t_ms | Sensor timestamp since boot | ms | Arduino |
| status | Subsystem status code | - | Arduino |
| ir_front | Obstacle detection flag | 0/1 | IR Sensor |
| ir_starboard | Obstacle detection flag | 0/1 | IR Sensor |
| ir_port | Obstacle detection flag | 0/1 | IR Sensor |


##### Example CSV Output:

12345.4.27.0.92.0.1.0

##### Example JSON Output:

{
    "t_ms": 12345,
    "depth_m":  4.27,
    "confidence":   0.92,
    "ir_front": 0,
    "ir_starboard": 1,
    "ir_port":  0
}

##### How the Communication Subsystem Uses This Data
- Aligns Arduino IR timestamps with GPS time
- Fuses IR obstacle data with Pixhawk depth and navigation telemetry
- Generates unified mission logs
- Publishes processed telemetry to the base station
- Displays real-time situational visuals

**Important:** Ping1D depth does not come directly from the Arduino. It reaches the Raspberry Pi through Pixhawk MAVLink messages.[2],[7]



#### 2. Navigation Subsystem - Pixhawk 2.4.8 + GPS via Raspberry Pi 4

##### Purpose of Interface

The Navigation Subsystem requires real-time depth to maintain situational awareness and generate bathymetric maps. The Ping1D connects directly to the Pixhawk over UART to guarantee precise timing and seamless firmware integration.[1],[2] IR data is available to the Pixhawk indirectly via MAVLink through the Raspberry Pi.

##### Physical Connection (Direct)

Ping1D → Pixhawk SERIAL/TELEM Port (UART)
- Ping1D TX → Pixhawk RX
- Ping1D RX → Pixhawk TX
- Ping1D V+ → 5 V rail
- Ping1D GND → system ground[1],[2]

##### Logical/Data Interface

Pixhawk receives:

| Field | Description | Units | Source |
|---|---|---|---|
| depth_m | Water depth | meters | Ping1D via UART |
| confidence | Sonar return quality | 0–1 | Ping1D |

These are interpreted by ArduPilot’s RANGEFINDER driver and published via MAVLink.[2],[7]

##### Indirect Data Path (via Raspberry Pi → MAVLink)

Pixhawk can also be provided with:
- IR obstacle flags (if desired in future behavior modules)
- Combined sensor fusion data from the Raspberry Pi

##### How the Navigation Subsystem Uses the Data
- Integrates depth into navigation solution
- Constructs bathymetric mapping[2],[7]
- Monitors depth confidence to detect unreliable readings
- Supports hazard awareness through additional IR data if integrated

##### Feedback to Sensors Subsystem
- No direct control commands to IR sensors or Arduino
- Ping1D configuration (update rate, min/max range) is controlled via Pixhawk parameters[2]



#### 3. Hardware Subsystem - Mechanical and Structural

##### Purpose of Interface

To ensure sensors are correctly mounted, environmentally protected, and mechanically supported, enabling reliable data collection.

##### Inputs Received from Hardware Subsystem
- Mounting Brackets
  - Rigid, vertical, downward-facing mount for Ping1D[1]
  - Adjustable-angle mounts for IR sensors[3]
- Enclosure Requirements
  - IP54+ rating for electronics housing
  - Cable glands for Ping1D and IR sensor wiring
- Cable Routing
  - Separation of signal/power cables to avoid interference
  - Strain relief on transducer and IR sensor cables

##### Outputs Provided to Hardware Subsystem
- Required installation orientation (Ping1D vertical, IR sensors outward)[1],[3]
- Mechanical dimensions, hole spacing, and mounting clearances
- Internal space requirements for Arduino, SD module, wiring, and connectors
- Cabling guidelines to maintain UART and SPI signal integrity

#### 4. Power Subsystem

##### Purpose of Interface:

To supply stable and clean electrical power to all sensors and acquisition electronics.

##### Electrical Inputs to Sensors Subsystem:

| Line | Source | Requirement |
|---|---|---|
| 5V rail | Power Subsystem | 4.75-5.25V, ≥1.5 A capacity |
| Ground (GND) | Power Subsystem | Shared with Pixhawk, Pi, and Arduino |

##### Internal Power Distribution
- Ping1D: 5 V → high-current load[1]
- Arduino Uno: 5 V → microcontroller + IR sensors[3],[4]
- IR Sensors: 5 V for emitter + LM393 comparator[3]
- SD Module: 3.3 V regulator onboard or module-provided[5]
- Decoupling capacitors used at key nodes (Ping1D, SD module)[1],[5]

##### Outputs to Power Subsystem
- Estimated current draw (~1.2–1.5 A total)
- Electrical noise characteristics (Ping1D current transients)
- Grounding and isolation requirements

##### Power Subsystem Does Not Provide
- Data lines
- Logic-level control
- Communication with sensors



#### Summary of All Interfaces:

| Subsystem | Inputs to Sensors Subsystem | Outputs from Sensors Subsystem | Communication Method | Notes |
|---|---|---|---|---|
| Communication (Pi 4) | None | IR data, timestamps, subsystem status | USB CDC Serial (Arduino ↔ Pi) | Depth arrives from Pixhawk → Pi |
| Navigation (Pixhawk) | 5 V, GND to Ping1D | Depth + confidence | (Ping1D direct), IR data (indirect) | UART (Ping1D ↔ Pixhawk) + MAVLink | Main navigation data path |
| Hardware | Mounts, enclosure, cable routing | Orientation specs, mechanical constraints | Mechanical | Ensures accurate measurements |
| Power | 5 V, GND | Current draw details, transient characteristics | Direct wiring | Shared ground with entire vessel |



## 3D Model of Custom Mechanical Components

N/A

## Buildable Schematic 

The following schematic provides all wiring details for the Sensors and Data Acquisition Subsystem. It includes the Ping1D Sonar (connected directly to the Pixhawk over UART), the IR proximity sensors connected to the Arduino Uno, the SD card module SPI wiring, shared 5 V power distribution, and the USB serial connection to the Raspberry Pi. This schematic is intended to be buildable by someone with no prior knowledge of the subsystem. All connectors, pins, and nets are clearly labeled.[1],[3],[4],[5],[6]

<div style="text-align:center;">

<img width="595" height="419" alt="Sensors_Subsystem (dragged) Rev 2" src="https://github.com/user-attachments/assets/ceb704ac-85c8-489b-b80b-246f893c55fa" />
 
**Figure 3.1 – System-Level Interface Schematic**

<img width="595" height="419" alt="Sensors_Subsystem (dragged) 2 Rev2" src="https://github.com/user-attachments/assets/17e6949e-a3c1-4e4f-a6ff-f0d6879288c5" />


**Figure 3.2 – Detailed Electrical Schematic**

</div>


##### The schematic is divided into distinct functional sections for readability:
- Sensor Interfaces: Ping1D sonar and IR modules
- Microcontroller Interfaces: Arduino Uno pins and logic-level mappings
- Data Interfaces: USB serial connection to the Raspberry Pi
- Navigation Interface: UART wiring to the Pixhawk TELEM/SERIAL port
- Power Distribution: Common +5V rail and ground reference
- Local Data Logging: SD card module on the SPI bus

This diagram represents the complete wiring requirements necessary to construct the working subsystem.

## Printed Circuit Board Layout

N/A


## Flowchart

The following flowchart illustrates the high-level decision-making process executed by the Arduino Uno within the Sensors and Data Acquisition Subsystem. This process represents the continuous microcontroller loop responsible for reading IR sensors, timestamping the data, generating structured records, logging those records to the SD card, and transmitting the data to the Communication Subsystem via USB serial.[3],[4],[5],[6] Since the Ping1D sonar interfaces directly with the Pixhawk, its processing is not handled by the Arduino and is therefore not included in this flowchart.

<div style="text-align:center;">

<img width="612" height="792" alt="Sensors_Flowchart_V2" src="https://github.com/user-attachments/assets/cc5b706c-4af7-4bdc-8431-17131821d613" />


**Figure 5.1 — Arduino Sensor Subsystem Flowchart**

</div>

This loop executes continuously (“loop forever”) as part of the Arduino’s main control cycle until the system is powered off. The diagram provides an overview-level representation appropriate for detailed design documentation and avoids unnecessary implementation details, focusing instead on subsystem behavior and data movement.

## Bill of Materials

| Ref Des | Manufacturer / Brand | Part Number | Description | Distributor | Dist. Part Number | Qty | Unit Price | Total | URL |
|--------|------------------------|-------------|-------------|-------------|--------------------|-----|------------|--------|-----|
| U1 | Arduino | A000066 | Arduino Uno R3 Microcontroller Board | Arduino / Digi-Key | 1050-A000066-ND | 1 | $28.50 | $28.50 | https://store.arduino.cc/products/arduino-uno-rev3 |
| J4 | Blue Robotics | BR-110 (Ping Sonar) | Ping Sonar Altimeter / Echosounder (successor to Ping1D) | Blue Robotics | BR-110 | 1 | $430.00 | $430.00 | https://bluerobotics.com/store/sonars/echosounders/ping-sonar-r2-rp/ |
| J5 | Aexit | LM393-AEXIT | IR Obstacle Avoidance Module (Front) | Amazon | B07DDB6NMN | 1 | $6.79 | $6.79 | https://www.amazon.com/Aexit-Infrared-Obstacle-Avoidance-Vibration/dp/B07DDB6NMN |
| J6 | Aexit | LM393-AEXIT | IR Obstacle Avoidance Module (Starboard) | Amazon | B07DDB6NMN | 1 | $6.79 | $6.79 | https://www.amazon.com/Aexit-Infrared-Obstacle-Avoidance-Vibration/dp/B07DDB6NMN |
| J7 | Aexit | LM393-AEXIT | IR Obstacle Avoidance Module (Port) | Amazon | B07DDB6NMN | 1 | $6.79 | $6.79 | https://www.amazon.com/Aexit-Infrared-Obstacle-Avoidance-Vibration/dp/B07DDB6NMN |
| J8 | KEYESTUDIO / Generic | KS0068 | MicroSD SPI Storage Module w/ Logic-Level Shifting | Amazon | B07PFDFPPC | 1 | $5.99 | $5.99 | https://www.amazon.com/Module-Storage-Adapter-Interface-Arduino/dp/B07PFDFPPC |
| C1 | Murata | GRM188R71C104KA01D | 0.1 µF (100 nF), 16 V MLCC Bypass Capacitor | Digi-Key | 490-1591-1-ND | 1 | $0.10 | $0.10 | https://www.digikey.com/en/products/detail/murata-electronics/GRM188R71C104KA01D/587012 |
| C2 | Panasonic | EEU-FR1A100 | 10 µF, 10 V Electrolytic Capacitor | Digi-Key | P12472-ND | 1 | $0.40 | $0.40 | https://www.digikey.com/en/products/detail/panasonic-electronic-components/EEU-FR1A100/2688678 |
| C3 | Murata | GRM188R71C104KA01D | 0.1 µF (100 nF), 16 V MLCC Bypass Capacitor | Digi-Key | 490-1591-1-ND | 1 | $0.10 | $0.10 | https://www.digikey.com/en/products/detail/murata-electronics/GRM188R71C104KA01D/587012 |
| J1 | JST / SparkFun | PRT-09914 | JST-PH 2-Pin Connector Kit (Power Connector) | SparkFun | PRT-09914 | 1 | $1.50 | $1.50 | https://www.sparkfun.com/products/9914 |
| J2 | Adafruit | 1764 | USB Type B Breakout Board | Adafruit | 1764 | 1 | $1.95 | $1.95 | https://www.adafruit.com/product/1764 |
| J3 | JST | GH-B04B-PH-K | 4-Pin JST-GH Connector (Pixhawk UART) | Digi-Key | 455-1802-ND | 1 | $1.20 | $1.20 | https://www.digikey.com/en/products/detail/jst-sales-america-inc/GH-B04B-PH-K/620211 |
| — | Adafruit | 1957 | 22 AWG Silicone Hook-Up Wire Kit (High-Flex, 6-color) | Adafruit | 1957 | 1 | $9.95 | $9.95 | https://www.adafruit.com/product/1957 |

### **Total Cost = $513.56**


## Analysis

The Sensors and Data Acquisition Subsystem has been designed to satisfy the functional, electrical, environmental, standards-based, ethical, and socio-economic constraints defined earlier, while providing the information required for safe autonomous navigation and bathymetric mapping. This section analyzes how the chosen architecture and components support those goals and why the design is expected to perform as intended.

#### 1. Functional Performance Analysis

##### Depth Measurement (Ping1D → Pixhawk)
The Blue Robotics Ping1D sonar is specified for underwater distance and depth measurement with a maximum range on the order of 100 m and a relatively narrow beam angle, suitable for altimetry and bathymetry tasks.[1],[2] By interfacing the Ping1D directly to a Pixhawk serial/UART port and configuring it as a rangefinder within ArduPilot, the subsystem leverages firmware that is already optimized for real-time sensor integration.[2],[7] This satisfies:
- SDS-F-01 (Depth Range): The 0.5–100 m mapped depth range is within the Ping1D’s capability.[1],[2]
- SDS-F-02 (Resolution/Accuracy): The time-of-flight resolution supports ≤ 1 cm depth resolution, and typical system-level error (including mounting and beam spread) is expected to remain within the 10 cm bound when the sensor is correctly installed and calibrated.[1]
- SDS-F-03 (Update Rate): The Ping1D’s supported ping frequency and Pixhawk’s serial handling allow ≥ 10 Hz depth updates, ensuring dense spatial sampling at vessel speeds around 0.5–1.0 m/s.[1],[2]

Because depth is integrated directly into the Navigation Subsystem, it is automatically time-aligned with GPS, attitude, and velocity, which improves map quality and reduces post-processing complexity.[2],[7]

##### Obstacle Detection (IR Sensors → Arduino)
The LM393 IR modules provide discrete HIGH/LOW outputs within a short detection range (approximately 2–30 cm) based on reflective IR sensing.[3] This matches the intended use case of detecting nearby docks, hull collisions, or shoreline approach rather than long-range obstacle avoidance. With three sensors (front, port, starboard), the system gains coarse directional awareness around the bow. The Arduino polls each digital input at ~10 Hz or higher, which is sufficient given the short detection distance and slow vessel speeds.[4] This satisfies:
- SDS-F-04 (Obstacle Range): The stated modules operate reliably within 2–30 cm; mounting geometry and sensitivity tuning (via onboard potentiometer) can be adjusted during testing to optimize detection.[3]

The IR readings add situational awareness that complements, but does not replace, navigation planning and depth information.

##### Data Logging and Output
The Arduino formats IR readings and timestamps into structured records (CSV or JSON) and logs them to a microSD card via SPI while simultaneously streaming the same data over USB serial to the Raspberry Pi.[4] This dual-path strategy:
- Meets SDS-F-05, logging at ≥ 5 Hz, which is sufficient for synchronizing with navigation data and recreating events in post-analysis.
- Meets SDS-F-06, by using standard CSV/JSON formats easily parsed by Python, MATLAB, or other analysis tools, along with MAVLink-format depth data from the Pixhawk.[2],[6],[7],[9],[10]

Depth data, though not logged by the Arduino, is logged by the Pixhawk and/or Raspberry Pi, so the combined dataset (depth + IR + GPS + attitude) can be reconstructed for mapping and performance evaluation.

Overall, the subsystem provides continuous, well-structured sensor data in real time and preserves it for offline analysis, directly supporting the mapping and safety objectives.

#### 2. Electrical and Power Analysis

##### Voltage and Logic Compatibility
All selected components operate within the defined voltage domains:
- Arduino Uno and LM393 modules operate at 5 V.[3],[4]
- Ping1D accepts 5 V power and uses a serial interface compatible with Pixhawk’s rangefinder configuration when properly wired and configured.[1],[2]
- The SD card module includes its own 3.3 V regulator/level shifting or is otherwise interfaced through appropriate level shifting, satisfying SDS-E-01 and SDS-E-03.[5]

The design centrally relies on the 5 V rail supplied by the Power Subsystem, rather than including ad-hoc regulators within the Sensors Subsystem. This helps maintain clear responsibility boundaries and simplifies analysis.

##### Current Budget and Stability
The Ping1D is the single largest consumer on the 5 V rail (up to roughly 900 mA peak).[1] The Arduino, IR modules, and SD module add modest current overhead.[3] The estimated total current (~1.2–1.5 A) is within the specified limit in SDS-E-02, assuming the Power Subsystem provides margin above that. Decoupling capacitors are placed near the Ping1D and SD module to mitigate transient current spikes and local supply noise, which is important both for sonar performance and SD card reliability.[1],[5]

The use of a shared ground plane and clear grounding rules reduces the risk of ground loops or noise coupling into sensor lines. Together, these factors make the subsystem electrically robust and aligned with low-voltage safety and reliability expectations.[8]

#### 3. Environmental and Mechanical Analysis

##### Operating Environment
The components chosen (Arduino Uno, Ping1D, IR modules, SD module) are designed for typical ambient operating temperature ranges that comfortably cover 0–40 °C, consistent with many embedded and hobbyist electronics used in moderate outdoor conditions.[1],[3],[4],[5],[6] This satisfies SDS-ENV-01 for expected Tennessee lake conditions (Cane Creek Lake and similar environments).

##### Water Exposure and Enclosure
Only the Ping1D transducer is designed to be submerged.[1] All electronics are enclosed in an IP54 (or better) rated housing with sealed cable pass-throughs. This directly satisfies SDS-ENV-02, preventing water ingress while allowing cables for the Ping1D, IR sensors, and power connections. Proper strain relief and routing reduce mechanical stress on cables, improving long-term reliability.

##### Mounting Stability and Sensor Orientation
The Ping1D is mounted vertically downward on a rigid bracket, minimizing tilt and vibration.[1] This is critical to meeting the accuracy expectation in SDS-ENV-03, as off-axis mounting or vibration can introduce measurement error and noise. IR sensors are placed at the bow and slightly outward-angled to maximize their usefulness for short-range detection, which increases the likelihood that obstacles in front of the vessel are detected in time.[3]

These mechanical choices are coordinated with the Hardware Subsystem to ensure that the geometry matches the assumptions made in the navigation and sensor processing logic.

#### 4. Standards, Ethics, and Socio-Economic Considerations

##### Standards Compliance
The electrical design adheres to low-voltage design principles and aligns with relevant portions of IEC 61140 for protection against electrical hazards, satisfying SDS-STD-01.[8] Signal encoding and data logging follow widely accepted standards (IEEE 754 for floating-point representation and UTF-8 text encoding), satisfying SDS-STD-02 and improving portability and interoperability.[9],[10]

By using Pixhawk’s native MAVLink message types for depth reporting, the design also aligns with established UAV/USV communication practices, making integration with ground-control software and analysis tools straightforward.[2],[7]

##### Ethical Considerations
The Ping1D sonar operates within vendor-specified acoustic power levels intended for use in underwater robotics and depth measurement scenarios.[1],[2] By mounting the transducer to achieve the needed range without excessive gain or unnecessary ping rate, the design avoids introducing harmful acoustic pollution to the lake environment, supporting SDS-ETH-01.

The dual-path data logging strategy and use of unaltered raw logs support SDS-ETH-02 (Data Transparency and Integrity). Raw sensor values are preserved and timestamped, which discourages selective reporting or manipulation and enables external verification or re-analysis of results.

##### Socio-Economic Constraints
The BOM demonstrates that the subsystem cost remains below the $400 limit defined in SDS-SOC-01, while still using high-quality and widely used components (Arduino, Pixhawk-compatible sonar, commodity IR sensors and SD modules).[1],[3],[4],[5],[6] The emphasis on off-the-shelf, open, or well-documented hardware (SDS-SOC-02) ensures that future student teams, researchers, or community groups can reproduce or extend the subsystem without proprietary lock-in or significant cost barriers.

#### 5. Subsystem Integration and Risk Analysis

##### Integration with Navigation and Communication
By routing Ping1D directly into the Pixhawk and IR data into the Raspberry Pi via the Arduino, the design cleanly separates concerns:
- Navigation and control algorithms get depth data in the form most useful to them (native rangefinder input via ArduPilot).[2],[7]
- The Raspberry Pi receives both navigation telemetry (via MAVLink) and IR status (via USB serial), enabling flexible data fusion and visualization.[6],[7]

This architecture satisfies SDS-SYS-01 by ensuring that depth and obstacle information are both available at the system level and can be combined with GPS for bathymetric mapping and situational awareness. Power is supplied exclusively from the Power Subsystem in a well-defined way (SDS-SYS-02), keeping subsystem responsibilities clear.

##### Potential Risks and Mitigation
- Risk: Power brown-out under high Ping1D load.  
  Mitigation: The current budget was sized with margin; decoupling capacitors handle transient spikes. During integration testing, supply voltage will be monitored under worst-case load to validate design assumptions.[1],[5]

- Risk: IR false positives/negatives under strong sunlight or reflective surfaces.  
  Mitigation: Sensor thresholds will be tuned experimentally, and IR sensor placement can be adjusted by the Hardware Subsystem to minimize direct sunlight and reflections. Additionally, IR readings are only one of several cues and do not directly command propulsion without higher-level logic.[3]

- Risk: SD card write failures or corruption.  
  Mitigation: The Arduino firmware can implement simple retry logic on write errors and flush data periodically to minimize the impact of unexpected power loss. In parallel, depth and navigation data are still logged by Pixhawk and/or the Raspberry Pi, providing redundancy.[2],[4],[5],[6]

- Risk: UART misconfiguration between Ping1D and Pixhawk.  
  Mitigation: UART baud rate and Ping1D-specific configuration will be tested in isolation first. ArduPilot’s known configuration parameters for rangefinders will be used, and bench testing can verify reliable communication before lake deployment.[2],[7]

Through these mitigations, the subsystem is designed not only to meet nominal specifications but also to degrade gracefully in the face of typical field issues.

#### Conclusion

The chosen architecture (Ping1D depth directly into the Pixhawk, IR sensors and SD logging on the Arduino, and system-level fusion on the Raspberry Pi) meets the functional desires of the project (bathymetric mapping and obstacle awareness) while satisfying the defined technical, environmental, standards-based, ethical, and socio-economic constraints.[1]–[10] Electrical budgets, environmental limits, and inter-subsystem interfaces have been considered in sufficient detail to justify that the subsystem is buildable, integrable, and capable of performing reliably in the target operational environment.

Assuming proper implementation of the described schematics, firmware, and mechanical mounting, the Sensors and Data Acquisition Subsystem should accomplish its intended function and support the overall autonomous vessel in safely and effectively achieving its mission objectives.

## References

[1]  Blue Robotics, “Ping Sonar Altimeter and Echosounder (Ping1D),” product page, accessed Nov. 2025. Available: https://bluerobotics.com/store/sonars/echosounders/ping-sonar-r2-rp/

[2]  Blue Robotics, “Ping Sonar Technical Manual,” technical guide, Sep. 2019. Available: https://bluerobotics.com/learn/ping-sonar-technical-guide/

[3]  ArduPilot Development Team, “Blue Robotics Ping Underwater Sonar,” ArduPilot Copter Documentation, accessed Nov. 2025. Available: https://ardupilot.org/copter/docs/common-bluerobotics-ping.html

[4]  Arduino, “UNO R3,” Arduino Hardware Documentation, accessed Nov. 2025. Available: https://docs.arduino.cc/hardware/uno-rev3

[5]  Arduino, “Arduino Uno Rev3,” product page, accessed Nov. 2025. Available: https://store.arduino.cc/products/arduino-uno-rev3

[6]  Microchip Technology Inc., “ATmega48A/PA/88A/PA/168A/PA/328/P 8-bit AVR Microcontrollers,” Datasheet, 2016. Available: https://ww1.microchip.com/downloads/en/DeviceDoc/ATmega48A-PA-88A-PA-168A-PA-328-P-DS-DS40002061A.pdf

[7]  ON Semiconductor, “LM393 Low Offset Voltage Dual Comparators,” Datasheet, Rev. 34, Sep. 2025. Available: https://www.onsemi.com/download/data-sheet/pdf/lm393-d.pdf

[8]  Chip Dip, “Infrared Proximity Sensor, Obstacle-Avoiding Module (LM393-based),” Module Datasheet, accessed Nov. 2025. Available: https://static.chipdip.ru/lib/663/DOC001663798.pdf

[9]  HiLetgo, “Micro SD TF Card Adapter Reader Module, 6-pin SPI Interface, with Level Conversion for Arduino,” product listing, accessed Nov. 2025. Available: https://www.amazon.com/dp/B07BJ2P6X6

[10]  Generic Vendor, “Micro SD Card Module Storage Board, 6-pin TF Card Adapter,” product listing, accessed Nov. 2025. Available: https://www.amazon.com/dp/B07PFDFPPC

[11]  Raspberry Pi Ltd., “Raspberry Pi Computer Hardware Documentation,” official documentation portal, accessed Nov. 2025. Available: https://www.raspberrypi.com/documentation/computers/raspberry-pi.html

[12]  PX4 Development Team, “Pixhawk Series Flight Controllers,” PX4 User Guide, accessed Nov. 2025. Available: https://docs.px4.io/main/en/flight_controller/pixhawk_series

[13]  ArduPilot Development Team, “Pixhawk Overview,” ArduPilot Copter Documentation, accessed Nov. 2025. Available: https://ardupilot.org/copter/docs/common-pixhawk-overview.html

[14]  “Pixhawk 2.4.8 (Pixhawk 1),” RflySim Documentation, accessed Nov. 2025. Available: https://rflysim.com/doc/en/B/2.1Pixhawk1.html

[15]  Blue Robotics, “Ping Viewer Documentation,” accessed Nov. 2025. Available: https://docs.bluerobotics.com/ping-viewer/

[16]  International Electrotechnical Commission, “IEC 61140:2016 – Protection against Electric Shock – Common Aspects for Installation and Equipment,” 4th ed., 2016.

[17]  IEEE, “IEEE Standard 754-2019 for Floating-Point Arithmetic,” IEEE Std 754-2019, Jul. 2019.

[18]  F. Yergeau, “UTF-8, a transformation format of ISO 10646,” RFC 3629, Nov. 2003.

[19]  Murata Manufacturing Co., Ltd., “GRM188R71C104KA01D – Multilayer Ceramic Capacitor, 0.1 μF, 16 V,” Datasheet, accessed Nov. 2025. Available: https://www.murata.com/en-us/products/productdetail?partno=GRM188R71C104KA01D

[20]  Panasonic Industry, “EEU-FR Series Aluminum Electrolytic Capacitors,” Series Datasheet, accessed Nov. 2025. Available: https://na.industrial.panasonic.com/products/capacitors/aluminum-electrolytic-capacitors/lineup/aluminum-electrolytic-capacitors-radial-lead-type/series/83367
