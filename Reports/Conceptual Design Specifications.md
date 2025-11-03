# Conceptual Design

## Introduction

Understanding the depth and terrain of lakes and other bodies of water is essential for environmental monitoring, infrastructure planning, and recreational management. The conventional method of underwater mapping relies on specialized boats, lots of manpower, and expensive sonar equipment. This makes it unattainable for smaller organizations and research groups. As a result, there is a growing need for affordable, autonomous systems capable of collecting and transmitting in-depth data to produce detailed underwater maps.

## Restating the Fully Formulated Problem

This problem affects environmental and local governments who need accurate lakes and reservoir data. This data can then be used for environmental monitoring, flood management, and infrastructure planning. It also impacts fisheries, recreational, and conservation groups. The current lake mapping methods tend to be expensive and require well-trained operators. So, a low cost, autonomous solution would allow for more frequent and efficient data collection without the high cost and specialized skills. This would allow lake mapping to become more expandable and accessible. While commercial sonar and autonomous boats do exist, the cost and specific skill set needed to use them limit the availability greatly. Consumer sonar systems may provide depth readings but lack autonomy, data mapping, and integration capabilities. So, a custom-built solution can guarantee affordability, versatility in differing environments, and control over the capacity of the solution’s growth.

## Comparative Analysis of Potential Solutions

The project requires reliable sensing for both lake bottom depth mapping and **water current flow measurement**. Selecting the proper technologies for sonar-based depth sensing and flow monitoring is essential to balance affordability, accuracy, integration complexity, and long-term adaptability.

### **Depth Measurement Solutions (Sonar and Transducers)**

- **1. Consumer Fish Finder/Depth Finder Transducers**
 These systems are widely available, often costing under $500, and provide basic point depth readings. While inexpensive and easy to operate, they lack research-quality accuracy, real-time mapping integration, and compatibility with GIS platforms.

<img width="152" height="229" alt="image" src="https://github.com/user-attachments/assets/9dfb326e-636b-459f-9f45-c1f6cd724837" />


- **2. Single-Beam Echo Sounder with Transducer (e.g., Blue Robotics Ping2)**
 Costing approximately $400–$600, these sonar units deliver ±5–15 cm accuracy and support open-source integration with Raspberry Pi and autopilot modules. They are well-suited for proof-of-concept mapping, offering the best balance of affordability, accuracy, and adaptability.

<img width="153" height="192" alt="image" src="https://github.com/user-attachments/assets/69a171ef-f707-49f9-99a8-1368420c9afd" />


- **3. Survey-Grade Single-Beam Echo Sounders (e.g., CEESCOPE with RTK GNSS)**
 These professional systems achieve ±2–5 cm accuracy and integrate seamlessly into commercial hydrographic workflows. However, their cost exceeds $10,000, making them impractical for a budget-constrained student project.

<img width="186" height="186" alt="image" src="https://github.com/user-attachments/assets/0bf1edf6-9356-4631-b518-15340b7d9377" />


- **4. Multibeam Sonar Systems**
 Industry-standard for hydrography, multibeam systems provide swath coverage with ±1–2 cm precision. Yet, costs ($50,000–$100,000+) and required RTK-INS navigation systems render them infeasible for this project.


### Flow Measurement Solutions (Including YF-S201 Sensor)

1. YF-S201 Hall-Effect Flow Sensor: 
 The YF-S201 is a low-cost (~$10–$20) sensor that uses a turbine and Hall-effect sensor to measure flow rate. It outputs digital pulses proportional to the water velocity. While not as precise as acoustic methods, it is extremely affordable, lightweight, and easy to integrate with microcontrollers such as Arduino or Raspberry Pi. Its accuracy is typically within ±10% after calibration, making it suitable for small-scale, student-built systems.

<img width="214" height="143" alt="image" src="https://github.com/user-attachments/assets/2030f3d8-07c6-4a10-a374-97cbc60a4b11" />


### Vessel Propulsion and Turning Solutions:

1.	HPI SF‑50WP Servomotor
The HPI SF‑50WP is a waterproof servo motor designed for wet environments, capable of providing precise rotational control with moderate torque suitable for small autonomous vessels. In a propulsion application, the servo can be mechanically coupled to a rotor or propeller shaft to generate forward and reverse thrust. When controlled via standard PWM signals from a microcontroller or autopilot system, the pulse width determines the servo shaft’s rotation, which drives the propeller to propel the vessel. For improved maneuverability, two servos mounted on opposite sides of a catamaran hull can operate differentially, spinning in opposing directions or modulating speed to enable turning without a traditional rudder. The servo’s integrated electronics simplify interfacing with onboard control systems, while its waterproof design ensures reliable operation in wet or splashing conditions. This setup provides a compact, low-cost, and easily controllable propulsion and steering solution for lightweight autonomous boats.

<img width="204" height="163" alt="image" src="https://github.com/user-attachments/assets/8d39f1f4-2166-4bfb-ab3c-3663c35432d5" />


### Power Systems Solutions:

1. 12 V Deep-Cycle Marine Battery
The 12 V deep-cycle marine battery serves as a robust primary power source for the vessel, providing sustained energy for propulsion, sensors, and control electronics. Designed to handle repeated deep discharges, it delivers consistent voltage under load and is well-suited for marine environments due to its sealed, spill-resistant construction. Power is drawn from the battery via the distribution system to all subsystems, and its capacity ensures several hours of continuous operation before recharge is required.

<img width="120" height="79" alt="image" src="https://github.com/user-attachments/assets/6022f2b9-43d4-42c2-a418-2d7e65ce5e98" />


2. 14.8 V Lithium-Ion Battery Encased in Waterproof Container
 The 14.8 V lithium-ion battery provides a higher energy density alternative for extended mission duration while maintaining a lightweight form factor. Encased in a waterproof container, it is protected from splashes and accidental immersion. The battery supplies regulated DC power to propulsion, navigation, and sensor systems, while integrated protection circuitry prevents overvoltage, overcurrent, and thermal issues, ensuring safe operation in varying environmental conditions.

<img width="220" height="220" alt="image" src="https://github.com/user-attachments/assets/8792f29b-2838-4075-ba41-eb9b3fddcae5" />


3. Solar Backup Integration (ECO-WORTHY 130 Watt 12BB Flexible Solar Panel)
 The solar backup integration supplements the main battery system by trickling the onboard battery through a solar panel and charge controller. The controller regulates incoming energy to prevent overcharging or reverse current flow, ensuring safe and efficient energy transfer. During operation, the solar panel can extend mission endurance or provide emergency power in case of battery depletion, enabling longer deployments without external charging.

<img width="256" height="256" alt="image" src="https://github.com/user-attachments/assets/21d0d77c-8664-47e2-9410-9654d6876bac" />


### **Benefits vs. Costs**

The **Blue Robotics Ping2 single-beam sonar** offers the best balance for depth mapping: affordable, accurate within ±5–15 cm, and open-source friendly.
 For flow measurement, the **YF-S201 sensor** provides a budget-conscious alternative. Although less precise than ADCPs, it offers sufficient accuracy (±10%) for a capstone project, particularly when validated against reference datasets. Its ease of integration and extremely low cost make it highly attractive.


### **Most Likely to Succeed**

The chosen configuration combines a **single-beam sonar transducer (Ping2 or equivalent)** for depth measurement with a **YF-S201 Hall-effect flow sensor** for current velocity monitoring. Together, these solutions maximize affordability, accuracy, and ease of integration. The sonar system achieves the project’s bathymetric goals within a reasonable budget, while the YF-S201 enables basic flow monitoring without requiring costly acoustic Doppler systems.
This hybrid approach balances technical feasibility with project constraints and provides a scalable platform that can be expanded with more advanced sensors in the future.

### Navigation Subsystem Solutions (Including GPS and Autopilot Options)

1. u-blox NEO-M8N GPS Module 
 
The NEO-M8N is a cost-effective (~$20–$80) GNSS receiver that provides position accuracy within 2–5 meters under open-sky conditions. It communicates using standard serial (UART) NMEA or UBX protocols, making it easy to interface with Arduino, Raspberry Pi, or Pixhawk controllers. While not RTK-capable, it is lightweight, power-efficient, and widely supported by the ArduPilot and PX4 ecosystems. This makes it ideal for student-level autonomous boats requiring basic waypoint navigation and mapping.

<img width="209" height="149" alt="image" src="https://github.com/user-attachments/assets/297cba03-1134-4448-884d-1b8b819682a6" />


2. u-blox ZED-F9P RTK GNSS Receiver
 
The ZED-F9P offers high-precision dual-band GNSS positioning with centimeter-level accuracy when paired with RTK correction data. Typical cost ranges from $170–$300, with ready-to-use evaluation boards such as the SparkFun GPS-RTK2 or simpleRTK2B. It supports UART, I2C, and USB interfaces and can act as either a base or rover in RTK configurations. This module is well-suited for research or capstone-level autonomous boats requiring precise mapping and navigation performance.

<img width="229" height="174" alt="image" src="https://github.com/user-attachments/assets/5e209067-cf9d-484b-ae02-406f0b714442" />

3. Pixhawk Autopilot Controller
 
Pixhawk autopilots (~$120–$300) are open-source flight controllers compatible with ArduPilot and PX4 firmware. They include onboard IMUs, magnetometers, barometers, and multiple serial ports for GPS and telemetry integration. When configured with “Rover/Boat” firmware, Pixhawk can handle waypoint missions, motor control, and path-following surface vehicles. It is a proven, reliable option for small autonomous boats and integrates seamlessly with u-blox GPS modules.

<img width="198" height="183" alt="image" src="https://github.com/user-attachments/assets/8d49b93e-ec82-482d-80fa-a991f5886ddb" />

4. Navio2 (Raspberry Pi-Based Autopilots)
 
The Navio2 (~$199) can expand Raspberry Pi boards into full-featured autopilots with integrated IMUs, GPS interfaces, and PWM outputs. They allow for more flexible software control using Linux and companion programs like BlueOS or ROS. These solutions are excellent for teams wanting real-time data processing, networking, and camera or sensor integration alongside navigation. 

<img width="192" height="160" alt="image" src="https://github.com/user-attachments/assets/39c6406b-dca6-4db4-8590-be5d908cbde8" />


#### Benefits vs. Costs

The u-blox NEO-M8N GNSS receiver and Navio2 Raspberry Pi-based autopilot together offer the best balance between affordability, functionality, and navigation performance. The NEO-M8N provides reliable, meter-level positioning accuracy (2–5 m CEP) under open-sky conditions for only $20–$80, making it ideal for student or capstone-level autonomous boats. It consumes minimal power and integrates easily via UART, simplifying setup and reducing total system cost. The Navio2 expands a Raspberry Pi into a full-featured autopilot capable of running ArduPilot Rover/Boat firmware, handling motor control, waypoint navigation, and onboard sensor fusion. Although more expensive (~$199 plus Raspberry Pi), it offers significant long-term value through software flexibility, real-time processing, and compatibility with additional sensors such as cameras, LiDAR, or RTK GPS modules. Combined, these components deliver a high-performing navigation subsystem for approximately $250 total, far less than comparable Pixhawk + ZED-F9P configurations while still meeting project accuracy and functionality requirements.

#### Most Likely to Succeed

The selected configuration integrates the NEO-M8N GPS module with the Navio2 autopilot, leveraging each system’s strengths to ensure reliable, cost-effective navigation and control. The NEO-M8N supplies continuous position data sufficient for waypoint tracking and mapping, while the Navio2 provides onboard computation, sensor management, and communication with motor controllers—all within a single, Linux-based platform. This hybrid setup maximizes affordability, accuracy, and ease of integration, aligning well with capstone project constraints. It ensures a stable and well-documented system backed by the ArduPilot and Emlid communities, reducing risk and setup time. Additionally, the design remains scalable for future upgrades, such as replacing the NEO-M8N with an RTK-capable receiver for centimeter-level precision.Overall, the Navio2 + NEO-M8N solution provides the most practical and dependable foundation for an autonomous boat navigation subsystem—balancing cost, technical feasibility, and project success potential.

## **High-Level Solution**

The autonomous lake-mapping vessel integrates multiple coordinated subsystems to efficiently meet all stakeholder goals and project requirements while remaining within design constraints. The system combines a durable fiberglass-reinforced 3D-printed hull, efficient power management, and reliable communication to deliver accurate bathymetric mapping and autonomous navigation. High-precision sonar and water velocity sensors collect depth and location data, which are processed and transmitted via a wireless link to an on-shore computer for real-time monitoring. The modular battery configuration and optional solar backup ensure power reliability, while onboard SD storage prevents data loss in the event of communication failure. Lightweight materials, optimized control algorithms, and energy-efficient components reduce power consumption and maximize operational endurance. The design emphasizes safety, maintainability, and cost-effectiveness, achieving a balance between performance and resource optimization to provide a robust, autonomous, and user-friendly data collection platform.


## Hardware Block Diagram

<img width="1948" height="978" alt="image" src="https://github.com/user-attachments/assets/47834625-ea62-41f9-8d7f-2753a1e648f7" />


## Operational Flow Chart

<img width="515" height="343" alt="image" src="https://github.com/user-attachments/assets/3f387c0e-4f53-48b0-8423-2536365d000f" />


## Atomic Subsystem Specifications

### Subsystem 1: (Hardware)

### Function:

The Hardware Subsystem serves as the structural and mechanical foundation of the autonomous vessel. It supports all other subsystems—power, propulsion, communication, and sensors—by providing buoyancy, stability, and mounting surfaces for equipment. The vessel features a catamaran-style hull to enhance stability and hydrodynamic performance. It is designed to accommodate a payload of approximately 40 to 50 pounds, including all electronic components, sensors, and power systems. The hull will be 3D printed in modular sections for ease of fabrication and assembly and then reinforced with a fiberglass coating to ensure durability, water resistance, and impact protection during operation. The subsystem also includes the primary onboard instruments, such as the Lowrance Elite-5 DSI depth finder and the main battery system, which together enable continuous operation and real-time data collection.

### Interfaces:

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

### Operation:

The Hardware Subsystem operates as the physical base for all vessel systems.
- The 3D printed catamaran hull provides stability and balance for smooth operation in variable water conditions.
- Internal compartments house and protect sensitive electronics, including the power system, control boards, and communication modules.
- The Lowrance Elite-5 DSI depth finder or other sonar devices operate      continuously, transmitting sonar pings through its transducer to measure depth and detect underwater features.
- Collected depth data are transferred through the communication and data processing subsystem to the base station for live analysis.
- The vessel’s battery system provides consistent power to all onboard electronics for a minimum of two to four hours of continuous operation.
- The fiberglass coating ensures water resistance and enhances durability against environmental stress and minor impacts.
This subsystem ensures the vessel maintains structural integrity, buoyancy, and reliability under all expected environmental conditions while supporting continuous data collection and transmission.

### Shall Statements:

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

### Function:

The Power Subsystem supplies, regulates, and distributes electrical power to all onboard components, including the propulsion motors, sensors, communication module, and control electronics. It ensures stable operation across all environmental and load conditions while incorporating safety mechanisms to prevent overcurrent, overvoltage, and thermal damage. The subsystem also manages power input from external charging sources such as AC adapters or solar panels when applicable.

### Power Supply Components:

### Main Battery System

- **Type:** 12V Deep Cycle Marine Battery or Lithium Ion Battery encased in waterproof container
- **Function:** Provides the primary power source for all onboard systems, offering high energy density and resistance to vibration and moisture.
- **Capacity:** Sized to support a minimum of 2–4 hours of continuous operation under full load.
  
### Voltage Regulation Module

#### Function:

Steps down or regulates power to required voltage levels for various subsystems (e.g., 12V → 5V for microcontrollers and sensors).

#### Components:

Includes DC-DC buck converters and voltage regulators with thermal protection and efficiency >85%.

### Power Distribution Board (PDB)

#### Function:

Distributes regulated power from the main battery to all subsystems via fused lines.

#### Features:

Integrated current sensing for monitoring, reverse polarity protection, and master power switch for manual cutoff.

### Solar Backup Integration (Optional)

#### Function:

Allows trickle-charging of the main battery via a solar panel to extend operational endurance during long-duration missions.

#### Operation:

The solar charge controller prevents overcharging and backflow to ensure system safety.
- It can also serve as an emergency power source if battery gives out.

#### Interfaces:

Connection Type:
- Direct wired power connections using marine-grade insulated cables
  
Signal Type:
- DC electrical power distribution
  
Direction:
- Output from Power Subsystem to all other subsystems
  
Protocol:
- None (power lines only)
  
Data Sent:
- Voltage and current monitoring feedback (optional analog output from sensors to microcontroller)
  
Data Received:
- Power-on command signals (digital) from control subsystem
- Charging input from solar or AC source

### Overall Operation:

During startup, the power distribution board energizes the propulsion system, sensors, and control electronics sequentially to prevent current surges. The voltage regulator ensures that all sensitive devices receive stable DC levels. Battery levels are monitored continuously through analog feedback to the microcontroller, which can alert the operator via the communication subsystem if voltage drops below the critical threshold. When docked, the system automatically switches to charging mode if an external power input is detected.

### Shall Statements:

- The subsystem shall provide stable DC power to all connected subsystems within ±5% of nominal voltage.
- The subsystem shall maintain at least 2 hours of full operational power under typical mission load.
- The subsystem shall include voltage and current protection for all output lines.
- The subsystem shall allow for external recharging via AC or solar input.
- The subsystem shall monitor and report battery voltage to the communication subsystem every 10 seconds.
- The subsystem shall feature a main power switch to manually disconnect the entire vessel’s power supply.
- The subsystem shall isolate high-current propulsion power from sensitive electronics to prevent interference.
- The subsystem shall provide reverse polarity and short-circuit protection for all outputs.

### Subsystem 3: (Navigation)

### Function:

Provides absolute position, time, and velocity data for real-time tracking, route following, and mission mapping. The u-blox NEO-M8N GNSS receiver supplies high-sensitivity multi-constellation positioning (GPS, GLONASS, Galileo, QZSS) and supports optional RTK correction for sub-meter accuracy.

### GPS Positioning System

The GPS Positioning Subsystem provides absolute position, time, and velocity data to the navigation and autopilot subsystems. It determines the vessel’s precise location on the water and supports real-time tracking, route following, and mission mapping. The subsystem utilizes a high-sensitivity GNSS receiver with an integrated antenna and supports standard NMEA and vendor-specific protocols for data output.
For advanced accuracy, the subsystem optionally accepts RTK (Real-Time Kinematic) correction data to achieve sub-meter positional precision. The GPS continuously transmits satellite health, fix status, and position information to the onboard navigation computer and autopilot controller for real-time guidance and logging.

#### Interfaces:

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

#### Operation:

- Calculates latitude, longitude, altitude, and UTC time from multiple constellations
- Outputs NMEA or UBX messages at 1–10 Hz update rates
- Sends satellite count, fix quality, and HDOP metrics to navigation computer
- Accepts RTCM3 corrections for sub-meter accuracy
- Maintains last known fix and timestamp during signal loss
- Powered from 5 V DC, waterproof antenna ensures optimal satellite visibility

#### Shall Statements:

- Shall output latitude, longitude, altitude, and UTC time at 1–10 Hz
- Shall provide fix quality and satellite count with each update
- Shall communicate via UART (3.3 V logic) or USB virtual COM
- Shall accept configuration commands via RX line
- Shall support RTCM3 RTK correction for sub-meter accuracy
- Shall operate with 3.3–5 V DC and reverse-polarity protection
- Shall flag stale data if no fix within twice the update interval
- Shall include a waterproof antenna with strain relief

### Autopilot Subsystem

#### Function:

The Autopilot Subsystem fuses navigation data, onboard sensor measurements, and operator commands to autonomously guide the vessel along pre-defined routes. It maintains stable heading, speed, and position control through closed-loop feedback using an integrated IMU, magnetometer, barometer, and GPS input.
This subsystem executes mission management, guidance, and control algorithms, ensuring the vessel follows planned waypoints and coverage paths while responding to environmental and system conditions. The autopilot communicates with the navigation and communication subsystems to exchange telemetry, mission updates, and safety alerts.


#### Interfaces:

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


#### Operation:

- Fuses GPS and onboard sensor data using an Extended Kalman Filter (EKF)
- Generates PWM or CAN ESC commands for propulsion and steering
- Runs attitude control loops ≥ 50 Hz and navigation loops 5–20 Hz
- Sends telemetry and mission updates via MAVLink
- Supports failsafe modes (loiter, stop, return-to-home)
- Interfaces with Raspberry Pi 4 for high-level mission control
- Allows manual override at any time via RC or kill switch

#### Shall Statements:

- Shall receive GPS data from the navigation subsystem via UART
- Shall fuse IMU, magnetometer, barometer, and GPS data using EKF at ≥ 5 Hz
- Shall generate actuator outputs (PWM/DShot) at 50–400 Hz
- Shall communicate using MAVLink for telemetry and mission commands
- Shall manage waypoint, loiter, and return-to-home functions
- Shall trigger failsafe responses for GPS loss, telemetry loss, or low battery
- Shall allow manual override via RC input
- Shall log all navigation and control data with timestamps
- Shall operate with 5–12 V DC and provide voltage/current sensing
- Shall maintain < 100 ms control latency under normal operation

### Subsystem 4: (Communication)

The Communication and Data Processing Subsystem provides the primary link between the vessel’s onboard systems and the base station computer located on land. It manages both the transmission of collected sensor data—such as GPS coordinates, depth readings, and system status—and the reception of operator commands. The subsystem uses a wireless communication link, established through a Wi-Fi or RF telemetry module, to ensure reliable, real-time data transfer.

The onboard controller, a Raspberry Pi 4, serves as the central processing unit for managing communication and local data handling. In the event of a loss of connection, data are automatically stored on an onboard SD card to ensure no information is lost. Additionally, the SonTek Power and Communication Module (PCM) may be used to interface the sonar system with the main communication link, ensuring synchronized and noise-free data exchange for accurate bathymetric mapping and velocity measurements.

### Interfaces:

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

### Operation:

The Communication and Data Processing Subsystem serves as the vessel’s central communication hub, ensuring all mission data and commands are exchanged accurately and efficiently between onboard systems and the operator.

- Sensor and navigation data from the Arduino-based Data Acquisition Subsystem are transmitted to the Raspberry Pi 4.
- The Raspberry Pi formats the incoming data into structured packets containing GPS position, water depth, and vessel system status.
- These packets are transmitted over Wi-Fi or RF telemetry to the base station computer, allowing the operator to monitor the vessel’s progress in real time.
- Incoming commands from the base station (e.g., mission start, stop, or return) are received and relayed to the appropriate onboard subsystem.
- The SonTek Power and Communication Module (PCM) synchronizes sonar and velocity data, ensuring consistent and noise-free transmission.
- In case of network interruption, the Raspberry Pi stores all data locally on an SD card, resuming transmission once the connection is reestablished.
- The subsystem continuously monitors communication health and provides feedback on link strength and data integrity to the operator interface.
This operation ensures reliable, low-latency communication between the vessel and the control station throughout all mapping missions.

### Shall Statements:

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

### Transducer – Lowrance HDS-5 Fish Finding System

#### Function:

The transducer provides underwater depth measurements and positional data via its built-in GPS receiver. It operates as the primary instrument for bathymetric data collection, transmitting sonar pings to determine water depth relative to the transducer's face.

#### Interfaces:

- Connection Type: NMEA 2000 network (differential CAN bus)
- Signal Type: Digital data communication
- Direction: Input to Arduino system
- Protocol: NMEA 2000, 250 kbps
- Data Sent:
  - Depth measurement (PGN 128267)
  - GPS position and time (PGN 129029)
- Data Received: None (read-only connection)

#### Operation:

The Arduino Uno reads incoming NMEA 2000 messages through a CAN controller (MCP2515) and transceiver (TJA1050). The system extracts water depth, latitude, longitude, and time data, synchronizing them with other sensor inputs for unified data logging.

#### Shall Statements:

- The subsystem shall receive depth and GPS data from the Lowrance HDS-5 via the NMEA 2000 network.
- The subsystem shall extract and record latitude, longitude, and time from the GPS feed.
- The subsystem shall operate in read-only mode to avoid interference with the NMEA 2000 backbone.
- The subsystem shall store all received data to the SD card in a timestamped format.

### Autonomous Water Flow Control and Monitoring System

#### Function:

This subsystem measures the water’s flow rate and total volume using a YF-S201 Hall-effect flow sensor, while also utilizing an infrared sensor (LM393) for proximity detection. A relay module is available for controlling connected actuators or valves as needed for future automation features. The original design based on an ESP8266 microcontroller has been adapted to operate on the Arduino Uno platform.

#### Interfaces:

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

#### Operation:

The Arduino counts pulses from the YF-S201 sensor using an interrupt routine to compute real-time flow rate and cumulative volume. The LM393 IR sensor provides an obstacle or fluid presence signal, while the relay module can be activated for flow control or safety response.

#### Shall Statements:

- The subsystem shall measure flow rate using the YF-S201 Hall-effect sensor.
- The subsystem shall compute and log both instantaneous flow rate (L/min) and total volume (L).
- The subsystem shall detect nearby objects or changes using the LM393 IR sensor.
- The subsystem shall provide control signals to a relay module for optional actuation.
- The subsystem shall transmit all processed data to the Arduino data logger at a frequency of one record per second.

### Obstacle Avoidance Subsystem

#### Function:

The obstacle avoidance subsystem ensures safe navigation by detecting nearby physical obstacles above or below the waterline. It utilizes a combination of infrared (IR) proximity sensors and a sonar transducer to provide short- and mid-range detection.

#### Interfaces:

- Connection Type: Wired to Arduino digital I/O pins
- Signal Type:
  - Digital (IR sensor trigger)
  - Analog or timing pulse (ultrasonic transducer echo)
- Direction: Input to Arduino
- Protocol: Trigger/echo timing and TTL logic signals
- Data Sent: Distance measurement (cm), obstacle detection flag

#### Operation:

When the sonar sensor emits a pulse, the reflected echo is timed by the Arduino to calculate the distance to the nearest obstacle. Simultaneously, the IR sensor detects close-range obstacles and sends a binary high/low signal. The system flags potential hazards in real time and includes obstacle data in the logged dataset.

#### Shall Statements:

- The subsystem shall measure obstacle distance using ultrasonic sonar.
- The subsystem shall detect close-range objects with the LM393 IR sensor.
- The subsystem shall flag obstacles within one meter as potential hazards.
- The subsystem shall provide an obstacle flag output to the Arduino logger.
- The subsystem shall operate continuously during all mapping missions.

### Data Logging and Control Integration

#### Function:

All sensor subsystems interface through the Arduino Uno, which acts as the central processor and data logger. The Arduino aggregates data from the transducer, flow, and obstacle sensors, synchronizes it with GPS timestamps, and writes it to an SD card in a structured CSV format.

#### Interfaces:

- Connection Type: SPI for SD card, UART for communication
- Signal Type: Digital serial
- Direction: Bi-directional (read/write)
- Protocol: SPI @ 8 MHz, CSV file structure for logging
- Data Sent: Combined dataset of all measurements
- Data Received: System commands (start/stop, calibration)

#### Operation:

During operation, the Arduino performs a one-second logging cycle that gathers all active sensor readings. Each record contains timestamped data for depth, position, flow rate, and obstacle detection. The system also handles error detection and performs safe data flushing on power loss.

#### Shall Statements:

- The subsystem shall record synchronized sensor data at least once per second.
- The subsystem shall store data in CSV format with fields for time, GPS coordinates, depth, flow rate, and obstacle status.
- The subsystem shall perform SD card write verification and flush data upon shutdown.
- The subsystem shall communicate summary data to the onboard communication module.

### System Integration Summary

All sensors and modules are interconnected through the Arduino Uno, forming a unified sensing and acquisition platform. The system collectively enables real-time environmental measurement, data synchronization, and onboard storage for subsequent processing and visualization.


## Ethical, Professional, and Standards Considerations

### Specifications and Constraints

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


### **IEEE, TWRA, etc. Regulations and Constraints**

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


1.	Hardware

    a.	Boat Hull: Designed to provide structural support and buoyancy for all onboard equipment, including the propulsion system, Raspberry Pi, sensors, and power modules. The catamaran-style hull is 3D printed in modular sections and reinforced with fiberglass, supporting a total payload of 40–50 pounds for stable operation in open water.

    b.	Propulsion Motor (T200 Thrusters): Dual brushless DC thrusters providing primary propulsion and steering through differential thrust.

    c.	Servo Motor: Actuators used for rudder control or precise directional adjustments when operating with single-thruster setups.

    d.	Waterproof enclosure: Housing for all electronics to provide durability and long-term moisture protection

3.	Power
   
    a.	Battery and Charging System: Primary energy source consisting of 14.8 V Li-ion or LiFePO₄ batteries with an integrated Battery Management System (BMS). Provides 2–4 hours of continuous operation and supports recharging via shore charger or optional solar input.

    b.	Power Distribution Board: Central hub that distributes regulated power from the main battery to all subsystems including control, sensors, and communication modules.

    c.	Voltage Distribution Board: Supports multiple voltage outputs (e.g., 5 V, 12 V) to safely supply mixed-voltage devices.

    d.	Voltage Regulation Chips: Provide clean, consistent DC power to sensitive components such as microcontrollers and radios.

    e.	Solar Panels (optional): Renewable energy source to extend field operation time through trickle-charging.

5.	Navigation

    a.	Arduino: Functions as the sensor interface and data acquisition controller, directly managing low-level peripherals such as the YF-S201 flow sensor, IR proximity sensors, and relay modules. It communicates with the Raspberry Pi through serial connection for synchronized data logging and control feedback.

    b.	Autopilot: Embedded navigation controller that interprets GPS and IMU data to autonomously adjust heading, maintain course, and perform survey paths.

7.	Communication and Data Processing

    a.	Base Station Computer: Ground-based control and monitoring system used for mission oversight, real-time telemetry, and post-processing visualization.

    b.	Softlogic Power and Communication Module: Custom module that integrates power management with telemetry and data relay functions between vessel and operator.

    c.	Raspberry Pi: Main onboard processing unit for data storage, sensor fusion, and wireless communication with the base station. Handles mission logic, navigation routines, and telemetry management.

9.	Sensors and Data Acquisition

    a.	YF-S201 Water Flow Sensor: Provides digital pulse output proportional to water velocity for current profiling.

    b.	Data Processing System: Software and embedded routines that process sonar, GPS, velocity, and depth measurements into synchronized datasets and generate near-real-time bathymetric maps.

    c.	Depth Finder (Lowrance Elite-5 DSI): Primary sonar system for depth and underwater topography acquisition.

11.	Possible Software

    a.	Hypack: Industry-standard hydrographic survey software used for sonar data processing and integration.

    b.	QGIS: Open-source GIS platform for geospatial analysis, GPS point handling and mapping

    c.	ArcGIS Pro: Advanced geospatial for professional mapping and visualization.


### Budget

| Subsystem	| Item/Material	| Quantity	| Cost (USD Estimates) |
| --- | --- | --- | --- |
| Hardware	| | | |
|  | Hull |	1 |	iMakerSpace fee |
|  | Propulsion Motor (T200 Thrusters) |	1-2 |	$100.00-$200.00 |
|  | Servo Motor |	1-2	| $85.00 |
|   | Waterproof Enclosure | 	1	| $0.00-$20.00 |
|  | Electronic Speed Controllers (ESCs) | 2 | $35.00-$60.00 |
| Power | | | |
|   | Battery/Charger |	1-2 |	$25.00-$50.00 |
|   | Power Distribution Board |	1 |	$25.00-$40.00 |
|   | Voltage Distribution Board |	1 |	$40.00-$90.00 |
|   | Voltage Regulation Chips |	2-4 |	$5.99-$8.99 |
|   | ECO-WORTHY 130 Watt 12BB Flexible Solar Panel |	1-2 |	$69.99 |
| Navigation | | |	|
|  | Arduino | 1 | $25.00-$35.00 |
|  | Autopilot | 1 | $100.00-$200.00 |
| Communication | | | |	
|  | Base Station Computer | 1 | Preowned  |
|  | SonTek Power and Communication Module | 1 | Price to be requested |
|  | Raspberry Pi | 1 | $80.00-$120.00 |
|  | Telemetry Radio System | 1 | $40.00-$90.00 |
| Sensors and Data Acquisition | | | |
|  | YF-S201 Water Flow Sensor | 1 | $10.00 |
|  | Data Processing System | 1 | $0.00-$100.00 |
|  | Depth Finder (Lowrance Elite-5 DSI) | 1 | Preowned |
|  | Current Velocity Sensor | 1 | Included with YF-S201 |
|  | Sonar/Transducer System | 1 | Preowned |
| Possible Software | | | |			
|  | Hypack License (Survey Software) | 1 | $0.00 |
|  | QGIS License (GIS Software) | 1 | $0.00 |
|  | ArcGIS Pro License (GIS Software) | 1 | $100.00 |




### Division of Labor

Ryan Thomas: Communication Subsystem

-	Classes taken: Signals and Systems, Intro to Power Systems, Microcomputer Systems
  
-	Skills: C++, Assembly, Signal Processing

Ian Hanna: Sensors Subsystem

-	Classes taken: Microcomputer Systems, Digital System Design, Signals and Systems
 
-	Skills: C++, Assembly
  
Jackson Hamblin: Hardware Subsystem

-	Classes taken: Intro to Power, Signals and Systems, Microcomputer systems
  
-	Skills, Power Systems, CAD modeling, C++
  
Brady Nugent: Navigation Subsystem

-	Classes taken: Signals and Systems, Controls, Microcomputer Systems
  
-	Skills: C++, Assembly, Control Analysis
  
Nathan Norris: Power Subsystem

-	Classes taken: Intro to Power, Signals and Systems, Microcomputer Systems
  
-	Skills: Power systems, C++, Assembly, Control Analysis



### Timeline
<img width="468" height="167" alt="image" src="https://github.com/user-attachments/assets/d1cf6f43-aa3b-422e-b40e-372691a27c62" />


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
