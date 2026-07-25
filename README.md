# Lake Mapping Drone

An autonomous surface vessel developed by Tennessee Technological University
ECE Capstone Team 3 to collect depth, GPS, proximity, and vehicle-status data
for low-cost bathymetric mapping.

## Project overview

Commercial hydrographic survey platforms can be prohibitively expensive for
small research groups and local organizations. This project explores a modular
alternative built around a catamaran-style hull, a Pixhawk flight controller,
a Raspberry Pi, a Blue Robotics Ping1D sonar, proximity sensors, and a MAVLink
telemetry link.

The system is designed to associate depth measurements with position and time,
store a detailed onboard record, and transmit a simplified live data stream to
a shore-based ground station.

## System capabilities

- Measures water depth with a Ping1D single-beam sonar
- Associates depth readings with Pixhawk GPS and vehicle telemetry
- Logs timestamped mission data on a Raspberry Pi in CSV format
- Transmits live position and velocity information to a ground station
- Monitors three short-range proximity sensors
- Supports ArduPilot/MAVLink integration and downstream mapping in QGIS
- Preserves onboard data when the wireless link is interrupted

## Validation results

Subsystem testing produced the following results:

- **Depth sensing:** three controlled measurements showed absolute errors of
  0.0100-0.0304 m, corresponding to 0.83-4.99% error
- **Telemetry:** successful connections were demonstrated at approximately
  5 m, 100 m, and 150 m; one observed interruption was traced to a disconnected
  ground-station antenna
- **Data logging:** the Raspberry Pi recorded UTC time, position, depth,
  battery state, speed, operating mode, GPS fix, heading, proximity states,
  and message-age information without a reported logging failure

The delayed arrival of propulsion speed controllers prevented full motion,
autonomous waypoint, and on-water bathymetric survey testing during the
documented test phase. Those items remain future validation work and are not
presented as completed capabilities.

See the
[Experimental Analysis Report](Reports/Experimental%20Analysis%20Specifications.md)
for procedures, measurements, limitations, and conclusions.

## System architecture

The project is organized into five primary subsystems:

1. **Sensors and data acquisition** - sonar, proximity sensing, time-aligned
   data collection, and verification planning
2. **Navigation** - Pixhawk, GPS, ArduPilot configuration, and mission control
3. **Communication** - Raspberry Pi processing, MAVLink telemetry, onboard
   logging, and ground-station data
4. **Power** - battery protection, regulation, distribution, and monitoring
5. **Mechanical hardware** - catamaran hull, payload integration, waterproofing,
   and component mounting

## Team

| Team member | Primary responsibility |
| --- | --- |
| Ryan Thomas | Team coordination and communication subsystem |
| Ian Hanna | Sensors and data-acquisition subsystem and hardware block diagram |
| Brady Nugent | Navigation subsystem and operational flow |
| Nathan Norris | Power subsystem; team recorder and meeting-minutes coordinator |
| Jackson Hamblin | Mechanical hardware and comparative analysis |

**Faculty supervisor:** Dr. Christopher Johnson

**Project stakeholder:** Dr. Kalyanapu

## Repository guide

- [Project proposal](Reports/Project%20Proposal.md)
- [Conceptual design](Reports/Conceptual%20Design.md)
- [Final report](Reports/Final%20Report.md)
- [Sensors and data-acquisition design](Reports/Sensors%20Subsystem.md)
- [Experimental analysis](Reports/Experimental%20Analysis%20Specifications.md)
- [Meeting minutes](Documentation/Meeting%20Minutes)
- [Poster](Reports/Poster%20Template/poster.pdf)

The `Reports` directory contains the major design and validation documents.
`Documentation` contains subsystem references and meeting records retained for
project traceability.

## Project status

The sensing, telemetry, and logging chain was validated at the subsystem level.
Full propulsion, autonomous navigation, and end-to-end on-water mapping remain
outside the completed test scope documented in this repository.
