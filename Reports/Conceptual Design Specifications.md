# Conceptual Design

This document outlines the objectives of a conceptual design. After reading your conceptual design, the reader should understand:

- The fully formulated problem.
- The fully decomposed conceptual solution.
- Specifications for each of the atomic pieces of the solution.
- Any additional constraints and their origins.
- How the team will accomplish their goals given the available resources.

With these guidelines, each team is expected to create a suitable document to achieve the intended objectives and effectively inform their stakeholders.


## General Requirements for the Document
- Submissions must be composed in Markdown format. Submitting PDFs or Word documents is not permitted.
- All information that is not considered common knowledge among the audience must be properly cited.
- The document should be written in third person.
- An introduction section should be included.
- The latest fully formulated problem must be clearly articulated using explicit "shall" statements.
- A comparative analysis of potential solutions must be performed
- The document must present a comprehensive, well-specified high-level solution.
- The solution must contain a hardware block diagram.
- The solution must contain an operational flowchart.
- For every atomic subsystem, a detailed functional description, inputs, outputs, and specifications must be provided.
- The document should include an acknowledgment of ethical, professional, and standards considerations, explaining the specific constraints imposed.
- The solution must include a refined estimate of the resources needed, including: costs, allocation of responsibilities for each subsystem, and a Gantt chart.


## Introduction
This problem affects environmental and local governments who need accurate lakes and reservoir data. This data can then be used for environmental monitoring, flood management, and infrastructure planning. It also impacts fisheries, recreational, and conservation groups. The current lake mapping methods tend to be expensive and require well-trained operators. So, a low cost, autonomous solution would allow for more frequent and efficient data collection without the high cost and specialized skills. This would allow lake mapping to become more expandable and accessible. While commercial sonar and autonomous boats do exist, simply the cost and specific skill set needed to use them limit the availability greatly. Consumer sonar systems may provide depth readings but lack autonomy, data mapping, and integration capabilities. So, a custom-built solution can guarantee affordability, versatility in differing environments, and control over the capacity of the solution’s growth.


## Restating the Fully Formulated Problem

The fully formulated problem is the overall objective and scope complete with the set of shall statements. This was part of the project proposal. However, it may be that the scope has changed. So, state the fully formulated problem in the introduction of the conceptual design and planning document. For each of the constraints, explain the origin of the constraint (customer specification, standards, ethical concern, broader implication concern, etc).


##**Comparative Analysis of Potential Solutions**
The project requires reliable sensing for both **lake bottom depth mapping** and **water current flow measurement**. Selecting the proper technologies for sonar-based depth sensing and flow monitoring is essential to balance affordability, accuracy, integration complexity, and long-term adaptability.


### **Depth Measurement Solutions (Sonar and Transducers)**

- **1. Consumer Fish Finder/Depth Finder Transducers**
 These systems are widely available, often costing under $500, and provide basic point depth readings. While inexpensive and easy to operate, they lack research-quality accuracy, real-time mapping integration, and compatibility with GIS platforms.
- **2. Single-Beam Echo Sounder with Transducer (e.g., Blue Robotics Ping2)**
 Costing approximately $400–$600, these sonar units deliver ±5–15 cm accuracy and support open-source integration with Raspberry Pi and autopilot modules. They are well-suited for proof-of-concept mapping, offering the best balance of affordability, accuracy, and adaptability.
- **3. Survey-Grade Single-Beam Echo Sounders (e.g., CEESCOPE with RTK GNSS)**
 These professional systems achieve ±2–5 cm accuracy and integrate seamlessly into commercial hydrographic workflows. However, their cost exceeds $10,000, making them impractical for a budget-constrained student project.
- **4. Multibeam Sonar Systems**
 Industry-standard for hydrography, multibeam systems provide swath coverage with ±1–2 cm precision. Yet, costs ($50,000–$100,000+) and required RTK-INS navigation systems render them infeasible for this project.


### **Flow Measurement Solutions (Including YF-S201 Sensor)**

- **1. YF-S201 Hall-Effect Flow Sensor**
 The YF-S201 is a low-cost (~$10–$20) sensor that uses a turbine and Hall-effect sensor to measure flow rate. It outputs digital pulses proportional to the water velocity. While not as precise as acoustic methods, it is extremely affordable, lightweight, and easy to integrate with microcontrollers such as Arduino or Raspberry Pi. Its accuracy is typically within ±10% after calibration, making it suitable for small-scale, student-built systems.
- **2. Acoustic Doppler Current Profilers (ADCPs)**
 ADCPs represent the professional standard for water flow measurement, providing full velocity profiles and discharge calculations. They offer high accuracy and reliability but are expensive ($15,000–$30,000+) and require significant power and integration effort.
- **3. Mechanical and Acoustic Flow Meters (e.g., OTT ****Hydromet****)**
 Commercial mechanical and acoustic meters provide accurate point-based velocity measurements. However, they are costly ($5,000–$15,000) and less adaptable for integration into an autonomous platform.

### **Non-Starters**

- **Multibeam sonar** and **survey-grade single-beam** systems are eliminated due to prohibitive costs.
- **ADCPs and commercial flow meters** provide excellent accuracy but exceed budgetary and integration constraints.
- **Consumer fish finders** lack the precision and data interoperability required for research applications.


### **Benefits vs. Costs**

The **Blue Robotics Ping2 single-beam sonar** offers the best balance for depth mapping: affordable, accurate within ±5–15 cm, and open-source friendly.
 For flow measurement, the **YF-S201 sensor** provides a budget-conscious alternative. Although less precise than ADCPs, it offers sufficient accuracy (±10%) for a capstone project, particularly when validated against reference datasets. Its ease of integration and extremely low cost make it highly attractive.


### **Most Likely to Succeed**

The chosen configuration combines a **single-beam sonar transducer (Ping2 or equivalent)** for depth measurement with a **YF-S201 Hall-effect flow sensor** for current velocity monitoring. Together, these solutions maximize affordability, accuracy, and ease of integration. The sonar system achieves the project’s bathymetric goals within a reasonable budget, while the YF-S201 enables basic flow monitoring without requiring costly acoustic Doppler systems.
This hybrid approach balances technical feasibility with project constraints and provides a scalable platform that can be expanded with more advanced sensors in the future.

## **High-Level Solution**
The autonomous lake-mapping vessel integrates multiple coordinated subsystems to efficiently meet all stakeholder goals and project requirements while remaining within design constraints. The system combines a durable fiberglass-reinforced 3D-printed hull, efficient power management, and reliable communication to deliver accurate bathymetric mapping and autonomous navigation. High-precision sonar and water velocity sensors collect depth and location data, which are processed and transmitted via a wireless link to an on-shore computer for real-time monitoring. The modular battery configuration and optional solar backup ensure power reliability, while onboard SD storage prevents data loss in the event of communication failure. Lightweight materials, optimized control algorithms, and energy-efficient components reduce power consumption and maximize operational endurance. The design emphasizes safety, maintainability, and cost-effectiveness, achieving a balance between performance and resource optimization to provide a robust, autonomous, and user-friendly data collection platform.


## Hardware Block Diagram


## Operational Flow Chart


## Atomic Subsystem Specifications
### Subsystem 1: (Hardware)

#### Function:

The Hardware Subsystem serves as the structural and mechanical foundation of the autonomous vessel. It supports all other subsystems—power, propulsion, communication, and sensors—by providing buoyancy, stability, and mounting surfaces for equipment. The vessel features a catamaran-style hull to enhance stability and hydrodynamic performance. It is designed to accommodate a payload of approximately 40 to 50 pounds, including all electronic components, sensors, and power systems. The hull will be 3D printed in modular sections for ease of fabrication and assembly and then reinforced with a fiberglass coating to ensure durability, water resistance, and impact protection during operation.
The subsystem also includes the primary onboard instruments, such as the Lowrance Elite-5 DSI depth finder and the main battery system, which together enable continuous operation and real-time data collection.

#### Interfaces:

**1. Structural Interface**
- **Connection Type:** Mechanical assembly
- **Signal Type:** N/A
- **Direction:** N/A
- **Protocol:** N/A
- **Data Exchanged:** N/A
- **Description:** The 3D printed hull sections shall be mechanically fastened and sealed to form the catamaran-style body. Mounting brackets will interface with the propulsion, sensor, and control modules.

**2. Depth Finder (Lowrance Elite-5 DSI)**
- **Connection Type:** Wired connection to transducer and communication system
- **Signal Type:** Digital data
- **Direction:** Input/Output
- **Protocol:** NMEA 2000
- **Data Exchanged:** Sonar and GPS data transmitted to the onboard processing unit; system commands received as applicable

**3. Battery System**
- **Connection Type:** Wired
- **Signal Type:** DC power
- **Direction:** Output (to power subsystems)
- **Protocol:** N/A
- **Data Exchanged:** 12V or 24V regulated power distributed to all electronic components

**4. Power Distribution Module**
- **Connection Type:** Wired
- **Signal Type:** DC power
- **Direction:** Output to onboard systems
- **Protocol:** N/A
- **Data Exchanged:** Power flow to propulsion, sensors, and communication subsystems

**5. Waterproof Enclosures and Wiring Interfaces**
- **Connection Type:** Physical enclosure and wiring glands
- **Signal Type:** Mixed (Power and Data)
- **Direction:** Bidirectional
- **Protocol:** Dependent on connected system (NMEA, SPI, UART)
- **Data Exchanged:** Sensor and control signals routed between modules while maintaining environmental sealing

#### Operation:

The Hardware Subsystem operates as the physical base for all vessel systems.
- The 3D printed catamaran hull provides stability and balance for smooth operation in variable water conditions.
- Internal compartments house and protect sensitive electronics, including the power system, control boards, and communication modules.
- The Lowrance Elite-5 DSI depth finder or other sonar devices operate      continuously, transmitting sonar pings through its transducer to measure depth and detect underwater features.
- Collected depth data are transferred through the communication and data processing subsystem to the base station for live analysis.
- The vessel’s battery system provides consistent power to all onboard electronics for a minimum of two to four hours of continuous operation.
- The fiberglass coating ensures water resistance and enhances durability against environmental stress and minor impacts.
This subsystem ensures the vessel maintains structural integrity, buoyancy, and reliability under all expected environmental conditions while supporting continuous data collection and transmission.

#### Shall Statements:

- The vessel **shall** feature a catamaran-style hull to maximize stability and hydrodynamic efficiency.
- The vessel **shall** support a total payload capacity of 40–50 pounds, including all electronics, sensors, and batteries.
- The hull **shall** be 3D printed in modular sections for ease of fabrication and assembled prior to fiberglass reinforcement.
- The outer shell **shall** be coated in fiberglass to ensure water resistance and structural durability.
- The subsystem **shall** integrate mounting locations for all sensors, propulsion components, and electronics.
- The depth finder **shall** be a Lowrance Elite-5 DSI and **shall** interface directly with its transducer for sonar and GPS data collection.
- The depth finder **shall** transmit collected data to the communication and data processing subsystem for remote monitoring.
- The battery system **shall** supply stable DC power for a minimum of two to four hours of full operation.
- The batteries **shall** be marine rated to withstand moisture and possible water exposure.
- The hardware subsystem **shall** maintain waterproofing and mechanical stability in all expected operating conditions.


### Subsystem 2: (Power)

#### Function:

The Power Subsystem supplies, regulates, and distributes electrical power to all onboard components, including the propulsion motors, sensors, communication module, and control electronics. It ensures stable operation across all environmental and load conditions while incorporating safety mechanisms to prevent overcurrent, overvoltage, and thermal damage. The subsystem also manages power input from external charging sources such as AC adapters or solar panels when applicable.

#### Power Supply Components:

Main Battery System
- **Type:** 12V Deep Cycle Marine Battery or Lithium Ion Battery encased in waterproof container
- **Function:** Provides the primary power source for all onboard systems, offering high energy density and resistance to vibration and moisture.
- **Capacity:** Sized to support a minimum of 2–4 hours of continuous operation under full load.

Voltage Regulation Module
- **Function:** Steps down or regulates power to required voltage levels for various subsystems (e.g., 12V → 5V for microcontrollers and sensors).
- **Components:** Includes DC-DC buck converters and voltage regulators with thermal protection and efficiency >85%.
Power Distribution Board (PDB)
- **Function:** Distributes regulated power from the main battery to all subsystems via fused lines.
- **Features:** Integrated current sensing for monitoring, reverse polarity protection, and master power switch for manual cutoff.
Solar Backup Integration (Optional)
- **Function:** Allows trickle-charging of the main battery via a solar panel to extend operational endurance during long-duration missions.
- **Operation:** The solar charge controller prevents overcharging and backflow to ensure system safety.
- It can also serve as an emergency power source if battery gives out.

Interfaces:

**Connection Type:**
- Direct wired power connections using marine-grade insulated cables
**Signal Type:**
- DC electrical power distribution
**Direction:**
- Output from Power Subsystem to all other subsystems
**Protocol:**
- None (power lines only)
**Data Sent:**
- Voltage and current monitoring feedback (optional analog output from sensors to microcontroller)
**Data Received:**
- Power-on command signals (digital) from control subsystem
- Charging input from solar or AC source

Operation:

During startup, the power distribution board energizes the propulsion system, sensors, and control electronics sequentially to prevent current surges. The voltage regulator ensures that all sensitive devices receive stable DC levels. Battery levels are monitored continuously through analog feedback to the microcontroller, which can alert the operator via the communication subsystem if voltage drops below the critical threshold. When docked, the system automatically switches to charging mode if an external power input is detected.

Shall Statements:
- The subsystem shall provide stable DC power to all connected subsystems within ±5% of nominal voltage.
- The subsystem shall maintain at least 2 hours of full operational power under typical mission load.
- The subsystem shall include voltage and current protection for all output lines.
- The subsystem shall allow for external recharging via AC or solar input.
- The subsystem shall monitor and report battery voltage to the communication subsystem every 10 seconds.
- The subsystem shall feature a main power switch to manually disconnect the entire vessel’s power supply.
- The subsystem shall isolate high-current propulsion power from sensitive electronics to prevent interference.
- The subsystem shall provide reverse polarity and short-circuit protection for all outputs.

### Subsystem 3: (Navigation)

#### GPS Positioning System

The GPS Positioning Subsystem provides absolute position, time, and velocity data to the navigation and autopilot subsystems. It determines the vessel’s precise location on the water and supports real-time tracking, route following, and mission mapping. The subsystem utilizes a high-sensitivity GNSS receiver with an integrated antenna and supports standard NMEA and vendor-specific protocols for data output.
For advanced accuracy, the subsystem optionally accepts RTK (Real-Time Kinematic) correction data to achieve sub-meter positional precision. The GPS continuously transmits satellite health, fix status, and position information to the onboard navigation computer and autopilot controller for real-time guidance and logging.

Interfaces:

- Power Input
  - Connection Type: Wired
  - Signal Type: DC Power (3.3–5 V nominal)
  - Direction: Input to GPS module
  - Data Exchanged: Power supply from the main power distribution system
- Primary Communication (UART/USB)
  - Connection Type: Serial (TTL UART or USB virtual COM)
  - Signal Type: Digital data
  - Direction: Bidirectional (GPS → Host / Host → GPS)
  - Protocol: NMEA 0183 or UBX binary
  - Data Exchanged:
    - Output: Position, time, velocity, fix quality, satellite count
    - Input: Configuration commands (e.g., update rate, protocol select)
- Optional RTK Correction Input
  - Connection Type: Serial or UDP
  - Signal Type: Digital
  - Direction: Input to GPS
  - Protocol: RTCM3
  - Data Exchanged: Differential correction data for high-precision positioning
- Antenna Interface
  - Connection Type: SMA/MCX coaxial
  - Signal Type: RF input
  - Direction: Input to GPS receiver
  - Protocol: N/A
  - Data Exchanged: GNSS satellite RF signals
- Physical/Mechanical
  - Connection Type: Shielded cable with keyed connectors
  - Signal Type: Power, RF, and data
  - Direction: Bidirectional as applicable
  - Protocol: N/A
  - Data Exchanged: Environmental protection and mechanical connection integrity

Operation:

The GPS Positioning Subsystem operates as the primary localization source for the vessel. The GNSS receiver continuously calculates latitude, longitude, altitude, and UTC time using signals from multiple satellite constellations. The module outputs NMEA sentences (e.g., GGA, RMC, VTG) or binary UBX messages at configurable update rates between 1–10 Hz.
The navigation subsystem and autopilot receive these data through the UART/USB interface to maintain accurate localization and velocity estimation. When RTK corrections are available, the module applies differential data via the RTCM3 stream to improve accuracy to sub-meter levels.
Each position message includes fixed quality, satellite count, and error dilution metrics (HDOP) to ensure reliability. If signal loss occurs, the module flags “no fix” and maintains the last known position with a timestamp.
The GPS subsystem is powered by the main DC distribution line (5 V nominal) and is mechanically mounted with a waterproof antenna to ensure optimal satellite visibility and durability in marine conditions.


Shall Statements:

- The subsystem shall output latitude, longitude, altitude, and UTC time at a configurable rate between 1–10 Hz.
- The subsystem shall provide a fixed-quality indicator and number of satellites with every position update.
- The subsystem shall communicate over TTL UART (3.3 V logic) or USB virtual COM for host connection.
- The subsystem shall accept configuration commands over its RX serial line to adjust output parameters.
- The subsystem shall support optional RTK correction input using the RTCM3 protocol for sub-meter accuracy.
- The subsystem shall operate within 3.3–5 V DC power input and include reverse-polarity protection.
- The subsystem shall report stale data if no new fix is available within twice the update interval.
- The subsystem shall integrate a waterproof external antenna with proper strain relief and isolation.

#### Autopilot Subsystem

Function:

The Autopilot Subsystem fuses navigation data, onboard sensor measurements, and operator commands to autonomously guide the vessel along pre-defined routes. It maintains stable heading, speed, and position control through closed-loop feedback using an integrated IMU, magnetometer, barometer, and GPS input.
This subsystem executes mission management, guidance, and control algorithms, ensuring the vessel follows planned waypoints and coverage paths while responding to environmental and system conditions. The autopilot communicates with the navigation and communication subsystems to exchange telemetry, mission updates, and safety alerts.


### Interfaces:

- Power Input
  - Connection Type: Wired
  - Signal Type: DC Power (5–12 V regulated)
  - Direction: Input to autopilot
  - Protocol: N/A
  - Data Exchanged: Power supplied from the power distribution module
- Sensor Inputs
  - GPS Input: UART from GPS subsystem (NMEA or UBX data)
    - Direction: GPS → Autopilot
  - IMU, Magnetometer, Barometer: Internal or I²C connections to autopilot controller
    - Direction: Sensors → Autopilot
  - Sonar / LiDAR: Serial or I²C interface for obstacle/altitude data
    - Direction: Sensor → Autopilot
- Actuator Outputs
  - Connection Type: PWM, DShot, or CAN ESC
  - Signal Type: Digital control signals
  - Direction: Autopilot → Thrusters and Rudder
  - Protocol: PWM (50–400 Hz) or digital ESC protocols
- Telemetry / Ground Station Communication
  - Connection Type: Wireless (2.4 GHz / 900 MHz) or USB/Ethernet
  - Signal Type: Digital
  - Direction: Bidirectional
  - Protocol: MAVLink
  - Data Exchanged: Telemetry, mission updates, status, and control commands
- Companion Computer Interface
  - Connection Type: Serial or UDP (Ethernet/Wi-Fi)
  - Signal Type: Digital
  - Direction: Bidirectional (Autopilot ↔ Raspberry Pi)
  - Protocol: MAVLink
  - Data Exchanged: Waypoints, state estimates, mission commands, and feedback
- Safety Inputs
  - Connection Type: Wired (kill switch, RC input)
  - Signal Type: Digital logic
  - Direction: External → Autopilot
  - Protocol: GPIO / PWM
  - Data Exchanged: Manual override and emergency stop signals


Operation:

The Autopilot Subsystem governs the vessel’s autonomous control loops and mission execution. It receives GPS and sensor data to estimate the vessel’s attitude, velocity, and position using an Extended Kalman Filter (EKF).
Based on mission waypoints and path plans, the autopilot generates control commands for the propulsion and steering subsystems via PWM or CAN ESC interfaces. The system runs inner-loop attitude control at ≥50 Hz for stability and outer-loop navigation at 5–20 Hz for waypoint tracking.
Telemetry data—including battery level, GPS fix, EKF health, and mission progress—are sent to the ground station through the communication subsystem. In case of telemetry or GPS loss, the autopilot triggers failsafe behaviors such as loiter, stop, or return-to-home.
The subsystem integrates with a companion computer (Raspberry Pi 4) for high-level mission management and logging. Manual override capability is always available through the RC input or kill switch for safety.


Shall Statements:

- The subsystem shall receive GPS data from the GPS subsystem via UART and incorporate it into its navigation estimator.
- The subsystem shall fuse IMU, magnetometer, barometer, and GPS data using an EKF to estimate attitude, velocity, and position at ≥5 Hz.
- The subsystem shall generate actuator outputs (PWM/DShot) at rates of 50–400 Hz to control propulsion and steering.
- The subsystem shall communicate using MAVLink for telemetry, mission updates, and operator commands.
- The subsystem shall implement mission management functions, including waypoint following, loiter, and return-to-home.
- The subsystem shall provide failsafe responses for GPS loss, telemetry loss, and low battery conditions.
- The subsystem shall allow manual override via RC input and immediately cease autonomous commands when activated.
- The subsystem shall log all navigation and control data locally with timestamps for post-mission review.
- The subsystem shall receive 5–12 V DC input and include integrated voltage/current sensing for power monitoring.
- The subsystem shall maintain end-to-end control latency under 100 ms during normal operation.


### Subsystem 4: (Communication)

The Communication and Data Processing Subsystem provides the primary link between the vessel’s onboard systems and the base station computer located on land. It manages both the transmission of collected sensor data—such as GPS coordinates, depth readings, and system status—and the reception of operator commands. The subsystem uses a wireless communication link, established through a Wi-Fi or RF telemetry module, to ensure reliable, real-time data transfer.
The onboard controller, a Raspberry Pi 4, serves as the central processing unit for managing communication and local data handling. In the event of a loss of connection, data are automatically stored on an onboard SD card to ensure no information is lost. Additionally, the SonTek Power and Communication Module (PCM) may be used to interface the sonar system with the main communication link, ensuring synchronized and noise-free data exchange for accurate bathymetric mapping and velocity measurements.

**Interfaces:**
**1. Base Station Computer**
- **Connection Type:** Wireless (Wi-Fi / 2.4 GHz RF)
- **Signal Type:** Digital data
- **Direction:** Bidirectional
- **Protocol:** TCP/IP or Serial over RF
- **Data Exchanged:** GPS, depth, and system status data sent; control commands received
**2. ****SonTek**** Power and Communication Module (PCM)**
- **Connection Type:** Wired
- **Signal Type:** Digital serial
- **Direction:** Bidirectional
- **Protocol:** USB / RS-232
- **Data Exchanged:** Sonar depth and velocity data exchanged
**3. Arduino System (Sensor and Data Acquisition Subsystem)**
- **Connection Type:** Wired
- **Signal Type:** Digital serial
- **Direction:** Bidirectional
- **Protocol:** UART @ 115200 bps
- **Data Exchanged:** Aggregated sensor data sent; acknowledgment and timing signals received
**4. SD Card Module (Onboard Storage)**
- **Connection Type:** Wired
- **Signal Type:** Digital SPI
- **Direction:** Bidirectional
- **Protocol:** SPI @ 8 MHz
- **Data Exchanged:** Data logged and retrieved as backup
**5. Power Subsystem (Battery Management System)**
- **Connection Type:** Wired
- **Signal Type:** DC power
- **Direction:** Input
- **Protocol:** N/A
- **Data Exchanged:** 5V regulated power input

**Operation:**
The Communication and Data Processing Subsystem serves as the vessel’s central communication hub, ensuring all mission data and commands are exchanged accurately and efficiently between onboard systems and the operator.
- Sensor and navigation data from the Arduino-based Data Acquisition Subsystem are transmitted to the Raspberry Pi 4.
- The Raspberry Pi formats the incoming data into structured packets containing GPS position, water depth, and vessel system status.
- These packets are transmitted over Wi-Fi or RF telemetry to the base station computer, allowing the operator to monitor the vessel’s progress in real time.
- Incoming commands from the base station (e.g., mission start, stop, or return) are received and relayed to the appropriate onboard subsystem.
- The SonTek Power and Communication Module (PCM) synchronizes sonar and velocity data, ensuring consistent and noise-free transmission.
- In case of network interruption, the Raspberry Pi stores all data locally on an SD card, resuming transmission once the connection is reestablished.
- The subsystem continuously monitors communication health and provides feedback on link strength and data integrity to the operator interface.
This operation ensures reliable, low-latency communication between the vessel and the control station throughout all mapping missions.

**Shall Statements:**
- The subsystem **shall** provide a continuous bidirectional communication link between the vessel and the base station computer.
- The subsystem **shall** transmit GPS, depth, and system status data packets at a rate of at least one hertz (1 Hz).
- The subsystem **shall** receive and process operator control commands, including mission start, stop, and return instructions.
- The subsystem **shall** store all transmitted and received data to an onboard SD card to prevent loss during communication failures.
- The subsystem **shall** utilize Wi-Fi (TCP/IP) or 2.4 GHz RF telemetry as the primary communication link.
- The subsystem **shall** integrate with the SonTek Power and Communication Module (PCM) for synchronized sonar data exchange.
- The subsystem **shall** maintain a maximum end-to-end communication latency of less than two seconds.
- The subsystem **shall** automatically resume transmission once connectivity to the base station is restored.
- The subsystem **shall** receive 5V regulated DC power input from the Power Subsystem.
- The subsystem **shall** operate continuously during all mapping missions to support real-time monitoring and control.


### Subsystem 5: (Sensors and Data Acquisition)
The Sensors and Data Acquisition Subsystem is responsible for receiving and recording data from multiple onboard instruments to generate synchronized measurements of depth, flow rate, and geographical position. The subsystem processes input signals from the transducer, GPS receiver, and water flow sensor, then stores the resulting data on an SD card for later processing and analysis. All sensors are integrated through an Arduino Uno microcontroller, which serves as the core processing and control unit.

- Transducer – Lowrance HDS-5 Fish Finding System

Function:

The transducer provides underwater depth measurements and positional data via its built-in GPS receiver. It operates as the primary instrument for bathymetric data collection, transmitting sonar pings to determine water depth relative to the transducer's face.

Interfaces:

- Connection Type: NMEA 2000 network (differential CAN bus)
- Signal Type: Digital data communication
- Direction: Input to Arduino system
- Protocol: NMEA 2000, 250 kbps
- Data Sent:
  - Depth measurement (PGN 128267)
  - GPS position and time (PGN 129029)
- Data Received: None (read-only connection)

Operation:

The Arduino Uno reads incoming NMEA 2000 messages through a CAN controller (MCP2515) and transceiver (TJA1050). The system extracts water depth, latitude, longitude, and time data, synchronizing them with other sensor inputs for unified data logging.

Shall Statements:
- The subsystem shall receive depth and GPS data from the Lowrance HDS-5 via the NMEA 2000 network.
- The subsystem shall extract and record latitude, longitude, and time from the GPS feed.
- The subsystem shall operate in read-only mode to avoid interference with the NMEA 2000 backbone.
- The subsystem shall store all received data to the SD card in a timestamped format.

- Autonomous Water Flow Control and Monitoring System

Function:

This subsystem measures the water’s flow rate and total volume using a YF-S201 Hall-effect flow sensor, while also utilizing an infrared sensor (LM393) for proximity detection. A relay module is available for controlling connected actuators or valves as needed for future automation features. The original design based on an ESP8266 microcontroller has been adapted to operate on the Arduino Uno platform.

Interfaces:
- Connection Type: Direct wired to Arduino
- Signal Types:
  - Digital pulse signal (YF-S201)
  - Digital logic (LM393)
  - Digital output control (Relay)
  - Direction: Inputs from sensors to Arduino; output from Arduino to relay.
  - Protocol: GPIO (digital pulse counting and logic control).
- Data Sent:
  - Flow rate pulses (proportional to water velocity)
  - Proximity detection flag (IR sensor)
- Data Received:
  - Control signal to toggle relay module (if applicable).

Operation:

The Arduino counts pulses from the YF-S201 sensor using an interrupt routine to compute real-time flow rate and cumulative volume. The LM393 IR sensor provides an obstacle or fluid presence signal, while the relay module can be activated for flow control or safety response.

Shall Statements:
- The subsystem shall measure flow rate using the YF-S201 Hall-effect sensor.
- The subsystem shall compute and log both instantaneous flow rate (L/min) and total volume (L).
- The subsystem shall detect nearby objects or changes using the LM393 IR sensor.
- The subsystem shall provide control signals to a relay module for optional actuation.
- The subsystem shall transmit all processed data to the Arduino data logger at a frequency of one record per second.

- Obstacle Avoidance Subsystem

Function:

The obstacle avoidance subsystem ensures safe navigation by detecting nearby physical obstacles above or below the waterline. It utilizes a combination of infrared (IR) proximity sensors and a sonar transducer to provide short- and mid-range detection.

Interfaces:
- Connection Type: Wired to Arduino digital I/O pins
- Signal Type:
  - Digital (IR sensor trigger)
  - Analog or timing pulse (ultrasonic transducer echo)
- Direction: Input to Arduino
- Protocol: Trigger/echo timing and TTL logic signals
- Data Sent: Distance measurement (cm), obstacle detection flag

Operation:

When the sonar sensor emits a pulse, the reflected echo is timed by the Arduino to calculate the distance to the nearest obstacle. Simultaneously, the IR sensor detects close-range obstacles and sends a binary high/low signal. The system flags potential hazards in real time and includes obstacle data in the logged dataset.

Shall Statements:
- The subsystem shall measure obstacle distance using ultrasonic sonar.
- The subsystem shall detect close-range objects with the LM393 IR sensor.
- The subsystem shall flag obstacles within one meter as potential hazards.
- The subsystem shall provide an obstacle flag output to the Arduino logger.
- The subsystem shall operate continuously during all mapping missions.

- Data Logging and Control Integration

Function:

All sensor subsystems interface through the Arduino Uno, which acts as the central processor and data logger. The Arduino aggregates data from the transducer, flow, and obstacle sensors, synchronizes it with GPS timestamps, and writes it to an SD card in a structured CSV format.

Interfaces:
- Connection Type: SPI for SD card, UART for communication
- Signal Type: Digital serial
- Direction: Bi-directional (read/write)
- Protocol: SPI @ 8 MHz, CSV file structure for logging
- Data Sent: Combined dataset of all measurements
- Data Received: System commands (start/stop, calibration)

Operation:

During operation, the Arduino performs a one-second logging cycle that gathers all active sensor readings. Each record contains timestamped data for depth, position, flow rate, and obstacle detection. The system also handles error detection and performs safe data flushing on power loss.

Shall Statements:
- The subsystem shall record synchronized sensor data at least once per second.
- The subsystem shall store data in CSV format with fields for time, GPS coordinates, depth, flow rate, and obstacle status.
- The subsystem shall perform SD card write verification and flush data upon shutdown.
- The subsystem shall communicate summary data to the onboard communication module.

- System Integration Summary
All sensors and modules are interconnected through the Arduino Uno, forming a unified sensing and acquisition platform. The system collectively enables real-time environmental measurement, data synchronization, and onboard storage for subsequent processing and visualization.


** Ethical, Professional, and Standards Considerations**

## **Specifications and Constraints**

- The vessel shall be user-friendly and straightforward to operate in both autonomous and RC (manual) modes.
- The vessel shall include a fail-safe system in both autonomous and RC modes to prevent loss of the boat in case of system error or communication failure.
- The vessel shall carry a payload capacity of 40–50 lbs, including sensors, batteries, and communication equipment.
- The vessel shall support depth sensors and water flow measurement sensors for data collection.
- The vessel shall operate for a minimum of 2–3 hours per battery cycle, with provisions for multiple batteries for both boat propulsion and sensor operation.
- The vessel shall utilize a catamaran-style hull for stability and weight distribution.
- The system shall interface with commercial software such as Hypack for navigation and data logging.
- The system shall support open-source software such as QGIS for GPS point data and ArcGIS Pro for data post-processing.
- The vessel shall comply with relevant Tennessee boating regulations (TWRA) and federal guidelines regarding RC and autonomous surface vessels.
- The vessel shall not cause harm to the surrounding environment, including water quality, aquatic plants, or fish populations.


## **IEEE, TWRA, etc. Regulations and Constraints**

- 11. Battery Safety (UL 2054 – Household and Commercial Batteries)
 • The vessel’s battery systems shall comply with UL 2054 to ensure safe design, assembly, and operation, providing protection against fire, explosion, and electrolyte leakage.
- 12. Ingress Protection (IEC 60529 – IP Ratings)**
 • All electronic enclosures, sensors, and control modules shall meet an appropriate IEC 60529 IP rating to prevent ingress of dust and water under expected operating conditions.
- 13. Radio Frequency Compliance (FCC 47 CFR Part 15 – Radio Frequency Devices)
 • All wireless communication systems, including RC transmitters, receivers, and autonomous control modules, shall comply with FCC 47 CFR Part 15 to prevent harmful interference and ensure safe RF operation.
- 14. Hull Stability (ISO 8383:1985 – Ships and Marine Technology: Small Craft Stability)
 • The vessel’s hull design shall conform to ISO 8383:1985 to ensure adequate stability, buoyancy, and safety during operation in various load and environmental conditions.
- 15. Boating Regulations (Tennessee Wildlife Resources Agency – TWRA)
 • Operation of the vessel on public waterways shall comply with all applicable TWRA boating regulations, including safety, registration (if required), and operational conduct.
- 16. Waterway Use (Tennessee Valley Authority – TVA Waterway Regulations)
 • When operating within TVA-managed waters, the vessel shall comply with TVA waterway navigation and use regulations to ensure lawful and safe operation.
- 17. Environmental Protection (EPA Clean Water Act Compliance)
 • The vessel shall comply with the EPA Clean Water Act, ensuring that no pollutants, fuels, or chemicals are discharged into waterways during operation, maintenance, or testing.


## Resources
- Hardware
  - Boat Hull: Designed to provide structural support and room for mounting other hardware such as Raspberry pi, propulsion system, sensors, etc. Built to support 40-50 pounds in open water
  - Battery/Charging: Batteries with charging support to last 2-4 hours of continuous operation.
  - Microprocessor: Raspberry pi to process sensor data and interface with communication modules.
  - Autopilot/GPS: Module for autonomous navigation and GPS tracking.
  - Sonar/Transducer System: Primary sensor for depth measurement and underwater topography
  - Current Velocity System: Sensor used for measuring current velocity under the hull
  - Waterproof enclosure: Protection for electronics and sensors during operation.
- Communication and Data Processing
  - Telemetry Radio: Long-range communication for GPS and depth reading to reach data-processing system
  - Data Processing System: Software to process incoming data in real time. It will integrate sonar, GPS, velocity, and depth measurements to produce a synchronized dataset and generate a two-dimensional map for immediate visualization during operation.
- Software resources
  - Hypack: Industry-standard hydrographic survey software used for sonar data processing and integration.
  - QGIS: Free GIS platform for geospatial analysis, GPS point handling and mapping
  - ArcGIS Pro: Advanced geospatial for professional mapping and visualization.

### Budget

| Item/Material | Quantity | Cost (USD Estimates) |
| --- | --- | --- |
| Boat (Hull) | 1 | iMakerSpace fee |
| Battery/Charger | 1 | $25.00-$50.00 |
| Microprocessor | 2 | $80.00 |
| Sonar/Transducer | 1 | $400.00-$600.00 |
| Autopilot/GPS | 1 | $35.00 |
| Communication system to process data (Telemetry radios) | 1-2 | $40.00-$90.00 |
| Data Processing system | 1 | $0.00-100.00 |
| Propulsion Motor System | 1-2 | $50.00-$75.00 |
| yf-s201 water flow sensor | 1 | $10.00 |
| Hypack license (Survey Software) | 1 | $0.00 |
| QGIS license (GIS software) | 1 | $0.00 |
| ArcGIS pro license (GIS software | 1 | $100.00 |


### Division of Labor
Ryan Thomas: Communication Subsystem
Ian Hanna: Sensors Subsystem
Jackson Hamblin: Hardware Subsystem
Brady Nugent: Navigation Subsystem
Nathan Norris: Power Subsystem

### Timeline


## References

[1] Z. Li, Y. Gao, M. Xu, J. Liu, Y. Yang, and J. He, “Exploring modern bathymetry: A comprehensive review of methods, limitations, and future directions,” Frontiers in Marine Science, vol. 10, Apr. 2023. [Online]. Available: https://doi.org/10.3389/fmars.2023.1178845, ,

[2] F. Gerlotto, S. Gauthier, and B. Masse, “The application of multibeam sonar technology for quantitative estimates of fish density in shallow water acoustic surveys,” Aquatic Living Resources, vol. 13, no. 5, pp. 385–393, 2000. [Online]. Available: https://doi.org/10.1016/S0990-7440(00)01094-4

[3] DIY sonar transducer
[3] Teledyne Marine, “StreamPro ADCP,” Teledyne RD Instruments, 2023. [Online]. Available: https://www.teledynemarine.com

[4] SonTek, “RiverSurveyor RS5,” Xylem Inc., 2023. [Online]. Available: https://www.ysi.com/riversurveyor

[5] Blue Robotics, “Ping2 Echosounder and Altimeter,” Blue Robotics Inc., 2023. [Online]. Available: https://bluerobotics.com/store/sensors-sonars-cameras/sonar/ping-sonar-r2-rp

[6] CEESCOPE, “Integrated GNSS and single-beam echosounder for hydrographic surveys,” CEE Hydrosystems, 2023. [Online]. Available: https://www.ceehydrosystems.com

[7] Seafloor Systems Inc., “EchoBoat-160 Autonomous Survey Vessel,” Seafloor Systems, 2023. [Online]. Available: https://www.seafloorsystems.com

[8] Tersus GNSS, “TheDuck Autonomous Survey Boat,” Tersus GNSS Inc., 2023. [Online]. Available: https://www.tersus-gnss.com

[9] OTT Hydromet, “Qliner2: Mobile Doppler system for discharge measurements,” OTT Hydromet GmbH, 2023. [Online]. Available: https://www.ott.com

[10] ResearchGate, “Low-cost autonomous surface vehicles for inland water monitoring,” ResearchGate Publications, 2023. [Online]. Available: https://www.researchgate.net

[4] How to Connect, Read, and Process Sensor Data on Microcontrollers – A Beginner's Guide
[11] U.S. Geological Survey, Use of Acoustic Doppler Current Profilers for Streamflow Measurements, U.S. Dept. of the Interior, Reston, VA, USA. [Online]. Available: https://pubs.usgs.gov


## Statement of Contributions

- Ryan Thomas: Communications Subsystem, High Level Solution
- Brady Nugent: Operational Flowchart, Navigation Subsystem
- Ian Hanna: Sensors and Data Acquisition Subsystem, Hardware Block Diagram
- Nate Norris: Power subsystem, Resources
- Jackson Hamblin: Hardware subsystem, Comparative Analysis

All: Introduction, Restating Problem, References
