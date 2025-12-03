# Navigation System

## Function of the Subsystem

  The Navigation System is responsible for enabling the autonomous boat to determine its
  position, plan a route, and execute precise motion control while performing lake-mapping
  operations. This subsystem forms the core of autonomous behavior by integrating sensor
  data, processing navigation commands, and actuating the boat’s thrusters and control
  surfaces.

  At the highest level, the subsystem uses a Pixhawk 6C autopilot controller running
  ArduPilot firmware, managed through Mission Planner for configuration, mission planning,
  and telemetry monitoring. ArduPilot provides robust real-time navigation, stabilization, and
  low-level attitude control. The Pixhawk receives global positioning information from the
  GNSS module, inertial data from onboard IMUs, and mission waypoints from the
  companion computer. Using this information, it generates control outputs to the servos
  and thrusters to maintain heading, speed, and trajectory. [2]

  Power for the Pixhawk is supplied through the PM02 Power Module, which regulates
  voltage and monitors current from two lithium batteries. This ensures safe and stable
  power delivery while providing battery health data to the autopilot. [1]

  A Raspberry Pi 4 acts as the companion computer, handling high-level decision-making,
  mission coordination, sonar data processing, and dynamic updates to mission
  parameters. It communicates with the Pixhawk using the MAVLink protocol over a serial
  UART connection via the TELEM1 port. The physical wiring for MAVLink consists of: [4]

  •  Pixhawk TX → Pi RX (GPIO15, Pin 10)
  •  Pixhawk RX → Pi TX (GPIO14, Pin 8)
  •  Pixhawk GND → Pi GND (Pin 6)

  This simple three-wire connection enables full MAVLink communication for mission
  commands and telemetry. [4]

  The Blue Robotics Ping 1D sonar transducer connects to the Pixhawk via a UART interface,
  providing real-time depth measurements for lake mapping. This sensor data is integrated
  into navigation decisions to ensure accurate mapping and obstacle avoidance. [3]

##  Specifications and Constraints

## Specifications

1.  The navigation subsystem shall use a Pixhawk 6C autopilot controller. [1]
2.  The Pixhawk 6C shall include an FMU processor based on STM32H743, 32-bit Arm®

Cortex®-M7, operating at 480 MHz with 2 MB flash and 1 MB SRAM. [1]

3.  The Pixhawk 6C shall include an IO processor based on STM32F103, 32-bit Arm®

Cortex®-M3, operating at 72 MHz with 64 KB SRAM. [1]

4.  The subsystem shall integrate onboard sensor, Accelerometer/Gyro: ICM-42688-P

and BMI088

5.  The Pixhawk 6C shall support a maximum input voltage of 6 V and USB power input

between 4.75–5.25 V. [1]

6.  The servo rail shall accept input voltage between 0–36 V.
7.  Each telemetry port shall be limited to a current of 1.5 A.
8.  The Pixhawk 6C shall provide 16 PWM servo outputs (hardware switchable between
3.3 V or 5 V), 3 general-purpose serial ports, 2 GPS ports, 1 I²C port, and 2 CAN
buses. [6]

9.  The Pixhawk 6C shall have dimensions of 84.8 × 44 × 12.4 mm and weight of 59.3 g

(aluminum case) or 34.6 g (plastic case). [1]

10. The subsystem shall operate within a temperature range of -40 °C to 85 °C.

## Constraints

1.  The navigation subsystem shall include a regulated power input (max 6 V) using a

power distribution board or UBEC for safe operation.

2.  The subsystem shall be designed to operate within -40 °C to 85 °C and include

waterproofing and thermal management for lake conditions.

3.  All communication interfaces shall comply with ArduPilot/Mission Planner

protocols, and UART wiring shall support MAVLink protocol for Pixhawk–Raspberry
Pi communication. [4]

4.  The system shall implement failsafe modes to prevent uncontrolled movement in

case of sensor failure, in accordance with ArduPilot standards. [7]

5.  Component selection shall balance cost-effectiveness and reliability, prioritizing

open-source compatibility and affordability.

##  Overview of Proposed Solution

The proposed navigation solution integrates a Pixhawk 6C autopilot controller running
ArduPilot firmware, paired with a Raspberry Pi 4 companion computer to achieve
autonomous lake mapping. This architecture ensures robust real-time navigation, precise
motion control, and dynamic mission adaptability. [5]

The Pixhawk 6C handles low-level vessel control, including heading stabilization, speed
regulation, and thruster actuation. It processes data from onboard sensors (IMU,
magnetometer, barometer) and external GNSS for accurate positioning. Mission waypoints
and control parameters are configured through Mission Planner, which provides a user-
friendly interface for planning and monitoring operations. [2]

The Raspberry Pi 4 performs high-level tasks such as mission coordination, sonar data
processing, and adaptive route updates. It communicates with the Pixhawk using the
MAVLink protocol over a serial UART connection, enabling bidirectional exchange of
mission commands, telemetry, and system health data. The Blue Robotics Ping 1D sonar
transducer connects to the Pixhawk to provide depth measurements, enabling real-time
mapping of underwater terrain. [3]

Power is supplied by two lithium batteries through regulated circuits to maintain safe
voltage levels for all components. Communication interfaces include MAVLink for
Pixhawk–Raspberry Pi integration and CAN bus reserved for future sensor expansion. This
design meets all specifications and constraints by ensuring waterproofing, thermal
stability, and failsafe mechanisms per ArduPilot standards. [7]

##  Interface with Other Subsystems

•  Power Source
•  Method: Two lithium batteries connected to PM02 Power Module, which regulates

voltage and monitors current before supplying power to Pixhawk 6C. [1]

•  Nature of Data: No data transfer; provides stable DC power and current sensing for

battery health.

•  Pixhawk 6C Autopilot [1]
•  Role: Core navigation controller running ArduPilot firmware. [2]
•  Connections:

o  Receives GNSS and IMU sensor data for position and attitude control.
o  Outputs PWM signals to thrusters and servos for motion control.

•  Companion Computer (Raspberry Pi 4) [5]

•  Method: Communicates with Pixhawk using MAVLink protocol over UART via

TELEM1 port (TX, RX, GND).  [4]

o  Pixhawk TX → Pi RX (GPIO15, Pin 10)
o  Pixhawk RX → Pi TX (GPIO14, Pin 8)
o  Pixhawk GND → Pi GND (Pin 6)

•  Nature of Data: Mission commands, telemetry, and system health.
•  Transducer (Blue Robotics Ping 1D) [3]
•  Method: Connected to Pixhawk via UART.
•  Nature of Data: Depth measurements and sonar readings.
•  Communication Interfaces
•  MAVLink: Primary protocol for Pixhawk–Raspberry Pi integration. [4]
•  CAN Bus: Reserved for future sensors or actuators.

##  Buildable Schematic
 
<img width="558" height="530" alt="figure1" src="https://github.com/user-attachments/assets/9c49d84c-c52d-4f4f-bda9-d9b8c0515766" />

Figure 1 Pixhawk datasheet


<img width="475" height="290" alt="figure2" src="https://github.com/user-attachments/assets/c648a251-0aba-48d1-adf4-1c2f6e378127" />


Figure 2 Schematic

Flowchart

<img width="462" height="296" alt="figure3" src="https://github.com/user-attachments/assets/f9c9ddf2-ac07-4af3-88c1-8541454e6a6b" />


Figure 3 Operational Flowchart

##  BOM

Item – Pixhawk 6C Autopilot Controller w/ PM02 
 Part Number – SKU 20179 
 Distributor – Holybro 
 Distributor Part Number – SKU 20179 
 Quantity – 1 
 Price ($) – $169.98 
 Link – Pixhawk 6C – Holybro Store (https://holybro.com/products/pixhawk-6c?variant=42783569903805) 

##  Analysis

The proposed navigation subsystem meets all functional requirements and constraints for
an autonomous lake-mapping vessel. The integration of the Pixhawk 6C autopilot
controller with ArduPilot firmware ensures robust real-time navigation and precise motion
control. ArduPilot is a proven open-source platform widely used in autonomous systems,
offering advanced features such as waypoint navigation, failsafe modes, and sensor fusion
for accurate positioning. [7]

Performance and Reliability

•  Sensor Integration: The Pixhawk 6C incorporates high-performance IMUs,

magnetometer, and barometer, enabling accurate attitude and heading estimation.
Combined with GNSS input, the system achieves precise geolocation necessary for
mapping operations. [1]

•  Communication: The use of MAVLink protocol between the Pixhawk and Raspberry

Pi ensures reliable, structured data exchange for mission commands and
telemetry. MAVLink is lightweight, widely supported, and optimized for real-time
applications. [4]

•  Failsafe Mechanisms: ArduPilot’s built-in failsafe modes prevent uncontrolled

movement in case of sensor or communication failure, satisfying safety constraints.
[7]

Power and Environmental Considerations

•  The PM02 Power Module regulates voltage and monitors current from two lithium

batteries, ensuring safe power delivery and battery health monitoring. [1]
•  The subsystem is rated for -40 °C to 85 °C, and additional waterproofing and
thermal management will ensure reliable operation in lake environments.

Scalability and Future Expansion

•  The inclusion of CAN bus allows for future integration of additional sensors or

actuators requiring high reliability. [6]

•  The Raspberry Pi companion computer provides flexibility for advanced processing

tasks such as adaptive route planning and sonar data analysis.

Cost-Effectiveness

•  Pixhawk 6C was selected for its balance of affordability and performance compared
to industrial-grade alternatives. Its open-source compatibility reduces software
licensing costs and enables customization. [1]

Mission Adaptability

•  The system supports dynamic mission updates via MAVLink, allowing real-time re-

tasking based on sonar feedback. This capability is critical for efficient lake
mapping and obstacle avoidance. [4]

Conclusion: The navigation subsystem design meets all technical specifications,
environmental constraints, and safety requirements. It provides a robust, scalable, and
cost-effective solution for autonomous lake mapping, leveraging proven hardware and
open-source software to ensure reliability and adaptability.

##  References

[1] Holybro, “Pixhawk 6C Technical Specification,” Holybro Documentation. [Online].
Available: https://docs.holybro.com/autopilot/pixhawk-6c/technical-specification.
[Accessed: Dec. 1, 2025].

[2] ArduPilot, “Mission Planner Overview,” ArduPilot Documentation. [Online]. Available:
https://ardupilot.org/planner/docs/mission-planner-overview.html. [Accessed: Dec. 1,
2025].

[3] Blue Robotics, “Ping Sonar Technical Guide,” Blue Robotics Learn. [Online]. Available:
https://bluerobotics.com/learn/ping-sonar-technical-guide/. [Accessed: Dec. 1, 2025].

[4] MAVLink, “Protocol Overview,” MAVLink Documentation. [Online]. Available:
https://mavlink.io/en/about/overview. [Accessed: Dec. 1, 2025].

[5] Raspberry Pi Foundation, “Raspberry Pi 4 Model B Specifications,” Raspberry Pi.
[Online]. Available: https://www.raspberrypi.com/products/raspberry-pi-4-model-
b/specifications/. [Accessed: Dec. 1, 2025].

[6] ThinkRobotics, “Using CAN Bus in Robotics Projects – A Comprehensive Guide,”
ThinkRobotics Blog. [Online]. Available: https://thinkrobotics.com/blogs/learn/using-can-
bus-in-robotics-projects-a-comprehensive-guide. [Accessed: Dec. 1, 2025].

[7] ArduPilot, “Failsafe – Copter Documentation,” ArduPilot Documentation. [Online].
Available: https://ardupilot.org/copter/docs/failsafe-landing-page.html. [Accessed: Dec.
1, 2025]. [2]

[8] Holybro, “Pixhawk 6C – Holybro Store,” Holybro. [Online]. Available:
https://holybro.com/products/pixhawk-6c?variant=42783569903805. [Accessed: Dec. 1,
2025]. [1]

[9] Holybro, “System Diagram & Pinout,” Holybro Documentation. [Online]. Available:
https://docs.holybro.com/autopilot/pixhawk-6c/system-diagram-and-pinout. [Accessed:
Dec. 1, 2025].




> 图片内容识别：[LLM 返回结果为空]
