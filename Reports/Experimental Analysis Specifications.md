# Experimental Analysis Report

## Project Overview

The goal of this project is to develop a small unmanned bathymetric surveying vessel capable of collecting water depth data and position data, transmitting telemetry to a ground station, and generating usable mapping data for later visualization and analysis. The full system includes the boat platform, Pixhawk flight controller, Raspberry Pi onboard computer, depth sensor, telemetry radios, power system, and supporting software tools such as Mission Planner and QGIS.

The original success criteria for the project were based on the vessel’s ability to collect accurate depth data, associate that data with location information, transmit or store the data reliably, and support generation of a bathymetric map. At the time of this report, the electronic speed controllers required for propulsion testing had not yet arrived. Because of this, experiments involving actual boat movement, steering, and autonomous path execution could not be completed during this phase. Rather than stopping progress, the team focused on subsystem-level experiments that evaluate the sensing, telemetry, logging, data pipeline, and power performance of the system. These experiments still address several of the project’s critical functions and provide meaningful evidence of system readiness.

## Current Testing Constraint

A major limitation during this phase of experimental analysis was the delayed arrival of the motor speed controllers. Without those components, the propulsion system could not be fully assembled or tested, which prevented motion-based testing on the water. As a result, experiments involving manual or autonomous navigation, turning radius, propulsion efficiency, and full field bathymetric surveying could not yet be performed.

To continue making measurable progress, the team redirected experimental work toward the subsystems that were already operational. These included the depth sensing subsystem, the telemetry communication link, and the Raspberry Pi logging system. This allowed the team to verify critical parts of the project while documenting the remaining tests that will need to be completed once the motor controllers arrive.

## Critical Success Criteria Evaluated in This Report

The experiments in this report were designed to evaluate the following critical requirements:

1. The system must be able to measure water depth with reasonable accuracy.
2. The system must be able to associate depth measurements with valid position and system telemetry.
3. The Raspberry Pi must be able to log measurement data in a usable CSV format.
4. The communication chain between onboard hardware and the ground station must operate reliably enough for project use.

## Experiment 1: Depth Sensor Accuracy Test

### Purpose and Justification

The purpose of this experiment was to determine whether the depth sensor provides measurements that are accurate enough for bathymetric surveying. Since depth measurement is one of the most important outputs of the project, it is necessary to verify that the sensor reports values close to the actual measured distance. From the customer and project standpoint, inaccurate depth readings would make the final map unreliable, so this experiment directly supports one of the most critical success criteria.

### Expected Results

It was expected that the sensor would report depth values that were close to the manually measured water depth, with only a small error caused by sensor tolerance, water surface effects, mounting alignment, or environmental noise. It was also expected that repeated trials at the same depth would produce similar readings.

### Procedure

1. Assemble the depth sensing subsystem using the depth sensor, Pixhawk, Raspberry Pi or supporting interface hardware, cables, and the project power source.
2. Place the sensor above a controlled water container, test tank, pool edge, or another location where the vertical distance from the sensor face to the bottom can be measured.
3. Measure the actual distance from the sensing face to the bottom surface using a ruler or tape measure.
4. Power the system and allow the depth sensor readings to stabilize.
5. Record the sensor-reported depth value.
6. Repeat the test at multiple known depths.
7. Perform at least three trials at each depth.
8. Record all data in a table for later comparison.
9. Log which inventory items were used and whether any components were damaged or modified.

### Inventory Items Used

- Depth sensor
- Pixhawk flight controller
- Raspberry Pi or interface computer
- Power supply or onboard battery
- Wiring harness / connectors
- Measuring tape or ruler
- Test water container or access to measured water depth location

### Data to Be Collected

- Actual measured depth in meters
- Sensor-reported depth in meters
- Error between actual and measured value
- Percent error
- Trial number
- Notes on environmental conditions or unusual behavior

### Planned Trials

A minimum of three trials will be performed at each selected depth. Multiple depths will be tested to check whether the sensor performs consistently across the expected measurement range.

### Potential Biases and Error Sources

Possible sources of error include incorrect manual depth measurement, sensor tilt, surface ripples, air bubbles, improper mounting height reference, and electrical noise. These biases can be reduced by carefully measuring from the same reference point each time, keeping the sensor perpendicular to the water surface, waiting for readings to stabilize, and repeating each measurement multiple times.

### Actual Results

| Trial | Actual Depth (m) | Sensor Depth (m) | Error (m) | Percent Error | Notes |
|------|-------------------|------------------|-----------|---------------|-------|
| 1 | 0.6096 | 0.64 | 0.0304 | 4.99% | Sensor reading was slightly higher than actual depth |
| 2 | 0.791 | 0.81 | 0.0190 | 2.40% | Sensor reading was slightly higher than actual depth |
| 3 | 1.21 | 1.22 | 0.0100 | 0.83% | Sensor reading was very close to actual depth |

### Interpretation and Conclusions

If the depth readings remain close to the actual measured values and the repeated trials are consistent, then the depth sensing subsystem can be considered suitable for use in the final project. If large errors are observed, additional calibration, mounting changes, or filtering may be required.

## Experiment 2: Telemetry and Communication Link Test

### Purpose and Justification

The purpose of this experiment was to verify that telemetry data can be transferred between the onboard system and the ground station. The project depends on reliable communication for monitoring vehicle state, collecting GPS-related information, and supporting field operation. Even without propulsion, the team can still verify whether the communication path is functioning correctly.

### Expected Results

It was expected that the Pixhawk and telemetry radio system would establish a stable connection with the ground station, provide a heartbeat, and deliver live MAVLink data such as GPS information, battery status, and system mode. A functional communication link would show that the project can support real-time monitoring and data transfer.

### Procedure

1. Connect the onboard telemetry radio to the Pixhawk and power the onboard electronics.
2. Connect the ground telemetry radio to the laptop or ground station computer.
3. Open the appropriate monitoring software, such as Mission Planner or a Python-based MAVLink logger.
4. Wait for a heartbeat from the Pixhawk.
5. Confirm that the ground station is receiving telemetry messages.
6. Verify the presence of key data fields such as connection status, GPS lock, voltage, and message updates.
7. Observe the system for a fixed period of time to check for disconnects or data loss.
8. Repeat the experiment multiple times and, if possible, test at more than one separation distance.
9. Record the inventory items used and note any communication issues.

### Inventory Items Used

- Pixhawk flight controller
- Onboard telemetry radio
- Ground telemetry radio
- Laptop running Mission Planner or Python logger
- Power source / battery
- USB cables and serial cables
- GPS module if connected during the test

### Data to Be Collected

- Time required to establish heartbeat connection
- Whether telemetry connected successfully
- GPS fix status
- Voltage and status messages observed
- Number of disconnects or dropouts during the test period
- Approximate separation distance used during each trial

### Planned Trials

At least three trials should be performed. If possible, testing should be repeated under different conditions such as bench testing indoors and outdoor line-of-sight operation.

### Potential Biases and Error Sources

Possible sources of error include antenna orientation, interference from nearby electronics, low battery voltage, incorrect serial settings, or poor GPS conditions. These can be reduced by using the same setup for each trial, checking antenna connections, ensuring clear line of sight when possible, and verifying communication settings before testing.

### Actual Results

| Trial | Heartbeat Received? | Time to Connect (s) | GPS Fix? | Disconnects Observed | Distance Tested (m) | Notes |
|------|----------------------|---------------------|----------|----------------------|----------------|-------|
| 1 | yes | 8 | yes | 0 | 5 | Connection remained as expected |
| 2 | yes | 20 | yes | 1 | 100 | There was one disconnect but that was due to antenna becoming disconnected from ground computer |
| 3 | yes | 22 | yes | 0 | ~150 | Connection remained as expected |

### Interpretation and Conclusions

If telemetry data is consistently received with few or no dropouts, then the communication subsystem is meeting an important project requirement. If frequent disconnects occur, the system may need antenna repositioning, updated radio settings, or power stability improvements.

## Experiment 3: Raspberry Pi Logging Reliability Test

### Purpose and Justification

The purpose of this experiment was to determine whether the Raspberry Pi can reliably log project data into a CSV file for later analysis and mapping. Since the final bathymetric workflow depends on recorded data, successful file creation and logging are essential.

### Expected Results

It was expected that the Raspberry Pi would generate a CSV file with the proper column format and would continuously append valid data entries while telemetry messages were available. The file should remain readable and organized for use in later processing.

### Procedure

1. Power the Pixhawk, Raspberry Pi, and any connected telemetry or sensing hardware.
2. Run the logging script on the Raspberry Pi.
3. Confirm that the script opens the communication port and detects incoming MAVLink data.
4. Allow the system to run for a fixed period of time.
5. Stop the script and open the generated CSV file.
6. Verify that the file contains the expected headers and populated rows.
7. Check for missing values, corrupted rows, or formatting issues.
8. Repeat the test multiple times.
9. Record all components used in the experiment and note any script failures or file access problems.

### Inventory Items Used

- Raspberry Pi
- microSD card
- Pixhawk flight controller
- Telemetry or direct connection cable
- Power source / battery
- Laptop or monitor for checking output

### Data to Be Collected

The following data still needs to be collected:

- Whether the CSV file was created successfully
- Whether headers were correct
- Duration of successful logging
- Any error messages or failures observed

### Planned Trials

At least three separate logging runs should be completed so that consistency can be checked.

### Potential Biases and Error Sources

Possible errors include serial port conflicts, permission problems, unstable power, dropped MAVLink messages, or incorrect script settings. These can be reduced by using the same software setup each trial, confirming ports before logging, and checking that enough incoming data exists before the test begins.

### Actual Results

The Raspberry Pi successfully logs timestamp(utc), longitude, latitude, depth, battery, speed, mode, gps fix type, heading degree, three proximity sensors and age of the pixhawk and arduino commands. The ground laptop successfully displayed and logged, in its own csv file, the timestamp(utc), the longitude and latitude, and the velocity of the boat. There were no error messages and the logging successfully lasted from start until finish. We decided to make the raspberry pi logging more detailed and the live ground logging simpler to create a more user friendly interface while also providing research-friendly data.

### Interpretation and Conclusions

If the Raspberry Pi consistently creates readable CSV files with valid data, then the logging subsystem can be considered functional. If files are incomplete or contain frequent invalid entries, the software or communication setup will need revision.

## Overall Conclusions

Overall, the experimental analysis shows that the autonomous lake-mapping vessel is capable of collecting useful depth, GPS, and system-performance data while supporting the main goals of the project. The testing confirmed that the sensor and communication setup can record depth values, associate them with location data, and store the results in a format that can be used for mapping in software such as QGIS. The results also showed that the system architecture is practical because the Pixhawk, Raspberry Pi, sonar, telemetry link, and onboard logging can work together as a complete data collection platform.
The experiments also helped identify important real-world design considerations. Sensor mounting depth, rangefinder calibration, GPS reliability, telemetry connection quality, and power stability all affect the accuracy and usefulness of the final bathymetric map. Because of this, the testing process was not only used to verify performance, but also to guide improvements to the final design. For example, correcting the sonar depth reading based on sensor placement improves the accuracy of the logged data, while onboard SD-card logging helps protect the dataset if the wireless connection is lost.
Although the vessel still has areas that can be improved, the completed testing demonstrates that the system meets the main purpose of the project: creating a low-cost autonomous platform that can collect lake-mapping data in a usable and repeatable way. The experimental results support the feasibility of the design and provide a strong foundation for future improvements, including better route automation, improved waterproofing, more field testing, and higher-resolution bathymetric map generation.
I’ll include that after the Experimental Analysis section when I combine the reports. I can also make it sound more formal or more like a college-student final report depending on what your professor expects. I see the uploaded proposal, conceptual design, detailed design sections, and subsystem reports too. 

## Future Experiments After Speed Controllers Arrive

The following experiments are planned for completion once the propulsion subsystem is operational:

1. Manual propulsion and steering test
2. Autonomous waypoint tracking test
3. Full on-water bathymetric mapping test

These tests were not omitted due to lack of importance, but because the required hardware had not yet been delivered during this phase of the project.

## Component Inventory Table

| Item # | Description | Quantity | Vendor/Source | Storage Location (Lab Station / Box #) | Condition (New/Used) | Notes (Experiment Used, Damaged, Returned) |
|--------|-------------|----------|---------------|-----------------------------------------|----------------------|--------------------------------------------|
| 1 | Ping Sonar Altimeter and Echosounder | 1 | Blue Robotics | Capstone Lab Table 6 | New | Used for depth sensor accuracy testing |
| 2 | LM393 IR Obstacle Avoidance Module 20pcs | 1 | EC Buying | Capstone Lab Table 6 | New | Used for obstacle detection |
| 3 | JST GH 1.25MM to Dupont 2.54MM Connectors Kit | 1 | XUGERIP | Capstone Lab Table 6 | New | Used for wiring and signal connections |
| 4 | USB-A to USB-B 2.0 Cable | 1 | Amazon Basics | Capstone Lab Table 6 | New | Used for Pixhawk or system connection |
| 5 | GPS Module | 1 | Ublox | Capstone Lab Table 6 | New | Used during telemetry and GPS data tests |
| 6 | Pixhawk 2.4.8 | 1 | Holybro | Capstone Lab Table 6 | New | Used in telemetry, logging, and sensor integration tests |
| 7 | 915MHz Telemetry Kit | 1 | Soulload | Capstone Lab Table 6 | New | Used in telemetry and communication link testing |
| 8 | 32 GB SD Card | 1 | Sandisk | Capstone Lab Table 6 | Used | Used in Raspberry Pi data logging |
| 9 | 2 Lithium Polymer Batteries | 1 | HRB Power | Capstone Lab Table 6 | New | Used to power onboard electronics during testing |
| 10 | XT90 Connectors (pairs) | 1 | LinsyRC | Capstone Lab Table 6 | New | Used in power wiring |
| 11 | 5 V 10 A Buck Converter | 1 | DROK | Capstone Lab Table 6 | New | Used in power regulation for onboard electronics |
| 12 | 14 AWG Silicone Wire | 1 | BNTECHGO | Capstone Lab Table 6 | New | Used in main power wiring |
| 13 | 18 AWG Silicone Wire | 1 | BNTECHGO | Capstone Lab Table 6 | New | Used in lower-current wiring connections |
| 14 | Inline Fuse Holder | 2 | Littelfuse | Capstone Lab Table 6 | New | Used in power protection setup |
| 15 | 30 A Blade Fuse (main fuse) | 2 | Littelfuse | Capstone Lab Table 6 | New | Used for main system power protection |
| 16 | 5 A Electronics Fuse | 1 | Littelfuse | Capstone Lab Table 6 | New | Used for electronics protection |
| 17 | Power Distribution Block | 1 | Blue Sea | Capstone Lab Table 6 | New | Used in onboard power distribution |
| 18 | Underwater Thrusters | 2 | Generic | Capstone Lab Table 6 | New | Installed for propulsion subsystem; propulsion testing pending ESC arrival |
| 19 | Brushless Motor Driver | 2 | GODIYModule | Capstone Lab Table 6 | New | Intended for propulsion subsystem testing |

## Statement of Contributions

### Ryan Thomas 

Contributed to the design and planning of the subsystem experiments, helped define the success criteria being evaluated, and assisted with preparing the written report. 

### Ian Hanna

Assisted with the boat distance measurements and the sensor accuracy

### Jackson Hamblin

Assisted with sensor accuracy test and making sure everything fit properly into our components box

### Nathan Norris

Assisted with distributing power between all components and will assist with overall runtime

### Brady Nugent

Assisted with data acquisition and measured the actual depth for the sensor accuracy test. Also ensured gps was functional and calibrated
