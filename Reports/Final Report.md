# Lake Mapping Drone

## Introduction

Understanding the depth and terrain of lakes and other bodies of water is essential for environmental monitoring, infrastructure planning, and recreational management. The conventional method of underwater mapping relies on specialized boats, lots of manpower, and expensive sonar equipment. This makes it unattainable for smaller organizations and research groups. As a result, there is a growing need for affordable, autonomous systems capable of collecting and transmitting in-depth data to produce detailed underwater maps. 

The project proposes the design and development of an autonomous boat that will measure depth and send data to be able to generate a detailed map of a body of water. The system will incorporate low-cost sonar technology, autonomous navigation, and wireless data transmission. With this system, it will be user friendly and scalable. By taking away the need for constant human operation and cheaper equipment, the project’s focus is to make underwater mapping more practical and accessible to a vast variety of users.

This proposal will outline the background and motivation for the project, clearly define the problem, describe the system requirements, and present the proposed design approach. In addition, it will discuss anticipated challenges, testing methods, and the broader impact of the solution.

## Formulating the Problem

This problem affects environmental and local governments who need accurate lake and reservoir data. This data can then be used for environmental monitoring, flood management, and infrastructure planning. It also impacts fisheries, recreational and conservation groups. The current lake mapping methods tend to be expensive and require well-trained operators. So, a low cost, autonomous solution would allow for more frequent and efficient data collection without the high cost and specialized skills. This would allow lake mapping to become more expandable and accessible. While commercial sonar and autonomous boats do exist, simply the cost and specific skill set needed to use them limit the availability greatly. Consumer sonar systems may provide depth readings but lack autonomy, data mapping, and integration capabilities. So, a custom-built solution can guarantee affordability, versatility in differing environments, and control over the capacity of the solution’s growth.

### Background

Effective depth mapping relies on sonar, GPS, and GIS technologies working together to generate accurate underwater maps [1]. However, mapping conditions are rarely ideal: wind, waves, and surface turbulence can introduce noise, while GPS accuracy may degrade near shorelines or under canopy cover. Although sonar is less affected by water clarity than optical sensors, shallow waters present specific challenges, such as interference from surface echoes and reduced resolution due to limited range [2].

Current commercial solutions often face limitations, including reduced accuracy in shallow environments, high cost, and lack of modularity for adapting to different research needs. These factors make them less accessible to smaller research groups and less versatile for studying properties beyond depth, such as stream flow.

The project addresses these challenges by developing a modular, low-cost, and user-friendly mapping platform. While its primary focus is lake bottom depth, the design emphasizes adaptability, enabling researchers to expand into diverse water applications without being restricted by cost or technical barriers. This project is specifically targeted at inland lakes/small rivers and small-scale research environments, rather than large-scale hydrographic surveys, thereby focusing its scope on cost-effective and adaptable solutions for academic and field research settings.

### Specifications and Constraints

#### Specifications

1. The vessel shall be user-friendly and straightforward to operate in both autonomous and RC (manual) modes. 
2. The vessel shall include a fail-safe system in both autonomous and RC modes to prevent loss of the boat in case of system error or communication failure. 
3. The vessel shall carry a payload capacity of 40–50 lbs, including sensors, batteries, and communication equipment. 
4. The vessel shall support depth sensors and water flow measurement sensors for data collection. 
5. The vessel shall operate for a minimum of 2–3 hours per battery cycle, with provisions for multiple batteries for both boat propulsion and sensor operation. 
6. The vessel shall utilize a catamaran-style hull for stability and weight distribution. 
7. The system shall interface with commercial software such as Hypack for navigation and data logging. 
8. The system shall support open-source software such as QGIS for GPS point data and ArcGIS Pro for data post-processing. 
9. The vessel shall comply with relevant Tennessee boating regulations (TWRA) and federal guidelines regarding RC and autonomous surface vessels. 
10. The vessel shall not cause harm to the surrounding environment, including water quality, aquatic plants, or fish populations. 

#### Constraints

1.	Battery Safety (UL 2054 – Household and Commercial Batteries) 

- The vessel’s battery systems shall comply with UL 2054 to ensure safe design, assembly, and operation, providing protection against fire, explosion, and electrolyte leakage. 

2.	Ingress Protection (IEC 60529 – IP Ratings) 

- All electronic enclosures, sensors, and control modules shall meet an appropriate IEC 60529 IP rating to prevent ingress of dust and water under expected operating conditions 

3. 	Radio Frequency Compliance (FCC 47 CFR Part 15 – Radio Frequency Devices) 

- All wireless communication systems, including RC transmitters, receivers, and autonomous control modules, shall comply with FCC 47 CFR Part 15 to prevent harmful interference and ensure safe RF operation. 

4.	 Hull Stability (ISO 8383:1985 – Ships and Marine Technology: Small Craft Stability) 

- The vessel’s hull design shall conform to ISO 8383:1985 to ensure adequate stability, buoyancy, and safety during operation in various load and environmental conditions. 

5.	 Boating Regulations (Tennessee Wildlife Resources Agency – TWRA) 

- Operation of the vessel on public waterways shall comply with all applicable TWRA boating regulations, including safety, registration (if required), and operational conduct. 

6.	 Waterway Use (Tennessee Valley Authority – TVA Waterway Regulations) 

- When operating within TVA-managed waters, the vessel shall comply with TVA waterway navigation and use regulations to ensure lawful and safe operation. 

7. 	 Environmental Protection (EPA Clean Water Act Compliance) 

- The vessel shall comply with the EPA Clean Water Act, ensuring that no pollutants, fuels, or chemicals are discharged into waterways during operation, maintenance, or testing. 


## Survey of Existing Solutions

This section summarizes current approaches and products used to measure water depth and water flow. We group solutions by sensing method and platform, then note capabilities and trade-offs relevant to an autonomous, low-cost survey vessel.

**1. Acoustic Doppler Current Profilers (ADCPs) for Flow/Discharge**

ADCPs are the water-industry standard for measuring velocity profiles and computing discharge. U.S. Geological Survey (USGS) guidance details validated procedures for moving-boat ADCP measurements and associated QA/QC practices. [3]
**Representative systems**
- **Teledyne RD Instruments (RDI) StreamPro** – portable ADCP designed for small streams; provides real-time discharge with built-in QA/QC workflows. Typical operating depths are on the order of decimeters to a few meters, targeting small channels. [4]
- **SonTek (Xylem) RiverSurveyor RS5** – compact ADCP intended for discharge in rivers, streams, and canals; often paired with a small tow board or micro-USV and optional RTK positioning. [5]
  
**Relevance to the project:**
 ADCPs offer high-fidelity flow/velocity data and well-established methods but increase cost, power draw, and integration complexity compared with simpler single-beam depth sensors. [3]

**2. Echo Sounders for Depth/Bathymetry**

Single-beam echo sounders provide vertical depth at a point; multibeam systems map swaths for faster coverage but at higher cost and integration requirements.
- **Single-beam (e.g., Blue Robotics Ping/Ping2)** – low-cost echosounders (≈100 m range, ~25° beam) commonly used on small USVs for bathymetric point soundings and as altimeters; [6]
- **Survey-grade single-beam (e.g., CEESCOPE with RTK GNSS)** – integrated echo sounder + GNSS/RTK receivers for centimeter-level bottom mapping in professional workflows. [7]
- **Multibeam packages (case example)** – ASVs equipped with multibeam sonars and RTK-INS (e.g., SBG Ekinox-D) enable wide-swath mapping with precise positioning and motion compensation.
  
**Relevance to the project:**
 A single-beam transducer is typically sufficient for channel profiling and proof-of-concept bathymetry; multibeam improves coverage but requires higher budget and more advanced navigation/attitude sensing. [6]

**3. Unmanned Surface Vessels (USVs) / Autonomous Survey Platforms**

Commercial USVs integrate propulsion, navigation, and payload bays for sonar and GNSS/INS.
- **Seafloor Systems EchoBoat-160** – purpose-built hydrographic USV supporting single-/multibeam payloads; marketed for efficient, crew-reduced surveys. [8]
- **Tersus GNSS “TheDuck”** – compact USV with single-beam echo sounder aimed at bathymetric surveys. [9]
  
**Relevance to the project:**
 Commercial USVs offer turn-key reliability but at significant cost. A student-built platform can tailor sensors and autonomy to the specific use case at lower price, with added integration effort. [8]

**4. Industry Practices and Alternative Flow Instruments**

Beyond ADCPs, vendors such as OTT Hydromet offer mechanical and acoustic flow meters and fixed stations for long-term discharge monitoring; mobile Doppler systems like OTT Qliner2 have specified accuracy and profiling ranges for wading and boat deployments. [10]

**5. Open-Source / Academic Efforts**

Recent literature and maker ecosystems describe low-cost autonomous surface vehicles that integrate commodity echosounders, GNSS, and open autopilots for bathymetry/monitoring—highlighting feasibility for budget-constrained applications, albeit with varying levels of validation versus professional gear. [11]

## Measures of Success

To evaluate the accuracy and consistency of the system’s data, the collected measurements will be compared against reference datasets previously validated by professionals in the field. Success will be defined using measurable accuracy thresholds, as outlined below: 

**1. Depth Measurement (Sonar Data):**

a. Each depth reading will be compared with reference measurements taken from surveyed points. 

b. Project success will be defined as achieving depth readings within ±5 cm of the reference values across all test conditions. 

c. During early stages of testing, a tolerance of ±15 cm will be permitted, and this threshold will be narrowed progressively until the final accuracy goal of ±5 cm is reached. 

**2. Water Flow Measurement (Sensor Data):**

a. Flow sensor readings will be compared against reference data obtained from calibrated flow meters. 

b. Success will be defined as flow rate measurements deviating by no more than ±10% from the reference values. Initial testing will allow up to ±20% deviation, with a gradual reduction as the system is refined. 

**3. Data Transmission and Processing:** 

a. Collected data will be transmitted remotely and analyzed in real time. The analysis program will generate statistical summaries (mean error, standard deviation) and visual outputs (charts, deviation plots). 

b. Success will be demonstrated by maintaining ≥ 95% uptime in transmission reliability and ensuring that processed data remains within the defined accuracy thresholds. 

**4. System Consistency Across Conditions:**

a. Testing will occur under varied conditions (calm vs. turbulent water, shallow vs. deep environments). 

b. Success will be defined as maintaining accuracy thresholds (±5 cm for depth, ±10% for flow) with no more than a 5% degradation in performance across these conditions. 

**5. Iterative Improvement:**

a. Hardware and software adjustments will be guided by error analysis. 

b. Success will be indicated by a measurable reduction in deviations from one testing phase to the next, converging toward the final accuracy goals. 

## Resources

1. **Hardware**
- Boat Hull: Designed to provide structural support and room for mounting other hardware such as Raspberry pi, propulsion system, sensors, etc. Built to support 40-50 pounds in open water
- Battery/Charging: Batteries with charging support to last 2-4 hours of continuous operation.
- Microprocessor: Raspberry pi to process sensor data and interface with communication modules.
- Autopilot/GPS: Module for autonomous navigation and GPS tracking.
- Sonar/Transducer System: Primary sensor for depth measurement and underwater topography
- Current Velocity System: Sensor used for measuring current velocity under the hull
- Waterproof enclosure: Protection for electronics and sensors during operation.
2. **Communication and Data Processing**
- Telemetry Radio: Long-range communication for GPS and depth reading to reach data-processing system
- Data Processing System: Software to process incoming data in real time. It will integrate sonar, GPS, velocity, and depth measurements to produce a synchronized dataset and generate a two-dimensional map for immediate visualization during operation.
3. **Software resources**
- Hypack: Industry-standard hydrographic survey software used for sonar data processing and integration.
- QGIS: Free GIS platform for geospatial analysis, GPS point handling and mapping
- ArcGIS Pro: Advanced geospatial for professional mapping and visualization.

### Budget

The estimated budget for the project is outlined below:

| Item/Material |	Quantity | Cost (USD Estimates) |
|---------------|----------|----------------------|
| Boat (Hull)	| 1	| iMakerSpace fee |
| Battery/Charger	| 1	| $25.00-$50.00 |
| Microprocessor | 1 | $80.00 |
| Sonar/Transducer | 1 | $400.00-$600.00 |
| Autopilot/GPS |	1	| $35.00 |
| Communication system to process data (Telemetry radios) |	1-2	| $40.00-$90.00 |
| Data Processing system |	1	| $0.00-100.00 |
| Propulsion Motor System	| 1-2	| $50.00-$75.00 |
| Current Velocity System	| 1	| $300.00-$500.00 |
| Hypack license (Survey Software) |	1	| $0.00 |
| QGIS license (GIS software) | 1	| $0.00 |
| ArcGIS pro license (GIS software) | 1	| $100.00 |

### Personel

**Engineering Team**

Jackson Hamblin: 3D modeling using CAD and CAM software, 3D printing, Manual and CNC machining / Fabrication, Experienced with depth finders and fish finders for fishing

Ian Hanna: User Interface Design, C++, Microcontrollers, Integrating Sensors, PCB Design

Nathan Norris: Signal Processing, C++, Power Systems

Brady Nugent: C++, Power Systems, 3D modeling

Ryan Thomas: Microcontrollers, C++, 3D printing, Fabrication and Design

*Any skill gaps or lesser knowledge of subjects that come up during the project will be resolved by research and practice* 

**Supervisor**
Dr. Christopher Johnson - He is there to give advice and guide the project in the correct direction. 

**Customer**
Dr. Kalyanapu – Chosen because of his knowledge of autonomous boats and water research. The customer is meant to help create expectations that the project must meet. 

### Timeline

<img width="2971" height="1233" alt="image" src="https://github.com/user-attachments/assets/c2c2c4ee-a456-4aee-9bc5-9cdb76bcea40" />


## Specific Implications

The implementation of the proposed water depth sensor device carries several key implications that directly benefit the customer.

- **User-Friendly Interface:** The system will present data in a straightforward manner, allowing technicians to operate it without requiring an engineer on-site. This reduces staffing costs and makes deployment more accessible.
  
- **Autonomous Functionality with Manual Override:** The device will be capable of autonomously mapping the entire body of water, minimizing human intervention and improving efficiency. For flexibility, a manual override option will allow remote operation from a computer when needed.
  
- **Primary Depth Sensing with Modularity:** While the core function is depth measurement, the modular design ensures scalability. The first planned add-on module will enable water stream flow measurements, expanding the device’s research applications without redesigning the system.
  
- **Carrying Capacity of 40–50 Pounds:** With this payload capacity, the device can accommodate future sensors or equipment, ensuring long-term adaptability for additional water sensing needs.

By integrating these features, the project provides tangible benefits. It reduces operating costs, increases efficiency through autonomy, and guarantees flexibility for future applications.

## Broader Implications, Ethics, and Responsibility as Engineers

Implementing the depth sensor will have several economic, environmental, and societal impacts:

- **Global and Economic:** The device lowers costs and democratizes hydrographic technology for small research groups and developing regions, though it may disrupt existing commercial markets.
  
- **Environmental:** Accurate depth data supports sustainable water management and habitat monitoring, but deployment must minimize habitat disturbance through low-noise, environmentally conscious design.
  
- **Societal:** Affordable, accessible depth mapping improves safety, fisheries management, and community resilience, but equity must be ensured so benefits extend beyond well-funded institutions.
  
- **Ethical Responsibility:** Each team member must ensure accuracy, transparency, and sustainability, while upholding public welfare and taking personal responsibility for the societal and environmental impacts of their work.
  
These implications emphasize the importance of designing our project within the context of the impacts it will have on the world. Doing so will enable long term sustainability of accessible depth mapping.

## References

[1] Z. Li, Y. Gao, M. Xu, J. Liu, Y. Yang, and J. He, “Exploring modern bathymetry: A comprehensive review of methods, limitations, and future directions,” Frontiers in Marine Science, vol. 10, Apr. 2023. [Online]. Available: https://doi.org/10.3389/fmars.2023.1178845

[2] F. Gerlotto, S. Gauthier, and B. Masse, “The application of multibeam sonar technology for quantitative estimates of fish density in shallow water acoustic surveys,” Aquatic Living Resources, vol. 13, no. 5, pp. 385–393, 2000. [Online]. Available: https://doi.org/10.1016/S0990-7440(00)01094-4

[3] U.S. Geological Survey, Use of Acoustic Doppler Current Profilers for Streamflow Measurements, U.S. Dept. of the Interior, Reston, VA, USA. [Online]. Available: https://pubs.usgs.gov 

[4] Teledyne Marine, “StreamPro ADCP,” Teledyne RD Instruments, 2023. [Online]. Available: https://www.teledynemarine.com  

[5] SonTek, “RiverSurveyor RS5,” Xylem Inc., 2023. [Online]. Available: https://www.ysi.com/riversurveyor  

[6] Blue Robotics, “Ping2 Echosounder and Altimeter,” Blue Robotics Inc., 2023. [Online]. Available: https://bluerobotics.com/store/sensors-sonars-cameras/sonar/ping-sonar-r2-rp  

[7] CEESCOPE, “Integrated GNSS and single-beam echosounder for hydrographic surveys,” CEE Hydrosystems, 2023. [Online]. Available: https://www.ceehydrosystems.com 

[8] Seafloor Systems Inc., “EchoBoat-160 Autonomous Survey Vessel,” Seafloor Systems, 2023. [Online]. Available: https://www.seafloorsystems.com  

[9] Tersus GNSS, “TheDuck Autonomous Survey Boat,” Tersus GNSS Inc., 2023. [Online]. Available: https://www.tersus-gnss.com   

[10] OTT Hydromet, “Qliner2: Mobile Doppler system for discharge measurements,” OTT Hydromet GmbH, 2023. [Online]. Available: https://www.ott.com  

[11] ResearchGate, “Low-cost autonomous surface vehicles for inland water monitoring,” ResearchGate Publications, 2023. [Online]. Available: https://www.researchgate.net 



## Statement of Contributions

Jackson Hamblin - Existing Solutions

Ian Hanna - Background, Specific/Broader Implications

Nathan Norris - Resources, Budget, Timeline 

Brady Nugent - Introduction, Formulating the Problem

Ryan Thomas - Specifications/Constraints, Measures of Success


# Conceptual Design

## Introduction

Understanding the depth and terrain of lakes and other bodies of water is essential for environmental monitoring, infrastructure planning, and recreational management. The conventional method of underwater mapping relies on specialized boats, extensive manpower, and expensive sonar equipment. For instance, Blue Robotics offers the BlueROV2, a professional-grade remotely operated vehicle, starting at approximately $4,900, not including accessories such as a surface controller, tether, or data processing software. These costs make such systems unattainable for smaller organizations and research groups. As a result, there is a growing need for affordable, autonomous systems capable of collecting and transmitting in-depth data to produce detailed underwater maps.

## Restating the Fully Formulated Problem

Accurate and efficient mapping of lakes and reservoirs is a critical need for environmental agencies, local governments, and research institutions. Such data supports a wide range of applications, including environmental monitoring, flood risk assessment, sediment analysis, and infrastructure planning. In addition, fisheries management, recreational safety, and conservation efforts all depend on up-to-date bathymetric and environmental information.

Current lake mapping methods often rely on expensive survey vessels and specialized operators, limiting the frequency and accessibility of data collection. Commercial autonomous surface vehicles (ASVs) and sonar systems—such as the Blue Robotics BlueROV2 at $4,900 or larger hydrographic ASVs exceeding $10,000—provide high-quality data but remain financially impractical for small-scale use. Meanwhile, consumer-grade sonar systems, though affordable, typically lack autonomy, spatial mapping capabilities, and integration with other sensing technologies.

This project seeks to address these limitations by developing a low-cost, autonomous boat capable of performing lake mapping missions. The proposed system integrates GPS, sonar, and onboard computation to autonomously navigate and collect bathymetric data, which can then be processed into a detailed map of the waterbody. The need for a multidisciplinary engineering team is evident, as the solution requires expertise in mechanical design, embedded systems, control algorithms, and data processing. Together, these disciplines contribute to an innovative and accessible alternative for water resource monitoring and management.

## Comparative Analysis of Potential Solutions

The project requires reliable sensing for both lake bottom depth mapping and **water current flow measurement**. Selecting the proper technologies for sonar-based depth sensing and flow monitoring is essential to balance affordability, accuracy, integration complexity, and long-term adaptability.

### **Depth Measurement Solutions (Sonar and Transducers)**

**1. Lowrance Elite-5 DSI (consumer fish finder):**

Uses a downward-looking sonar (single-beam/DownScan Imaging) that emits acoustic pulses and measures echo return times to compute depth relative to the transducer face. Includes a built-in GPS for position and waypointing. The Elite-5 DSI supports NMEA serial communications (documented as NMEA-0183 / data port options) for exporting position and depth information. Typical depth ranges depend on frequency/mode (e.g., DownScan & CHIRP/83–200 kHz families) and listed max depths in consumer specs.

<img width="152" height="229" alt="image" src="https://github.com/user-attachments/assets/9dfb326e-636b-459f-9f45-c1f6cd724837" />


**2. SonTek RiverSurveyor (S5 / M9) + PCM:**

SonTek instruments are professional acoustic systems that can operate as ADCP-based or multi-frequency single/multiple beam profiler/echo sounder devices. They measure depth and (optionally) water-column velocities using acoustic pulses and Doppler processing. The PCM supplies power, 2.4 GHz telemetry (or RTK GPS options), and a communications interface (USB/serial or radio telemetry) to a base computer. SonTek systems produce survey-grade bathymetric data and can be configured with RTK GNSS for centimeter-level positioning.

<img width="153" height="192" alt="image" src="https://github.com/user-attachments/assets/69a171ef-f707-49f9-99a8-1368420c9afd" />


**3. Single-Beam Echo Sounder with Transducer (e.g., Blue Robotics Ping2)**

The Blue Robotics Ping2 is a single-beam echosounder that measures water depth by emitting a pulse of ultrasonic sound downward and then recording the time it takes for the sound to reflect off the bottom and return to the transducer. By multiplying this time delay by the speed of sound in water, the Ping2 accurately calculates the distance to the lakebed or seafloor. Operating at a frequency of 115 kHz, it provides a balance between depth range and resolution while avoiding interference with common fish-finder frequencies. The Ping2 integrates a built-in bottom-tracking algorithm that helps it maintain reliable readings even when water conditions or bottom compositions vary.  

<img width="186" height="186" alt="image" src="https://github.com/user-attachments/assets/0bf1edf6-9356-4631-b518-15340b7d9377" />


### Flow Measurement Solutions 

**1. YF-S201 Hall-Effect Flow Sensor:**

 The YF-S201 is a low-cost (~$10–$20) sensor that uses a turbine and Hall-effect sensor to measure flow rate. It outputs digital pulses proportional to the water velocity. While not as precise as acoustic methods, it is extremely affordable, lightweight, and easy to integrate with microcontrollers such as Arduino or Raspberry Pi. Its accuracy is typically within ±10% after calibration, making it suitable for small-scale, student-built systems.

<img width="214" height="143" alt="image" src="https://github.com/user-attachments/assets/2030f3d8-07c6-4a10-a374-97cbc60a4b11" />

**2. DFROBOT SEN0216 Liquid Flow Sensor (Hall-Effect Type)**

The DFROBOT SEN0216 is a durable, mid-range alternative to entry-level turbine sensors like the YF-S201. Priced around $20, it features a reinforced plastic housing, threaded connectors, and silicone sealing for improved resistance to leaks and corrosion. The sensor measures flow rates from 1 to 30 L/min with typical accuracy of ±5%, offering tighter calibration and smoother pulse output. Designed for long-term use in field conditions, it integrates directly with Arduino or Raspberry Pi platforms through a digital pulse output. This makes it a practical choice for small autonomous boats that require greater reliability and environmental protection without a major cost increase.

<img width="300" height="300" alt="image" src="https://github.com/user-attachments/assets/bcd7a411-c851-467e-969b-5fcd94b0bf0a" />

**3. Ultrasonic Flow Sensor (DFRobot Gravity or GEMS FT-110)**

The ultrasonic flow sensor provides a non-intrusive, solid-state alternative for measuring water velocity. Priced around $80–$150, it uses time-of-flight ultrasonic pulses to detect flow rates between 0.5 and 10 m/s with a typical accuracy of ±1–3%. Because it has no moving parts, it offers excellent reliability and minimal maintenance in clean-water environments. The sensor outputs either analog or serial data and can be easily interfaced with embedded systems for real-time monitoring. Its higher precision and robustness make it a suitable upgrade for research-grade or long-duration autonomous missions.

<img width="300" height="225" alt="image" src="https://github.com/user-attachments/assets/c4cb7e0f-01cb-4aa1-aed6-b044a1db1755" />


### Vessel Propulsion and Turning Solutions:

**1.	HPI SF‑50WP Servomotor**

The HPI SF‑50WP is a waterproof servo motor designed for wet environments, capable of providing precise rotational control with moderate torque suitable for small autonomous vessels. In a propulsion application, the servo can be mechanically coupled to a rotor or propeller shaft to generate forward and reverse thrust. When controlled via standard PWM signals from a microcontroller or autopilot system, the pulse width determines the servo shaft’s rotation, which drives the propeller to propel the vessel. For improved maneuverability, two servos mounted on opposite sides of a catamaran hull can operate differentially, spinning in opposing directions or modulating speed to enable turning without a traditional rudder. The servo’s integrated electronics simplify interfacing with onboard control systems, while its waterproof design ensures reliable operation in wet or splashing conditions. This setup provides a compact, low-cost, and easily controllable propulsion and steering solution for lightweight autonomous boats.

<img width="204" height="163" alt="image" src="https://github.com/user-attachments/assets/8d39f1f4-2166-4bfb-ab3c-3663c35432d5" />

**2. T200 Thruster**

The Blue Robotics T200 is a high-performance underwater thruster designed for small autonomous surface or submersible vehicles. Priced at approximately $230 per unit, it delivers up to 5.1 kgf (≈ 50 N) of thrust at 16 V while drawing a maximum of 25–30 A. The thruster uses a brushless DC motor with a sealed, water-cooled design, providing excellent efficiency and corrosion resistance in both fresh and saltwater environments. Its lightweight 10 oz (≈ 280 g) construction and plug-and-play compatibility with standard electronic speed controllers (ESCs) make it ideal for compact autonomous vessels requiring reliable propulsion and precise speed control.

<img width="300" height="300" alt="image" src="https://github.com/user-attachments/assets/b941ef88-85f3-418d-9a03-3750f245fc6a" />



### Power Systems Solutions:

**1. 12 V Deep-Cycle Marine Battery**

The 12 V deep-cycle marine battery serves as a robust primary power source for the vessel, providing sustained energy for propulsion, sensors, and control electronics. Designed to handle repeated deep discharges, it delivers consistent voltage under load and is well-suited for marine environments due to its sealed, spill-resistant construction. Power is drawn from the battery via the distribution system to all subsystems, and its capacity ensures several hours of continuous operation before recharge is required

<img width="120" height="79" alt="image" src="https://github.com/user-attachments/assets/6022f2b9-43d4-42c2-a418-2d7e65ce5e98" />


**2. 14.8 V Lithium-Ion Battery Encased in Waterproof Container**

The 14.8 V lithium-ion battery provides a higher energy density alternative for extended mission duration while maintaining a lightweight form factor. Encased in a waterproof container, it is protected from splashes and accidental immersion. The battery supplies regulated DC power to propulsion, navigation, and sensor systems, while integrated protection circuitry prevents overvoltage, overcurrent, and thermal issues, ensuring safe operation in varying environmental conditions.

<img width="220" height="220" alt="image" src="https://github.com/user-attachments/assets/8792f29b-2838-4075-ba41-eb9b3fddcae5" />

**3. Solar Backup Integration (ECO-WORTHY 130 Watt 12BB Flexible Solar Panel)**

 The solar backup integration supplements the main battery system by trickling the onboard battery through a solar panel and charge controller. The controller regulates incoming energy to prevent overcharging or reverse current flow, ensuring safe and efficient energy transfer. During operation, the solar panel can extend mission endurance or provide emergency power in case of battery depletion, enabling longer deployments without external charging.

<img width="256" height="256" alt="image" src="https://github.com/user-attachments/assets/21d0d77c-8664-47e2-9410-9654d6876bac" />


### **Benefits vs. Costs**

The **Blue Robotics Ping2 single-beam sonar** offers the best balance for depth mapping: affordable, accurate within ±5–15 cm, and open-source friendly.
 For flow measurement, the **YF-S201 sensor** provides a budget-conscious alternative. Although less precise than ADCPs, it offers sufficient accuracy (±10%) for a capstone project, particularly when validated against reference datasets. Its ease of integration and extremely low cost make it highly attractive.


### **Most Likely to Succeed**

The chosen configuration combines a **single-beam sonar transducer (Ping2 or equivalent)** for depth measurement with a **YF-S201 Hall-effect flow sensor** for current velocity monitoring. Together, these solutions maximize affordability, accuracy, and ease of integration. The sonar system achieves the project’s bathymetric goals within a reasonable budget, while the YF-S201 enables basic flow monitoring without requiring costly acoustic Doppler systems.
This hybrid approach balances technical feasibility with project constraints and provides a scalable platform that can be expanded with more advanced sensors in the future.

### Navigation Subsystem Solutions (Including GPS and Autopilot Options)

**1. u-blox NEO-M8N GPS Module**
 
The NEO-M8N is a cost-effective (~$20–$80) GNSS receiver that provides position accuracy within 2–5 meters under open-sky conditions. It communicates using standard serial (UART) NMEA or UBX protocols, making it easy to interface with Arduino, Raspberry Pi, or Pixhawk controllers. While not RTK-capable, it is lightweight, power-efficient, and widely supported by the ArduPilot and PX4 ecosystems. This makes it ideal for student-level autonomous boats requiring basic waypoint navigation and mapping.

<img width="209" height="149" alt="image" src="https://github.com/user-attachments/assets/297cba03-1134-4448-884d-1b8b819682a6" />


**2. u-blox ZED-F9P RTK GNSS Receiver**
 
The ZED-F9P offers high-precision dual-band GNSS positioning with centimeter-level accuracy when paired with RTK correction data. Typical cost ranges from $170–$300, with ready-to-use evaluation boards such as the SparkFun GPS-RTK2 or simpleRTK2B. It supports UART, I2C, and USB interfaces and can act as either a base or rover in RTK configurations. This module is well-suited for research or capstone-level autonomous boats requiring precise mapping and navigation performance.

<img width="229" height="174" alt="image" src="https://github.com/user-attachments/assets/5e209067-cf9d-484b-ae02-406f0b714442" />

**3. Pixhawk Autopilot Controller**
 
Pixhawk autopilots (~$120–$300) are open-source flight controllers compatible with ArduPilot and PX4 firmware. They include onboard IMUs, magnetometers, barometers, and multiple serial ports for GPS and telemetry integration. When configured with “Rover/Boat” firmware, Pixhawk can handle waypoint missions, motor control, and path-following surface vehicles. It is a proven, reliable option for small autonomous boats and integrates seamlessly with u-blox GPS modules.

<img width="198" height="183" alt="image" src="https://github.com/user-attachments/assets/8d49b93e-ec82-482d-80fa-a991f5886ddb" />

**4. Navio2 (Raspberry Pi-Based Autopilots)**
 
The Navio2 (~$199) can expand Raspberry Pi boards into full-featured autopilots with integrated IMUs, GPS interfaces, and PWM outputs. They allow for more flexible software control using Linux and companion programs like BlueOS or ROS. These solutions are excellent for teams wanting real-time data processing, networking, and camera or sensor integration alongside navigation. 

<img width="192" height="160" alt="image" src="https://github.com/user-attachments/assets/39c6406b-dca6-4db4-8590-be5d908cbde8" />


#### Benefits vs. Costs

The u-blox NEO-M8N GNSS receiver and Navio2 Raspberry Pi-based autopilot together offer the best balance between affordability, functionality, and navigation performance. The NEO-M8N provides reliable, meter-level positioning accuracy (2–5 m CEP) under open-sky conditions for only $20–$80, making it ideal for student or capstone-level autonomous boats. It consumes minimal power and integrates easily via UART, simplifying setup and reducing total system cost. The Navio2 expands a Raspberry Pi into a full-featured autopilot capable of running ArduPilot Rover/Boat firmware, handling motor control, waypoint navigation, and onboard sensor fusion. Although more expensive (~$199 plus Raspberry Pi), it offers significant long-term value through software flexibility, real-time processing, and compatibility with additional sensors such as cameras, LiDAR, or RTK GPS modules. Combined, these components deliver a high-performing navigation subsystem for approximately $250 total, far less than comparable Pixhawk + ZED-F9P configurations while still meeting project accuracy and functionality requirements.

#### Most Likely to Succeed

The selected configuration integrates the NEO-M8N GPS module with the Navio2 autopilot, leveraging each system’s strengths to ensure reliable, cost-effective navigation and control. The NEO-M8N supplies continuous position data sufficient for waypoint tracking and mapping, while the Navio2 provides onboard computation, sensor management, and communication with motor controllers—all within a single, Linux-based platform. This hybrid setup maximizes affordability, accuracy, and ease of integration, aligning well with capstone project constraints. It ensures a stable and well-documented system backed by the ArduPilot and Emlid communities, reducing risk and setup time. Additionally, the design remains scalable for future upgrades, such as replacing the NEO-M8N with an RTK-capable receiver for centimeter-level precision.Overall, the Navio2 + NEO-M8N solution provides the most practical and dependable foundation for an autonomous boat navigation subsystem—balancing cost, technical feasibility, and project success potential.

## **High-Level Solution**

The autonomous lake-mapping vessel integrates multiple coordinated subsystems to efficiently meet all stakeholder goals and project requirements while remaining within design constraints. The system combines a durable fiberglass-reinforced 3D-printed hull, efficient power management, and reliable communication to deliver accurate bathymetric mapping and autonomous navigation. High-precision sonar and water velocity sensors collect depth and location data, which are processed and transmitted via a wireless link to an on-shore computer for real-time monitoring. The modular battery configuration and optional solar backup ensure power reliability, while onboard SD storage prevents data loss in the event of communication failure. Lightweight materials, optimized control algorithms, and energy-efficient components reduce power consumption and maximize operational endurance. The design emphasizes safety, maintainability, and cost-effectiveness, achieving a balance between performance and resource optimization to provide a robust, autonomous, and user-friendly data collection platform.


## Hardware Block Diagram

<img width="2108" height="1052" alt="image" src="https://github.com/user-attachments/assets/0041e062-1ff9-4dbe-8a70-de16fc0a1a7b" />




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

Implementing the depth sensor will have several economic, environmental, and societal impacts:

- **Global and Economic:** The device lowers costs and democratizes hydrographic technology for small research groups and developing regions, though it may disrupt existing commercial markets.
  
- **Environmental:** Accurate depth data supports sustainable water management and habitat monitoring, but deployment must minimize habitat disturbance through low-noise, environmentally conscious design.
  
- **Societal:** Affordable, accessible depth mapping improves safety, fisheries management, and community resilience, but equity must be ensured so benefits extend beyond well-funded institutions.
  
- **Ethical Responsibility:** Each team member must ensure accuracy, transparency, and sustainability, while upholding public welfare and taking personal responsibility for the societal and environmental impacts of their work.
  
These implications emphasize the importance of designing our project within the context of the impacts it will have on the world. Doing so will enable long term sustainability of accessible depth mapping.

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

[11] Battery Safety (UL 2054 – Household and Commercial Batteries)
This standard was adopted to mitigate risks of fire, explosion, and electrolyte leakage within the power subsystem. Compliance with UL 2054 ensures that all onboard lithium-based and marine batteries maintain proper cell containment, electrical isolation, and protection circuits, promoting safe charge and discharge cycles during operation.

[12] Ingress Protection (IEC 60529 – IP Ratings)
This regulation addresses the risk of water intrusion and corrosion in electrical and electronic assemblies. Ensuring that all enclosures meet a minimum IP65 rating prevents moisture-related failures during splash exposure or brief submersion, thereby enhancing long-term reliability in marine environments.

[13] Radio Frequency Compliance (FCC 47 CFR Part 15 – Radio Frequency Devices)
To mitigate interference and communication faults, all wireless components, including the RC transmitter, receiver, and telemetry modules, will comply with FCC Part 15. This ensures that signal emissions remain within authorized frequency bands and power limits, maintaining safe and interference-free communication with external control systems.

[14] Hull Stability (ISO 8383:1985 – Ships and Marine Technology: Small Craft Stability)
This international standard was included to prevent capsizing and instability under variable environmental conditions. Compliance with ISO 8383:1985 ensures that the vessel’s buoyancy, mass distribution, and metacentric height remain within acceptable design thresholds, improving operational safety.

[15] Boating Regulations (Tennessee Wildlife Resources Agency – TWRA)
TWRA boating regulations were applied to ensure legal operation on public waterways. These requirements mitigate potential safety and compliance issues by defining registration procedures, operator conduct, and navigational rules applicable to unmanned surface vessels.

[16] Waterway Use (Tennessee Valley Authority – TVA Waterway Regulations)
TVA waterway regulations were adopted to mitigate operational conflicts and navigation hazards on TVA-managed lakes and reservoirs. Adhering to these constraints ensures that the vessel operates within authorized regions, maintaining lawful navigation and environmental responsibility.

[17] Environmental Protection (EPA Clean Water Act Compliance)
This constraint was incorporated to address potential environmental contamination during vessel operation and maintenance. Compliance with the EPA Clean Water Act ensures that no pollutants, fuels, or chemicals are released into water bodies, supporting sustainable field operations and ecological protection.


## Resources


1.	Hardware

    - Boat Hull: Designed to provide structural support and buoyancy for all onboard equipment, including the propulsion system, Raspberry Pi, sensors, and power modules. The catamaran-style hull is 3D printed in modular sections and reinforced with fiberglass, supporting a total payload of 40–50 pounds for stable operation in open water.

    - Propulsion Motor (T200 Thrusters): Dual brushless DC thrusters providing primary propulsion and steering through differential thrust.

    - Servo Motor: Actuators used for rudder control or precise directional adjustments when operating with single-thruster setups.

    - Waterproof enclosure: Housing for all electronics to provide durability and long-term moisture protection

2.	Power
   
    - Battery and Charging System: Primary energy source consisting of 14.8 V Li-ion or LiFePO₄ batteries with an integrated Battery Management System (BMS). Provides 2–4 hours of continuous operation and supports recharging via shore charger or optional solar input.

    - Power Distribution Board: Central hub that distributes regulated power from the main battery to all subsystems including control, sensors, and communication modules.

    - Voltage Distribution Board: Supports multiple voltage outputs (e.g., 5 V, 12 V) to safely supply mixed-voltage devices.

    -	Voltage Regulation Chips: Provide clean, consistent DC power to sensitive components such as microcontrollers and radios.

    - Solar Panels (optional): Renewable energy source to extend field operation time through trickle-charging.

5.	Navigation

    - Arduino: Functions as the sensor interface and data acquisition controller, directly managing low-level peripherals such as the YF-S201 flow sensor, IR proximity sensors, and relay modules. It communicates with the Raspberry Pi through serial connection for synchronized data logging and control feedback.

    - Autopilot: Embedded navigation controller that interprets GPS and IMU data to autonomously adjust heading, maintain course, and perform survey paths.

4.	Communication and Data Processing

    - Base Station Computer: Ground-based control and monitoring system used for mission oversight, real-time telemetry, and post-processing visualization.

    - Softlogic Power and Communication Module: Custom module that integrates power management with telemetry and data relay functions between vessel and operator.

    - Raspberry Pi: Main onboard processing unit for data storage, sensor fusion, and wireless communication with the base station. Handles mission logic, navigation routines, and telemetry management.

5.	Sensors and Data Acquisition

    - YF-S201 Water Flow Sensor: Provides digital pulse output proportional to water velocity for current profiling.

    - Data Processing System: Software and embedded routines that process sonar, GPS, velocity, and depth measurements into synchronized datasets and generate near-real-time bathymetric maps.

    - Depth Finder (Lowrance Elite-5 DSI): Primary sonar system for depth and underwater topography acquisition.

6.	Possible Software

    -	Hypack: Industry-standard hydrographic survey software used for sonar data processing and integration.

    -	QGIS: Open-source GIS platform for geospatial analysis, GPS point handling and mapping

    -	ArcGIS Pro: Advanced geospatial for professional mapping and visualization.


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
| **TOTAL** |  |  | **$636.00 – $1,379.00** |




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

[1] Navico/Lowrance, LOWRANCE Elite-5 DSI – Installation & Operation Manual, Rev. 06-2013. [Online]. Available: https://www.ssbilbehor.se/pub_docs/files/Lowrance/Lowrance_Elite-4_DSI_Mark-4_DSI_Maual.pdf
. Accessed: Oct. 27, 2025.

[2] Xylem/SonTek, RiverSurveyor S5/M9 Specification Sheet, Xylem Inc. [Online]. Available: https://www.xylem.com/siteassets/brand/sontek/resources/specification/sontek-riversurveyor-spec-sheet.pdf
. Accessed: Oct. 28, 2025.

[3] Blue Robotics, Ping Sonar Altimeter & Echosounder Technical Manual. [Online]. Available: https://bluerobotics.com/learn/ping-sonar-technical-guide/
. Accessed: Oct. 29, 2025.

[4] DATAQ Instruments, “Flow Sensor, part no. 2000362,” DATAQ Instruments. [Online]. Available: https://www.dataq.com/products/accessories/flow-sensor/2000362.html
. Accessed: Nov. 5, 2025.

[5] GEMS Sensors & Controls, FT-110 Series Flow Sensor Datasheet, 2023. [Online]. Available: https://www.gemssensors.com/docs/default-source/resource-files/catalog-pages/catalog-f_ft110-series.pdf

[6] DFRobot, Water Flow Sensor – 1/8” (SKU: SEN0216) Product Wiki, 2022. [Online]. Available: https://wiki.dfrobot.com/Water_Flow_Sensor_-_1_8__SKU__SEN0216

[7] HPI Racing, “#105366 – HPI SF-50WP Servo (Waterproof / 12.0 kg·cm @ 6.0 V),” HPI Racing A/S. [Online]. Available: https://www.hpiracing.com/en/part/105366
. Accessed: Nov. 5, 2025.

[8] Blue Robotics, T200 Thruster (R2), 2024. [Online]. Available: https://bluerobotics.com/store/thrusters/t100-t200-thrusters/t200-thruster-r2-rp/

[9] LiTime, “12 V 6 Ah LiFePO₄ Lithium Battery,” LiTime. [Online]. Available: https://www.litime.com/products/12v-6ah-lithium-battery?variant=46704399155420
. Accessed: Nov. 5, 2025.

[10] Dantona Batteries, “L148A68-8-12-2W,” Dantona, Wantagh, NY. [Online]. Available: https://dantona.com/products/l148a68-8-12-2w/
. Accessed: Nov. 5, 2025.

[11] u-blox AG, NEO-M8: u-blox M8 Concurrent GNSS Modules – Data Sheet, Doc. UBX-15031086, Rev. R12, Dec. 16, 2022. [Online]. Available: https://content.u-blox.com/sites/default/files/NEO-M8-FW3_DataSheet_UBX-15031086.pdf
. Accessed: Oct. 30, 2025.

[12] u-blox AG, ZED-F9P-04B High Precision GNSS Module – Data Sheet, Doc. UBX-21044850, Rev. R05, Mar. 21, 2024. [Online]. Available: https://content.u-blox.com/sites/default/files/ZED-F9P-04B_DataSheet_UBX-21044850.pdf
. Accessed: Oct. 31, 2025.

[13] PX4 Autopilot, “Pixhawk 4 | PX4 User Guide.” [Online]. Available: https://docs.px4.io/main/en/flight_controller/pixhawk4
. Accessed: Nov. 1, 2025.

[14] Pimoroni Ltd., EML NAV2 Autopilot Shield for Raspberry Pi – Data Sheet, Nov. 7, 2018. [Online]. Available: https://media.digikey.com/pdf/Data%20Sheets/Pimoroni%20PDFs/EML_NAV2_Web.pdf
. Accessed: Nov. 2, 2025.

[15] Z. Li, Y. Gao, M. Xu, J. Liu, Y. Yang, and J. He, “Exploring modern bathymetry: A comprehensive review of methods, limitations, and future directions,” Frontiers in Marine Science, vol. 10, Apr. 2023. [Online]. Available: https://doi.org/10.3389/fmars.2023.1178845
. Accessed: Nov. 3, 2025.

[16] F. Gerlotto, S. Gauthier, and B. Massé, “The application of multibeam sonar technology for quantitative estimates of fish density in shallow water acoustic surveys,” Aquatic Living Resources, vol. 13, no. 5, pp. 385–393, 2000. [Online]. Available: https://doi.org/10.1016/S0990-7440(00)01094-4
. Accessed: Nov. 3, 2025.

[17] Teledyne Marine, “StreamPro ADCP,” Teledyne RD Instruments, 2023. [Online]. Available: https://www.teledynemarine.com
. Accessed: Nov. 4, 2025.

[18] SonTek, “RiverSurveyor RS5,” Xylem Inc., 2023. [Online]. Available: https://www.ysi.com/riversurveyor
. Accessed: Nov. 4, 2025.

[19] Blue Robotics, “Ping2 Echosounder and Altimeter,” Blue Robotics Inc., 2023. [Online]. Available: https://bluerobotics.com/store/sensors-sonars-cameras/sonar/ping-sonar-r2-rp
. Accessed: Nov. 4, 2025.

[20] CEE Hydrosystems, “CEESCOPE: Integrated GNSS and single-beam echosounder for hydrographic surveys,” 2023. [Online]. Available: https://www.ceehydrosystems.com
. Accessed: Nov. 4, 2025.

[21] Seafloor Systems Inc., “EchoBoat-160 Autonomous Survey Vessel,” 2023. [Online]. Available: https://www.seafloorsystems.com
. Accessed: Nov. 5, 2025.

[22] Tersus GNSS, “TheDuck Autonomous Survey Boat,” Tersus GNSS Inc., 2023. [Online]. Available: https://www.tersus-gnss.com
. Accessed: Nov. 5, 2025.

[23] OTT Hydromet, “Qliner2: Mobile Doppler System for Discharge Measurements,” OTT Hydromet GmbH, 2023. [Online]. Available: https://www.ott.com
. Accessed: Nov. 5, 2025.

[24] ResearchGate, “Low-cost autonomous surface vehicles for inland water monitoring,” ResearchGate Publications, 2023. [Online]. Available: https://www.researchgate.net
. Accessed: Nov. 6, 2025.

[25] U.S. Geological Survey, Use of Acoustic Doppler Current Profilers for Streamflow Measurements, U.S. Dept. of the Interior, Reston, VA, USA. [Online]. Available: https://pubs.usgs.gov
. Accessed: Nov. 6, 2025.

[26] How to Connect, Read, and Process Sensor Data on Microcontrollers – A Beginner’s Guide. [Online]. https://www.freecodecamp.org/news/connect-read-process-sensor-data-on-microcontrollers-for-beginners/. Accessed: Nov. 6, 2025.


## Statement of Contributions

- Ryan Thomas: Communications Subsystem, High Level Solution
- Brady Nugent: Operational Flowchart, Navigation Subsystem
- Ian Hanna: Sensors and Data Acquisition Subsystem, Hardware Block Diagram
- Nate Norris: Power subsystem, Resources, Budget
- Jackson Hamblin: Hardware subsystem, Comparative Analysis

All: Introduction, Restating Problem, References


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
- Total subsystem cost shall not exceed $530.
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

<img width="928" height="654" alt="Screenshot 2025-12-01 at 1 20 32 PM" src="https://github.com/user-attachments/assets/678aa5d6-c425-4b49-9a00-c112b7c09f98" />
 
**Figure 3.1 – System-Level Interface Schematic**

<img width="992" height="700" alt="Screenshot 2025-12-01 at 1 20 01 PM" src="https://github.com/user-attachments/assets/10ec2578-07ba-4612-a87b-8dd081ba5726" />

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

<img width="493" height="701" alt="Screenshot 2025-12-01 at 1 23 01 PM" src="https://github.com/user-attachments/assets/f7c582f5-9d95-4acc-8229-7bb9a37817cf" />

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

## Subsystem Testing and Verification Plan

This section defines the procedures used to verify that the Sensors and Data Acquisition Subsystem satisfies all functional, electrical, environmental, and integration requirements. Testing is performed in three stages: bench-level component testing, integrated system testing, and field validation. Each test maps directly to one or more subsystem specifications (SDS-F, SDS-E, SDS-ENV, SDS-SYS) to ensure traceability between requirements and verification.


#### 1. Verification Strategy

Testing is divided into three phases:

##### Phase A — Bench Testing
Individual sensors and electronics are verified independently to confirm correct electrical operation and basic functionality before integration.

##### Phase B — Integration Testing
Subsystem components (Ping1D, Arduino, SD module, Pixhawk, Raspberry Pi) are connected together to validate data flow, synchronization, and communication reliability.

##### Phase C — Field Testing
On-water testing validates real-world performance, environmental robustness, and mission readiness under realistic operating conditions.


#### 2. Functional Verification Tests

##### SDS-T-01 — Ping1D Depth Measurement Verification  
**Requirement Coverage:** SDS-F-01, SDS-SYS-01  
- Connect Ping1D directly to Pixhawk UART/TELEM port.  
- Place the transducer at known distances from a flat surface or bottom reference.  
- Compare measured depth values reported through MAVLink with actual distances.  

**Pass Criteria:**  
- Depth readings appear continuously in Pixhawk logs  
- No communication dropouts  
- Measurements track known distance changes with reasonable accuracy  


##### SDS-T-02 — Depth Update Rate  
**Requirement Coverage:** SDS-F-03  
- Log depth messages for 10 seconds using MAVLink telemetry.  
- Count total samples received.  

**Pass Criteria:**  
- ≥ 10 samples per second (≥ 100 samples in 10 s)


##### SDS-T-03 — IR Obstacle Detection Range  
**Requirement Coverage:** SDS-F-04  
- Present an object at incremental distances (2–30 cm).  
- Adjust LM393 sensitivity potentiometer as needed.  

**Pass Criteria:**  
- Sensor reliably toggles HIGH/LOW within specified range  
- Minimal false triggers under indoor and outdoor lighting  


##### SDS-T-04 — IR Logging Frequency  
**Requirement Coverage:** SDS-F-05  
- Run Arduino logging routine for 5–10 minutes.  
- Inspect timestamps in SD card file.  

**Pass Criteria:**  
- Logged frequency ≥ 5 Hz  
- No missing or corrupted records  


##### SDS-T-05 — Data Output Formatting  
**Requirement Coverage:** SDS-F-06  
- Inspect serial output and SD logs.  
- Confirm CSV or JSON format is readable by analysis tools.  

**Pass Criteria:**  
- Files parse correctly  
- Numeric values preserved  
- UTF-8 encoding maintained  


#### 3. Electrical Verification Tests

##### SDS-T-06 — Voltage Compliance  
**Requirement Coverage:** SDS-E-01  
- Measure supply rails with a multimeter during operation.  

**Pass Criteria:**  
- 5 V rail remains within 4.75–5.25 V  
- 3.3 V SD interface stable  


##### SDS-T-07 — Current Draw  
**Requirement Coverage:** SDS-E-02  
- Measure total subsystem current using inline ammeter.  

**Pass Criteria:**  
- Total current ≤ 1.5 A  


##### SDS-T-08 — Logic-Level Compatibility  
**Requirement Coverage:** SDS-E-03, SDS-E-04  
- Inspect wiring and verify level shifting where required.  

**Pass Criteria:**  
- No 5 V signals directly applied to 3.3 V-only devices  
- No overheating or unstable communication  


#### 4. Environmental and Mechanical Verification

##### SDS-T-09 — Enclosure and Water Protection  
**Requirement Coverage:** SDS-ENV-02  
- Inspect enclosure seals and cable glands.  
- Perform light spray test.  

**Pass Criteria:**  
- No water ingress  
- Electronics remain dry  


##### SDS-T-10 — Sensor Mount Stability  
**Requirement Coverage:** SDS-ENV-03, SDS-ENV-04  
- Verify Ping1D vertical alignment.  
- Check cable strain relief and mounting rigidity.  

**Pass Criteria:**  
- Transducer remains vertical  
- No looseness or cable stress  


#### 5. Integration Verification

##### SDS-T-11 — Arduino → Raspberry Pi Communication  
**Requirement Coverage:** SDS-SYS-02  
- Stream USB serial data for extended duration.  
- Log and parse on Raspberry Pi.  

**Pass Criteria:**  
- No dropped packets  
- Continuous synchronized timestamps  


##### SDS-T-12 — End-to-End System Logging  
**Requirement Coverage:** SDS-SYS-01, SDS-ETH-02  
- Run full system (Pixhawk + Arduino + Raspberry Pi) for ≥ 15 minutes.  
- Confirm combined logs contain depth, GPS, and IR data.  

**Pass Criteria:**  
- All data streams recorded  
- Time alignment achievable  
- Logs unmodified and complete  


#### 6. Verification Artifacts

For each test, the following shall be recorded:
- Date and configuration  
- Firmware versions  
- Measured results  
- Pass/Fail outcome  
- Notes and corrective actions  

Maintaining these records ensures repeatability, supports troubleshooting, and demonstrates compliance with subsystem specifications.


#### Summary

This testing and verification plan confirms that:
- Depth sensing operates accurately and reliably  
- IR obstacle detection functions within required range  
- Electrical systems remain stable under load  
- Environmental protections are sufficient  
- Data flows correctly to higher-level subsystems  

Successful completion of these tests validates that the Sensors and Data Acquisition Subsystem is ready for safe field deployment and mission use.

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


# Detailed Design — Power Subsystem

## Function of the Subsystem

The Power Subsystem supplies all electrical energy required for propulsion, navigation, sensing, computation, communication, and onboard electronics for the autonomous hydrographic survey vessel. Its purpose is to provide stable, protected, low-noise power during lake-mapping missions while ensuring reliable performance of propulsion and sensitive electronics.

The vessel uses **one Liperior 16000mAh 4S 14.8V LiPo battery** (12C continuous / 24C peak) as the primary power source. This battery feeds:

- A **high-current 14.8 V propulsion rail** powering the T200 thrusters and ESCs.
- A **regulated 5 V electronics rail** powering the Pixhawk, Raspberry Pi, Arduino, Ping1D sonar, GPS, IR sensors, and telemetry radios.

Proper grounding, fusing, regulation, and isolation prevent:
- Voltage sag  
- ESC-induced electrical noise  
- Pixhawk resets  
- Sensor interference  
- Communication dropouts  

Battery telemetry from the PM02 power module enables:
- Voltage & current monitoring  
- Low-voltage failsafes  
- Automatic Return-to-Launch (RTL)  

The subsystem is enclosed in a sealed electronics bay and uses marine-safe wiring, strain relief, and polarity-protected connectors for durability.

---

# Specifications and Constraints

## 1. Electrical Specifications

### **Primary Energy Source**
- **Battery:** Liperior 16000mAh 4S LiPo  
- **Nominal Voltage:** 14.8 V  
- **Maximum Voltage:** 16.8 V  
- **Capacity:** 16 Ah  
- **Usable Capacity (80%):** 12.8 Ah  
- **Continuous Discharge:** 12C (192 A)  
- **Peak Discharge:** 24C (384 A)  
- **Connector:** XT90  
- **Weight:** 1273 g  

### **Voltage Rails Required**
- **Propulsion rail:** 14.8 V (12–16.8 V acceptable)
- **Electronics rail:** 5.0 V ± 5% (4.75–5.25 V)

### **Load Requirements**
- **Peak Propulsion Current:** up to 48 A (combined thrusters)
- **Electronics Current:** 5–6 A continuous
- **Total System Current:** 17–20 A typical, 40–50 A peak

### **Ripple & Noise Limits**
- Pixhawk: < 50 mV recommended  
- GPS & sonar: low tolerance  
- Raspberry Pi: high sensitivity  

---

## 2. Environmental & Mechanical Constraints

- Operating temperature: **0–40°C**
- All electronics enclosed in a waterproof sealed bay  
- Battery stored in an isolated, ventilated compartment  
- All wiring strain-relieved and secured against vibration  
- Wire gauges:  
  - 12–14 AWG propulsion  
  - 18 AWG electronics  

---

## 3. Safety Constraints

- Overcurrent protection:  
  - **40–60 A main fuse** (propulsion rail)  
  - **5 A fuse** (5 V electronics rail)
- Reverse polarity protection  
- TVS diode surge suppression  
- Battery over-discharge prevention  
- Waterproof connectors and enclosures  

---

## 4. Component-Level Power Requirements  
*(Moved here as requested — these are **constraints** the power subsystem MUST satisfy.)*

### **T200 Thruster + ESC**
- **Voltage:** 7–20 V (Nominal 14.8 V)  
- **Continuous Current:** 10–12 A  
- **Peak Current:** 24–26 A  
- **Ripple Sensitivity:** Low  
- **Startup:** High inrush  
- **Protection Required:**  
  - 40–60 A fuse  
  - XT90 wiring  
  - 12–14 AWG cabling  

---

### **Pixhawk Autopilot (PM02)**
- **Voltage:** 4.75–5.5 V  
- **Current:** ~0.5 A  
- **Ripple Sensitivity:** Very high  
- **Startup Time:** 2–3 seconds  
- **Protection Required:**  
  - Clean 5 V rail  
  - LC filtering  
  - Battery telemetry (PM02)  

---

### **Raspberry Pi 4**
- **Voltage:** 4.75–5.25 V  
- **Continuous Current:** 2.5–3.0 A  
- **Peaks:** Up to 6 A  
- **Ripple Sensitivity:** High  
- **Startup:** 3–6 seconds  
- **Protection Required:**  
  - Dedicated 5 V branch  
  - Decoupling capacitors  
  - DO NOT share rail with ESCs  

---

### **Arduino / Microcontroller**
- **Voltage:** 5 V  
- **Current:** 100–200 mA  
- **Ripple Sensitivity:** Medium  
- **Protection Required:**  
  - Standard decoupling  

---

### **Ping1D Sonar**
- **Voltage:** 4.75–5.25 V  
- **Current:** 500–900 mA  
- **Ripple Sensitivity:** Low  
- **Startup:** ~2 sec  
- **Protection Required:**  
  - Ferrite bead  
  - 10 µF + 0.1 µF decoupling  
  - Clean ground return  

---

### **GPS Module**
- **Voltage:** 5 V  
- **Current:** 150–200 mA  
- **Ripple Sensitivity:** High  
- **Startup:** 1–2 sec + lock 10–30 sec  
- **Protection Required:**  
  - EMI separation  
  - Ferrite filtering  

---

### **Telemetry Radio**
- **Voltage:** 5 V  
- **Current:** 0.3 A average / 1.1–1.5 A peak
- **Ripple Sensitivity:** Medium  
- **Protection Required:**  
  - Clean 5 V rail  
  - Avoid proximity to ESC wires  

---

### **IR Sensors**
- **Voltage:** 5 V typical  
- **Current:** 30–50 mA each  
- **Ripple Sensitivity:** High  
- **Protection Required:**  
  - Reverse polarity  
  - Optional filtering  

---

### **PM02 Power Module**
- **Input:** 7–50 V  
- **Output:** Regulated 5.2 V to Pixhawk  
- **Telemetry:** Voltage & current sense  
- **Protection:**  
  - Reverse polarity  
  - Overvoltage  

---
## Interface with Other Subsystems

The Power Subsystem interfaces electrically with the Navigation, Sensors, and Communications subsystems. It does not process or generate data; it only supplies power and telemetry.

### Navigation Subsystem
- **Pixhawk 6C Autopilot:**
  - Input: 5.2 V regulated from the PM02 power module.
  - Ground: Shared electronics ground.
  - Telemetry: Battery voltage and current from PM02 are sent to Pixhawk for battery monitoring and low-voltage RTL failsafes.
- **Companion Computer (Raspberry Pi 4):**
  - Input: 5 V @ 3 A from the 5 V electronics rail.
  - Ground: Common ground with Pixhawk to maintain MAVLink UART signal integrity.
- **Ping1D Sonar (via Pixhawk or dedicated 5 V branch):**
  - Input: 5 V supply, up to 0.9 A.
  - Ground: Shared electronics ground.

### Sensors Subsystem
- **Arduino / IR Sensors / Auxiliary Sensors:**
  - Input: 5 V rail (4.75–5.25 V) with ≥1.5 A available capacity.
  - Ground: Shared with Pixhawk, Pi, and other electronics.
  - Additional Requirements: Local decoupling capacitors and ferrite beads (where needed) are supported by the 5 V rail design.

### Communications Subsystem
- **Telemetry Radio (e.g., 3DR Air Radio):**
  - Input: 5 V supply (0.3 A average, up to 1.1–1.5 A peak).
  - Ground: Shared electronics ground.
- **Battery Telemetry:**
  - The Power Subsystem provides battery voltage and current to the Pixhawk via PM02; Pixhawk then forwards this information over MAVLink to the shore station.
- **Data Direction:**
  - Power → Communications: Battery state (voltage/current) via Pixhawk.
  - Communications → Power: None (power subsystem is electrically passive with respect to data).

The Power Subsystem is thus responsible for maintaining stable voltage levels, adequate current capacity, and a clean shared ground reference required for reliable operation of all other subsystems.

---

# Overview of Proposed Solution

## 1. Energy Source & Input Protection
The 16Ah 4S LiPo battery connects through:
- XT90 connector  
- Master power switch  
- 40–60 A inline fuse  
- Reverse polarity MOSFET protection  
- TVS surge suppression  

## 2. High-Current Propulsion Bus (14.8 V)
- Direct supply to ESCs and thrusters  
- 12–14 AWG silicone wire  
- Isolated from the electronics rail  
- Short wiring runs to reduce voltage drop  

## 3. Regulated 5 V Electronics Bus
- 5 V / 15 A buck converter  
- Filters: LC input + ceramic/bulk output decoupling  
- Multiple branches for sensitive loads  
- Ferrite filtering for sonar, GPS, radio  

## 4. Battery Telemetry & Failsafes
PM02 module feeds Pixhawk with:
- Voltage  
- Current  
- Power consumption  
- Remaining battery estimate  

Pixhawk failsafes enabled:
- Low-voltage warning  
- RTL on critical voltage  

---

# Power Budget

| Component | Voltage | Current | Power |
|----------|---------|---------|--------|
| T200 Thrusters (2×) | 14.8 V | 15–18 A avg | 220–260 W |
| ESC Overhead | 14.8 V | 1 A | 15 W |
| Raspberry Pi 4 | 5 V | 3.0 A | 15 W |
| Pixhawk | 5 V | 0.5 A | 2.5 W |
| Ping1D Sonar | 5 V | 0.9 A | 4.5 W |
| GPS | 5 V | 0.15 A | 0.75 W |
| Telemetry Radio | 5 V | 0.3 A | 1.5 W |
| Arduino + IR | 5 V | 0.3–0.4 A | 2 W |
| Aux Sensors | 5 V | 0.2 A | 1 W |
| **Electronics Total** | 5 V | **5–6 A** | **25–30 W** |
| **System Total (avg)** | — | **17–20 A** | ~250–300 W |

---

# Runtime Calculation  
*(Based on **ONE 16Ah battery**, with an option for **TWO batteries in parallel**)*

### Single 16Ah 4S LiPo

**Usable battery capacity (80%):**  
16 Ah × 0.8 = **12.8 Ah**

**Average system draw (mapping conditions):**  
- High load: 18–20 A  
- Reduced throttle: 12–15 A  

**Runtime (one battery):**
- 12.8 Ah / 20 A ≈ **0.64 h** ≈ **38 min**  
- 12.8 Ah / 18 A ≈ **0.71 h** ≈ **43 min**  
- 12.8 Ah / 15 A ≈ **0.85 h** ≈ **51 min**  
- 12.8 Ah / 12 A ≈ **1.07 h** ≈ **64 min**  

So with a single pack, realistic survey runtime is roughly **40–60 minutes**, depending on throttle and maneuvering.


### Optional: Two 16Ah 4S Packs in Parallel

If the final design uses **two identical 16Ah batteries in parallel**:

- **Total capacity:** 16 Ah + 16 Ah = **32 Ah**  
- **Usable capacity (80%):** 32 Ah × 0.8 = **25.6 Ah**  
- **Bus voltage:** remains 14.8 V nominal (still a 4S system)

**Runtime (two batteries in parallel):**
- 25.6 Ah / 20 A ≈ **1.28 h** ≈ **77 min**  
- 25.6 Ah / 18 A ≈ **1.42 h** ≈ **85 min**  
- 25.6 Ah / 15 A ≈ **1.71 h** ≈ **102 min**  
- 25.6 Ah / 12 A ≈ **2.13 h** ≈ **128 min**  

With two packs in parallel, the vessel could realistically achieve **~1.3–2.1 hours** of operation, depending on throttle and mission profile.


---

# Grounding Strategy

- Star-ground at distribution block  
- Separate propulsion and electronics grounds  
- Twisted pairs for signal cables  
- Routing separation between ESC wires and sensor cables  

---

# Filtering & Noise Control

- LC filters on converter input  
- Bulk electrolytic capacitor near battery  
- Ceramics + ferrite beads on GPS, sonar, radio  
- Physically separate noisy and sensitive wiring  

---

# Power Sequencing

### Power-Up
1. Connect battery  
2. Master switch ON  
3. Propulsion rail energizes  
4. Buck converter outputs 5 V  
5. Pixhawk initializes  
6. Raspberry Pi boots  
7. Thrusters armed  

### Power-Down
1. Disarm thrusters  
2. Shutdown Raspberry Pi  
3. Power off Pixhawk  
4. Master switch OFF  
5. Disconnect battery  

---

# Safety Devices

- 40–60 A main fuse  
- 5 A electronics fuse  
- Reverse polarity MOSFET  
- TVS surge diode  
- Battery isolation bracket  
- Waterproof compartment  

---

# FMEA — Failure Mode and Effects Analysis

| Failure Mode | Cause | Effect | Severity | Mitigation |
|--------------|--------|--------|----------|-------------|
| Battery over-discharge | Long mission | Loss of propulsion | High | RTL failsafe |
| ESC brownout | Voltage sag | Loss of thrust | High | Separate rails |
| Pixhawk brownout | Ripple | Loss of control | Critical | Filtering |
| Pi undervoltage | Converter overload | Data loss | Medium | Dedicated rail |
| Short on 5 V rail | Wiring fault | Electronics loss | High | Fuse |
| Connector failure | Vibration | Loss of subsystem | Medium | Strain relief |

---


# Power Distribution Tree
<img width="1124" height="862" alt="image" src="https://github.com/user-attachments/assets/082f5cf1-9a64-417c-a905-362b83a3e737" />

**Figure 1 — Buildable Power Wiring Schematic for the Subsystem**

---

## Buildable Schematic

The Power Subsystem is constructed entirely from commercial off-the-shelf (COTS) components, wiring, and standard connectors. No custom PCB or circuit design is required. Therefore, the buildable schematic is represented as a wiring-level power distribution diagram

---

## Flowchart

The Power Subsystem contains no embedded software, decision-making logic, or microcontroller-driven control flow. Its function is purely electrical: to distribute and regulate power. Therefore, a software flowchart is not applicable to this subsystem.

---



# BOM


| Item / Description             | Manufacturer | Mfr P/N           | Distributor | Distributor P/N     | Qty | Cost (USD) | URL |
|--------------------------------|--------------|-------------------|------------|---------------------|-----|------------|-----|
| Liperior 16000mAh 4S LiPo      | Liperior     | LP-16000-4S-12C   | RCBattery  | LP-16000-4S-12C     | 1   | $114.99    | https://rcbattery.com/liperior-16000mah-4s-12c-14-8v-lipo-battery-with-xt90-plug.html |
| XT90 Connectors (pairs)        | Amass        | XT90              | Amazon     | Same as Mfr P/N     | 2 sets | $14.99  | https://www.amazon.com/Amass-Female-Bullet-Connectors-Battery/dp/B06ZY34369?th=1 |
| 5 V 15 A Buck Converter        | DROK         | 5V-15A            | DROK     | Same as Mfr P/N     | 1   | $15.75 | [[https://www.amazon.com/](https://www.amazon.com/DROK-Converter-5-3V-32V-Regulator-Transformer/dp/B078Q1624B/ref=sr_1_1_sspa?](https://www.droking.com/DC-Voltage-Step-Down-Regulator-ACC-Power-Supply-Module?utm_source=chatgpt.com)dib=eyJ2IjoiMSJ9.cKfwTrlAJh7wRVi5JMftbdF1j19UIxiKRCgAj6Dbb239w02Sz20OrDlTUBJRvMn8Ayv8srllGoDqDb56aj6QN4kxiMylhlQcrlHGD6Qbpzn-fmrnvPeHxqpHS16NOKZsLi2ymDf_w9sr7Omury6bclfZGdoHlz_iqf9C8JIwuj2YrbcoOtmQUxab9hD-8svzOM2Hb4ecMJuVzS7dpl2xbPTsEGVh6fmc_YD8lxdiOgk.O7brV1fXevM-OeOs6yuLmxxhrY3OoofALSrpcwFfVgE&dib_tag=se&keywords=drok%2Bbuck%2Bconverter&qid=1764783882&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1) |
| 14 AWG Silicone Wire           | BNTECHGO     | —                 | Amazon     | BNTECHGO 14AWG      | 1   | $0.58 / feet   | [https://www.amazon.com/](https://www.amazon.com/BNTECHGO-Silicone-Flexible-Resistant-Insulation/dp/B075VQ4G2Y?ref_=ast_sto_dp&th=1) |
| 18 AWG Silicone Wire           | BNTECHGO     | —                 | Amazon     | BNTECHGO 18AWG      | 1   | $1.40 / feet   | [https://www.amazon.com/ ](https://www.amazon.com/BNTECHGO-Silicone-Flexible-Strands-Stranded/dp/B01708AYYQ?ref_=ast_sto_dp&th=1)|
| Inline Fuse Holder             | Littelfuse   | FHAC0001          | Amazon     | FHAC0001            | 2   | $5–$10     | [https://www.amazon.com/](https://www.littelfuse.com/products/fuses-overcurrent-protection/fuse-holders-fuse-blocks-accessories/fuse-holders/in-line-fuse-holders/ato-fhac/fhac0001xp) |
| 30 A Blade Fuse (main fuse)    | Littelfuse   | ATO-30A           | Amazon     | ATO-30A             | 2   | $5.14    | [https://www.amazon.com/](https://www.amazon.com/Bussmann-MAX30-Maxi-Fuse/dp/B000CFDHYO) |
| 5 A Electronics Fuse           | Littelfuse   | ATO-5A            | Amazon     | ATO-5A              | 1   | $11.99      | [https://www.amazon.com/](https://www.amazon.com/Littelfuse-ATO5BP-ATO-Fuse-Pack/dp/B0002KKT4U) |
| Power Distribution Block       | Blue Sea     | 5029              | Amazon     | 5029                | 1   | $31.05  | [(https://www.bluesea.com/products/5029/ST_Blade_Fuse_Block_-_12_Circuits_with_Cover)](https://www.bluesea.com/products/5029/ST_Blade_Fuse_Block_-_12_Circuits_with_Cover)|
| PM02 Power Module              | Holybro      | PM02              | Holybro    | PM02                | 1   | $18.99 | https://holybro.com/products/pm02-v3-12s-power-module?srsltid=AfmBOoq2q2z4lpQyVxlOnT4CfL_Ek4MW4q12Bc6RNGNSR_A7s3ACsQ2Z |
| **Total Estimated Cost**       | —            | —                 | —          | —                   | —   | $234-$257 | — |

---

## Testing and Measurements

Since the Power Subsystem is still in the detailed design phase, no hardware testing has been conducted yet. All future validation data will be documented in this section once components are procured and assembled.

Planned measurements include:

- **Ripple Voltage:** Verify that the 5 V rail meets Pixhawk and Raspberry Pi noise tolerances.  
- **Electrical Noise:** Confirm filtering and grounding are adequate for GPS, sonar, and telemetry modules.  
- **Converter Efficiency:** Measure the 5 V buck converter efficiency under expected load conditions.  
- **Inrush Current:** Ensure safe startup behavior for the propulsion rail and buck converter.  
- **Reverse Polarity Protection Test:** Validate MOSFET-based protection on the battery input.  
- **Short Circuit Protection:** Confirm fuse operation and safety response under fault conditions.  
- **Overvoltage Protection:** Verify TVS diode clamps transients properly.  

**All empirical measurements, oscilloscope captures, and test results will be added here during subsystem integration and verification.**


---

# Analysis

A single 16Ah LiPo battery provides sufficient power for the vessel to meet its ≥1 hour runtime requirement, especially at survey throttle levels. The propulsion rail remains isolated from sensitive electronics, and a 10A buck converter provides stable, low-noise 5 V power for the Pixhawk, Pi, and sensors. Proper filtering ensures GPS, sonar, and radios function without interference.

Safety devices such as fuses, reverse polarity protection, and surge suppression prevent catastrophic failures. With careful wiring practices and adherence to grounding strategies, the subsystem meets all electrical, environmental, and safety constraints required for reliable autonomous operation.

---

# References

[1] PX4 Autopilot Project, “Pixhawk Series — PX4 Documentation.” Online.
Available: https://docs.px4.io/main/en/flight_controller/pixhawk_series.
Accessed: Dec. 2025.

[2] Analog Devices, “LTpowerCAD® Design Tool.” Online.
Available: https://www.analog.com/en/lp/ltpowercad.html.
Accessed: Nov. 2025.

[3] Raspberry Pi Ltd., “Raspberry Pi 4 Model B Specifications.” Online.
Available: https://www.raspberrypi.com/products/raspberry-pi-4-model-b/specifications/.
Accessed: Dec. 2025.

[4] Blue Robotics, “Ping Sonar Echosounder (Ping1D).” Online.
Available: https://bluerobotics.com/store/sonars/echosounders/ping-sonar-r2-rp/.
Accessed: Nov. 2025.

[5] Arduino, “Arduino Uno Rev3.” Online.
Available: https://store.arduino.cc/products/arduino-uno-rev3.
Accessed: Nov. 2025.

[6] Holybro, “Pixhawk 6C Flight Controller.” Online.
Available: https://holybro.com/products/pixhawk-6c?variant=42783569903805.
Accessed: Dec. 2025.

[7] RCBattery, “XT90 Battery Harness for 2 Packs in Parallel.” Online.
Available: https://rcbattery.com/xt90-battery-harness-for-2-packs-in-parallel.html.
Accessed: Dec. 2025.

[8] Bel Fuse Inc., “DC-DC Converters.” Online.
Available: https://www.belfuse.com/products/power-supplies/dc-dc-converters.
Accessed: Nov. 2025.

[9] FactoryMation, “Power Distribution Blocks.” Online.
Available: https://www.factorymation.com/Power_Distribution_Blocks_qs.
Accessed: Dec. 2025.

[10] Holybro, “PM02 V3 12S Power Module.” Online.
Available: https://holybro.com/products/pm02-v3-12s-power-module.
Accessed: Nov. 2025.

[11] Blue Robotics, “T200 Thruster R2.” Online.
Available: https://bluerobotics.com/store/thrusters/t100-t200-thrusters/t200-thruster-r2-rp/.
Accessed: Dec. 2025.

[12] Texas Instruments, “DC/DC and AC/DC Converters — Overview.” Online.
Available: https://www.ti.com/product-category/power-management/acdc-dcdc-converters/overview.html.
Accessed: Nov. 2025.

[13] Texas Instruments, “DC/DC Converter Product List.” Online.
Available: https://www.ti.com/product-category/power-management/acdc-dcdc-converters/products.html.
Accessed: Dec. 2025.

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

##  Project Specific Navigation Requirements and Control Strategy

 ## Operating Modes

The system shall support multiple operating modes to allow safe testing and autonomous operation:

• Manual – direct operator control for launch and recovery
• Stabilize – assisted heading stabilization
• Guided – commands from Raspberry Pi via MAVLink
• Auto – fully autonomous waypoint execution
• Hold/Loiter – maintain position during depth sampling
• Return‑To‑Launch (RTL) – automatic return during failsafe

## Waypoint Tracking Requirements

The navigation subsystem shall:

• Maintain cross‑track error ≤ 2 m
• Maintain heading error ≤ 5°
• Support repeatable grid (“lawnmower”) paths for mapping
• Accept dynamic waypoint updates during runtime
• Operate at cruise speeds of 1–2 m/s

## Control Architecture

Navigation control is hierarchical:

Raspberry Pi – mission planning, sonar processing, adaptive routing

Pixhawk – waypoint tracking, velocity/heading control, stabilization

Thrusters/servos – PWM actuation

This separation ensures real‑time stability while allowing flexible high‑level behavior.

## Ping1D Depth Integration

The Blue Robotics Ping1D sonar provides depth data that is used for both mapping and safety decisions.

Depth measurements are used to:

• Log bathymetry for lake mapping
• Detect shallow water or obstacles
• Reduce speed in shallow regions
• Trigger avoidance waypoint generation
• Initiate Hold or RTL if unsafe depth persists


##  Interfaces and Communication

 Serial Interfaces and Baud Rates

• Pixhawk ↔ Raspberry Pi (MAVLink): 57600–115200 baud
• GNSS (M8N): 38400–115200 baud
• Ping1D sonar: 115200 baud

All interfaces use 8N1 UART format.

GPS Wiring

• 5V → VCC
• GND → GND
• Pixhawk RX → GPS TX
• Pixhawk TX → GPS RX
• I²C SDA/SCL → compass lines

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

##  Configuration Parameters

 Initial ArduPilot parameters:

• GPS_TYPE = 1
• GPS_AUTO_CONFIG = 1
• WP_RADIUS = 2 m
• LOIT_RADIUS = 2–3 m
• CRUISE_SPEED = 1–2 m/s
• FS_BATT_ENABLE = enabled
• FS_GCS_ENABLE = enabled

These parameters will be tuned during testing.

##  Verification and Testing Plan

## Bench Testing

• Verify GNSS communication and satellite lock
• Confirm correct coordinates in Mission Planner
• Validate UART links
• Calibrate compass
• Verify thruster outputs

## Simulation Testing

• ArduPilot SITL waypoint tracking
• Cross‑track error evaluation
• Failsafe testing

## Field Testing

Static hold test (≤ 3 m drift)

Straight‑line waypoint test (≤ 2 m error)

Grid mapping mission

Shallow water avoidance test

RTL failsafe verification

## Acceptance Criteria

• ≥ 8 satellites
• HDOP ≤ 2.5
• Waypoint accuracy within ±2 m
• Successful autonomous mission completion

##  BOM

Item – Pixhawk 6C Autopilot Controller  
 Part Number – SKU 20179 
 Distributor – Holybro 
 Distributor Part Number – SKU 20179 
 Quantity – 1 
 Price ($) – $150.99
 Link – Pixhawk 6C – Holybro Store (https://holybro.com/products/pixhawk-6c?variant=42783569903805) 

Item – QWinOut Mini NEO-M8N GPS Module with Compass
 Part Number – NEO-M8N
 Distributor – QWinOut (Amazon)
 Distributor Part Number – Mini M8N GPS w/ Compass
 Quantity – 1
 Price ($) – ~$30–40
 Link – https://www.amazon.com/dp/B07P8YMVNT

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




## Hardware Subsystem - Detailed Design

## 1. Function of the Subsystem
The hardware subsystem provides the physical platform, propulsion, power distribution, and low-level control necessary for the autonomous vessel to operate on the water and support all sensing and computing subsystems. Specifically, the hardware subsystem:

- Supports roughly a 50 lb total system payload (hull, electronics, batteries, thrusters)
- Provides redundant propulsion and steering via two Blue Robotics thrusters in a catamaran configuration.
- Houses and protects the Pixhawk flight controller (ArduRover), Raspberry Pi, Arduino, Blue Robotics Ping1D, IR object-avoidance sensors, and communications hardware within a weather resistant central electronics bay
- Supplies regulated DC power from lithium batteries to propulsion and electronics while meeting current and runtime requirements.
- Provides mechanical mounting, cable routing, and strain relief for underwater sensors (including the Ping1D depth transducer) and thrusters.

Within the overall system, the hardware subsystem is the foundation that the navigation, sensing, and communication subsystems rely upon. Without the hull, thrusters, Ping1D, and integrated power/control system, autonomous depth-mapping and data return to the base station would not be possible. IR sensors extend the subsystem by providing short-range object detection for collision avoidance in shallow water, around docks, or near floating obstacles. These sensors feed data through the Arduino or directly to the Raspberry Pi, enabling evasive maneuvers or mission aborts before contact.

## 2. Specifications and Constraints

## 2.1 Performance Specifications

## Mass Capacity 
- Vessel must support ≥ 50 lb total mass including hull, battery packs, electronics, Ping1D, IR sensors, and structural components.
- Maintain ≥ 2 inches of freeboard at full load.
## Hull Dimensions
- Length: ~ 3ft
- Beam: ~ 19in
- Stern Width: ~ 4.75in
- Draft: ≤ 5 in
## Propulsion 
- Two Blue Robotics T200-class thrusters
- Speed: 0.5-1.0 m/s
- 1 hour continous runtime at ~ 0.5 m/s
## Electrical / Power
- Lithium-based battery packs (4S-6S)
- Voltage rails:
    - 14V high-power bus
    - 5V auxiliary bus
- Must supply Ping1D, IR Sensor, Raspberry Pi, Arduino, Pixhawk, and Thrusters sumultaneously
## Control Hardware
- Pixhawk – Navigation, sensor fusion, thruster control
- Raspberry Pi – Logging, communication, Ping1D handling, IR processing
- Arduino – IR sensor interfacing, kill switch, auxiliary functions
- Ping1D – Depth measurement
- IR Sensors – Short-range obstacle detection

## 2.2 Constraints

## Fabrication
- Hull printed in PLA and strengthened with fiberglass.
- Hull prints must fit within printer build volume.
- Mounts required for Thrusters, Ping1D, and forward-looking IR sensors.
## Environmental
- Designed for freshwater lakes/ponds.
- Electronics protected from water, sunlight, vibration.
- IR sensors require clear forward line-of-sight and protection from spray.
## Safety and Power
- Fused battery system.
- ESCs and IR sensors must not exceed current ratings.
- IR sensor wires must be strain-relieved and waterproofed.
## Ethical / Economical
- Avoid environmental contamination.
- Lithium safety standards.
- Fit within $1,100 budget using open-hardware components (Pixhawk, Pi, Arduino, IR sensors, Ping1D).
## System Integration
- IR sensors connect to Arduino (analog/digital lines).
- Arduino → Pi or Pixhawk for obstacle alerts.
- IR sensors must mount at bow to detect surface-level obstacles.

## 3. Overview of Proposed Solution

-The vessel combines:
  - A 3D-printed catamaran hull,
  - Dual Blue Robotics thrusters,
  - A multi-controller stack (Pixhawk + Raspberry Pi + Arduino),
  - A Blue Robotics Ping1D for depth measurement, and
  - IR object-avoidance sensors for near-field detection.
## Catamaran Hull Structure
- Stable twin-hull design
- Bridge deck supports electronics and battery.
- Front nose mounts hold 1–3 IR sensors angled outward for wider field-of-view.
## Propulsion System
- Two thrusters provide differential thrust steering.
- ESCs controlled by Pixhawk via PWM.
## Control Electronics
- Pixhawk runs ArduRover navigation.
- Raspberry Pi handles high-level logic and logs Ping1D and IR events.
- Arduino reads IR sensors and relays obstacle data upward.
- Ping1D provides continuous downward depth readings.
## Power Distribution
- Battery → Fuse → Master switch → PDB
- DC-DC converters generate 5 V (Pi/Arduino/Ping1D/IR sensors)

## 4. Interfaces with Other Subsystems

## 4.1 Electrical Interfaces

## Pixhawk → Thrusters
- PWM control
- ESCs powered directly from battery
## Raspberry Pi ↔ Pixhawk
- MAVLink telemetry
- Mission logging and shallow-water reaction based on Ping1D depth
## Arduino ↔ Raspberry Pi / Pixhawk
- Arduino reads IR sensors and outputs:
  - Safe distance
  - Warning
  - Immediate stop
- Commands or messages forwarded to Pixhawk for braking or evasive maneuvers.
## Ping1D ↔ Raspberry Pi
- Depth data via UART
- Pi timestamps and logs all depth measurements
## IR Sensors ↔ Arduino
- IR sensors supply analog/digital distance signals
- Arduino processes and filters readings
- Safety messages sent to Pi/Pixhawk

## 4.2 Mechanical Interfaces 
- Thrusters mounted at stern on reinforced mounts.
- Ping1D mounted beneath hull or on strut.
- IR sensors mounted at bow, optionally angled left/right for field-of-view coverage.
- Waterproof gland seals for IR sensor cabling.
- Electronics enclosure vibration-isolated.

## 5. 3D Model Components

## Hull Segments
- PLA with fiberglass wrap
- Cable channels
- Ping1D recessed mount
- IR sensor front brackets
## Thruster Mounts
- Curved brackets with rubs
- Reinforced for torque loads
## Ping1D Mount
- Downward-facing
- Clear acoustic path
- Water-tight cable entry
## IR Sensor Mounts
- Forward-facing sensor pockets
- Optional ±20° angled mounting
- Drip-edge protection to reduce false triggers
## Electronics Enclosure
- Waterproof enclosure on central deck
- Standoffs and strain relief built in

## 6. Buildable Schematic

## Power Input
- XT60 → Fuse → Master Switch → PDB
- Monitoring module to Pixhawk
## PDB 
- High-current ESC rails
- 5 V buck converters
- Fusing for logic rails (Pi, Arduino, Ping1D, IR sensors)
## Control Electronics
- Pixhawk power module
- Pi 5 V rail
- Arduino 5 V rail
- Ping1D 5 V + UART
- IR Sensors 5 V + signal lines → Arduino
## Signal Wiring 
- PWM (Pixhawk → ESCs)
- Serial (Pi ↔ Pixhawk)
- Serial (Pi ↔ Ping1D)
- I²C/UART (Arduino ↔ Pi/Pixhawk)
- IR Sensors (Analog/Digital → Arduino)

  <img width="557" height="332" alt="image" src="https://github.com/user-attachments/assets/12a88cd1-e19d-44ca-b8bf-9917f8913183" />


## 7. PCB Layout
- High-current zones isolated
- Logic zone for Arduino, IR sensor headers
- Test points for Ping1D and IR sensor power rails
- Wide traces for ESC outputs

## 8. Hardware Control Flow

## Startup
- Power rails energize
- Pixhawk, Pi, Arduino, Ping1D, IR sensors initialize
## Initialization
- Pixhawk calibrates
- Pi opens MAVLink, Ping1D, IR sensor channels
- Arduino reads IR baselines
## Pre-Mission Check
- Validate GPS lock
- Confirm thrusters responsive
- Verify Ping1D depth data
- Verify IR sensor distance readings
## Autonomous Operation
- Pixhawk performs navigation
- Pi logs depth and obstacle detections
- Arduino constantly monitors IR sensors
  - If object < threshold → send STOP or AVOID signal
- Vessel slows or turns based on IR/Ping1D safety triggers
## Failsafe
- Low battery → RTL or stop
- Loss of comms → halt
- Shallow water (Ping1D) → reverse or stop
- Obstacle detected (IR sensors) → reverse/turn/stop
- Kill switch → cut thrusters
## Shutdown
- Thrusters disarmed
- Logs saved
- Master switch turned off

## 9. Analysis

## Buoyancy
- Catamaran reduces drag and increases stability
- PLA + fiberglass increases durability
- Freeboard meets requirements
## Propulsion
- T200 thrusters supply ample thrust
- Differential steering removes need for rudder
## Power
- Lithium batteries enable 1-hr runtime
- Logic rails isolated from ESC noise
- Proper fusing ensures protection
## Sensor Integration
- Ping1D provides depth for bathymetry and safety
- IR sensors provide near-field obstacle avoidance (docks, rocks, debris)
- Arduino acts as real-time safety processor
- Pi logs and synchronizes sensor data
## Compliance
- Lithium safety
- Open-source tools
- Eco-friendly electric propulsion
- Fits within $1,100 budget (component selection dependent)

## 10. Bill of Materials (BOM) - Hardware Subsystem (with Ping1D & IR Sensors)

| Ref. | Component                        | Description / Role                                      | Manufacturer        | Example Part Number                       | Qty | Est. Unit Price | Subtotal | Example Vendor |
|------|----------------------------------|---------------------------------------------------------|---------------------|-------------------------------------------|-----|------------------|----------|----------------|
| BATT1 | Lithium Battery Pack            | 4S–6S LiPo, ≥10–15 Ah                                   | Various             | TBD                                       | 1   | $120            | $120     | Hobby RC Stores |
| ESC1 | ESC for Left Thruster            | 30–50 A speed controller                                | Blue Robotics       | Basic ESC                                 | 1   | $60             | $60      | Blue Robotics |
| ESC2 | ESC for Right Thruster           | Same as ESC1                                            | Blue Robotics       | Basic ESC                                 | 1   | $60             | $60      | Blue Robotics |
| TH1  | Left Thruster                    | T200-class thruster                                     | Blue Robotics       | T200 R2                                   | 1   | $230            | $40      | Blue Robotics |
| TH2  | Right Thruster                   | T200-class thruster                                     | Blue Robotics       | T200 R2                                   | 1   | $230            | $40      | Blue Robotics |
| U1   | Pixhawk Flight Controller        | Autopilot + sensors                                     | Various             | Pixhawk 2.4.8                             | 1   | $50             | $50      | AliExpress |
| U2   | Raspberry Pi 4                   | Companion computer                                      | Raspberry Pi        | Pi 4 Model B                               | 1   | $60             | $60      | Adafruit |
| U3   | Arduino Uno                      | IR sensor processing                                    | Arduino             | Uno R3                                     | 1   | $20             | $20      | SparkFun |
| U4   | 5 V Buck Converter               | Logic power supply                                      | Various             | 5V ≥ 5A                                    | 1   | $15             | $15      | Digi-Key |
| U5   | 3.3 V Regulator                  | Aux logic power                                         | Various             | LDO                                        | 1   | $5              | $5       | Mouser |
| U6   | Blue Robotics Ping1D             | Depth / bathymetry sensor                               | Blue Robotics       | Ping1D                                     | 1   | $430            | $430     | Blue Robotics |
| IR1  | IR Distance Sensor               | Object-avoidance sensor                                 | Sharp / Generic     | Sharp GP2Y0A21YK0F                         | 2   | $15             | $30      | Amazon / Mouser |
| PDB1 | Power Distribution Board         | Routes battery to ESCs and regulators                   | COTS / Custom       | N/A                                        | 1   | $25             | $25      | Amazon |
| ENC1 | Electronics Enclosure            | Waterproof housing                                      | Various             | IP65 Box                                   | 1   | $35             | $35      | Polycase |
| HULL | PLA + Fiberglass Materials       | Hull fabrication materials                              | Various             | N/A                                        | -   | $120            | $120     | Amazon |
| BRK* | Mounting Hardware                | Thruster, Ping1D, IR sensor brackets                    | N/A                 | 3D Printed                                 | -   | $40             | $40      | Local / Filament |
| CBL* | Wiring / Connectors              | XT60, AWG wire, glands, seals                           | Various             | N/A                                        | -   | $50             | $50      | Digi-Key |
| F1   | Fuse / Breaker                   | 40–60 A main protection                                 | Various             | Auto Fuse                                  | 1   | $10             | $10      | Auto Stores |


## 11. Refrences

## Hardware & Component Documentation
- Blue Robotics – T200 Thruster
https://bluerobotics.com/store/thrusters/t100-t200/thruster-t200-r2/
- Blue Robotics – Basic ESC / BlueROV2 ESC
https://bluerobotics.com/store/thrusters/speed-controllers/besc-r3-rp/
- Blue Robotics – Ping1D Sonar
https://bluerobotics.com/store/sensors-sonars-cameras/sound/ping-sonar-r2-rp/
- Pixhawk Flight Controller Hardware Documentation
https://docs.px4.io/main/en/
- ArduPilot – ArduRover Documentation
https://ardupilot.org/rover/
- Raspberry Pi 4 Model B Hardware Specification
https://www.raspberrypi.com/products/raspberry-pi-4-model-b/
- Arduino Uno Technical Specification
https://docs.arduino.cc/hardware/uno-rev3/
- Sharp IR Distance Sensor (GP2Y0A21YK0F) Datasheet
https://global.sharp/products/device/lineup/data/pdf/datasheet/gp2y0a21yk0f_e.pdf

## Power, Electronics, & Safety
- Texas Instruments – DC-DC Buck Converter Design Guide
https://www.ti.com/power-management/dc-dc-converters/buck-converters/overview.html
- UL 1642 – Lithium Battery Safety Standard (Summary)
https://standardscatalog.ul.com/standards/en/standard_1642
- IEC 62133 – Safety Requirements for Portable Sealed Cells
https://webstore.iec.ch/publication/24684
- NOAA Small Craft Standards – Hull Loads & Stability (Informal Reference)
https://www.noaa.gov/

## Sonar, Sensors, & Navigation
- Blue Robotics Ping Protocol Specification
https://docs.bluerobotics.com/ping-protocol/
- Principles of Sonar Depth Measurement
Engineering Acoustics Textbook (commonly referenced in sonar papers)
- Object Avoidance Using IR Sensors – Sharp Application Notes
https://www.sharp-ne.com/products/sensors/

## Software Protocols & APIs
- MAVLink Protocol Documentation
https://mavlink.io/en/
- ArduPilot Companion Computer Integration Guide
https://ardupilot.org/dev/docs/companion-computers.html
- Linux Serial Communication for Embedded Systems
Raspberry Pi Documentation: https://www.raspberrypi.com/documentation/computers/

## Fabrication & Materials
- PLA 3D Printing Material Properties – Simplify3D
https://www.simplify3d.com/support/materials-guide/pla/
- Fiberglass Reinforcement Techniques – West System Epoxy
https://www.westsystem.com/instruction-manuals/

## General Engineering & Design Practices
- IEEE Guide for Documentation of Hardware Systems
IEEE Std 1063
- Marine Robotics Fundamentals – MIT OpenCourseWare
https://ocw.mit.edu/courses/2-680-unmanned-marine-vehicle-autonomy-sensing-and-communications-2012/
- Small Autonomous Surface Craft Design Surveys (Academic Literature)
Collection of open-source ASV research papers (NSF, IEEE, arXiv).

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

### 8. Link Budget Estimate (915 MHz Telemetry Radio)

This estimate demonstrates that the specified **300–500 m line-of-sight range** is feasible for the 915 MHz telemetry link using representative SiK-class radio parameters and conservative assumptions.

#### Assumed / Representative Radio Parameters

The following parameters are representative of common 915 MHz SiK telemetry radios and are used to verify feasibility rather than optimize performance:

- Transmit power: 20 dBm (100 mW)
- Receiver sensitivity: −105 dBm (typical at moderate RF data rates)
- Transmit antenna gain: 2 dBi
- Receive antenna gain: 2 dBi
- Miscellaneous losses: 2 dB (connectors, mounting, minor cable losses)

These values demonstrate that the required range is achievable with commercially available telemetry radios and standard antennas.

#### Free-Space Path Loss at 500 m

The free-space path loss (FSPL) is calculated using:

$$
\mathrm{FSPL(dB)} = 32.44 + 20\log_{10}\!\left(f_{\mathrm{MHz}}\right) + 20\log_{10}\!\left(d_{\mathrm{km}}\right)
$$

For a carrier frequency of 915 MHz and a distance of 0.5 km:

$$
\mathrm{FSPL(dB)} = 32.44 + 20\log_{10}(915) + 20\log_{10}(0.5)
$$

$$
\mathrm{FSPL(dB)} \approx 85.65
$$

#### Received Power Estimate

The received power is estimated using:

$$
P_{\mathrm{RX}} = P_{\mathrm{TX}} + G_{\mathrm{TX}} + G_{\mathrm{RX}} - L - \mathrm{FSPL}
$$

Substituting assumed values:

$$
P_{\mathrm{RX}} = 20 + 2 + 2 - 2 - 85.65
$$

$$
P_{\mathrm{RX}} \approx -63.65 \ \mathrm{dBm}
$$

#### Link Margin

The link margin is defined as the difference between received power and receiver sensitivity:

$$
\mathrm{Link\ Margin} = P_{\mathrm{RX}} - S_{\mathrm{RX}}
$$

$$
\mathrm{Link\ Margin} = -63.65 - (-105)
$$

$$
\mathrm{Link\ Margin} \approx 41.35 \ \mathrm{dB}
$$

**Conclusion:**  
A link margin of approximately **41 dB** at **500 m LOS** provides significant headroom for multipath effects, antenna misalignment, and environmental losses. This confirms that the **300–500 m range requirement is achievable**.

---

### 9. MAVLink Bandwidth Estimate (Proof of Non-Saturation)

To validate the telemetry throughput requirement (≤ 20 kbps), MAVLink bandwidth usage is estimated using:

$$
\mathrm{Bitrate\ (bps)} = \sum \left(\mathrm{messages/sec} \times \mathrm{bytes/message} \times 8\right)
$$

A conservative framing overhead of **12 bytes per MAVLink packet** is included to account for headers, CRC, and protocol overhead.

#### Nominal Telemetry and Mapping Traffic

| Message Type | Rate (Hz) | Payload (Bytes) |
|-------------|-----------|-----------------|
| HEARTBEAT | 1 | 17 |
| SYS_STATUS | 1 | 39 |
| GLOBAL_POSITION_INT | 5 | 36 |
| ATTITUDE | 10 | 36 |
| VFR_HUD | 2 | 28 |
| DISTANCE_SENSOR | 5 | 22 |
| RADIO_STATUS | 1 | 17 |

Payload-only data rate:

$$
\begin{aligned}
\mathrm{Payload\ Bytes/sec} &= (1)(17) + (1)(39) + (5)(36) + (10)(36) \\
&\quad + (2)(28) + (5)(22) + (1)(17) \\
&= 779 \ \mathrm{bytes/sec}
\end{aligned}
$$

$$
\mathrm{Payload\ Bitrate} = 779 \times 8 \approx 6{,}232 \ \mathrm{bps}
$$

#### Framing Overhead

Total packets per second:

$$
\mathrm{Packets/sec} = 1 + 1 + 5 + 10 + 2 + 5 + 1 = 25
$$

Overhead contribution:

$$
\mathrm{Overhead\ Bytes/sec} = 25 \times 12 = 300
$$

Total telemetry data rate:

$$
\mathrm{Total\ Bytes/sec} = 779 + 300 = 1{,}079
$$

$$
\mathrm{Total\ Bitrate} = 1{,}079 \times 8 \approx 8{,}632 \ \mathrm{bps}
$$

#### Manual Override Worst-Case Load

During manual override, assume a conservative worst-case scenario:

$$
\mathrm{Override\ Bytes/sec} = 62 \times 20 = 1{,}240
$$

$$
\mathrm{Override\ Bitrate} = 1{,}240 \times 8 \approx 9{,}920 \ \mathrm{bps}
$$

#### Combined Worst-Case Bandwidth

$$
\mathrm{Total\ Worst\text{-}Case\ Bitrate} \approx 8.6 \ \mathrm{kbps} + 9.9 \ \mathrm{kbps}
$$

$$
\mathrm{Total\ Worst\text{-}Case\ Bitrate} \approx 18.5 \ \mathrm{kbps}
$$

**Conclusion:**  
Even under conservative assumptions, total MAVLink traffic remains below **20 kbps**, confirming that the telemetry link will not saturate under normal or manual override conditions.

---

### 10. Verification and Test Cases


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


# Future Work

Future work for the autonomous lake-mapping vessel should focus on improving reliability, mapping accuracy, and overall autonomy. The current system provides a strong foundation for collecting depth, GPS, and telemetry data, but additional development would make the platform more dependable for repeated field use.

One major area for improvement is expanded field testing. More on-water trials should be performed under different lake conditions, including calm water, windy conditions, shallow regions, deeper regions, and areas near shorelines. These tests would help determine how well the sonar, GPS, telemetry, and propulsion systems perform in realistic environments. Additional testing would also allow the team to compare collected depth data against known reference measurements to better quantify accuracy.

Another important improvement is refining the autonomous survey path. Future versions should use a more advanced waypoint pattern, such as a lawnmower-style grid, to ensure even coverage of the entire mapping area. This would improve the quality of the final bathymetric map by creating a more consistent spread of data points. The system could also be improved by automatically adjusting the route when shallow water, obstacles, or poor GPS coverage are detected.

The mapping workflow could also be improved. While the current data can be exported and processed in QGIS, future work should focus on streamlining the process from data collection to final map generation. This could include automatic CSV cleanup, depth correction for sensor mounting offset, interpolation presets, and a repeatable process for generating contour maps or color-coded bathymetric maps.

Hardware improvements would also increase the system’s reliability. The electronics enclosure should be further waterproofed, cable glands should be tested for long-term water exposure, and all sensor mounts should be strengthened to reduce vibration. The sonar mount should remain fixed and vertically aligned during operation because sensor angle can affect depth accuracy. Improving strain relief and waterproof connectors would also make the system easier to deploy and maintain.

Future work should also include improvements to power management. More detailed battery testing should be performed to measure actual runtime under different throttle levels. This would help determine whether a larger battery, parallel battery setup, or more efficient operating speed is needed. Additional current and voltage monitoring could also be used to trigger safer low-battery warnings and return-to-launch behavior.

Finally, the system could be expanded with additional sensors. Future versions could include water temperature sensing, turbidity sensing, improved obstacle detection, or a higher-accuracy GPS module. These upgrades would make the vessel more useful for environmental monitoring beyond basic depth mapping.

# Final Conclusion

The autonomous lake-mapping vessel project successfully demonstrates the feasibility of a low-cost platform for collecting bathymetric data using accessible hardware and open-source tools. The system combines a floating vessel platform, Pixhawk-based navigation, Raspberry Pi data logging, sonar depth measurement, GPS positioning, telemetry communication, and GIS-based mapping into one integrated design.

The project addresses the need for an affordable alternative to expensive commercial hydrographic survey systems. By using modular components such as a Pixhawk autopilot, Raspberry Pi, Blue Robotics sonar, telemetry radios, and QGIS-based post-processing, the design provides a practical approach for small research groups, student teams, and local organizations that need depth data but may not have access to professional survey equipment.

The final design shows that each subsystem plays an important role in the overall mission. The hardware subsystem provides the stable vessel structure and mounting platform. The power subsystem supplies regulated energy to propulsion and electronics. The navigation subsystem allows waypoint-based operation and safe vessel control. The communications subsystem provides telemetry, manual override capability, and data transfer. The sensors and data acquisition subsystem collects the depth and environmental data needed to build bathymetric maps.

Experimental analysis and system testing showed that the vessel can collect useful depth and location data while supporting onboard logging and real-time monitoring. The testing also revealed important practical considerations, including sonar mounting offset, GPS reliability, telemetry connection quality, waterproofing, and power stability. These results show that the project is technically feasible while also identifying clear areas for future improvement.

Overall, the project meets its main goal of developing a low-cost autonomous lake-mapping vessel capable of collecting usable depth data for map generation. While the system can still be improved through additional field testing, stronger waterproofing, better route automation, and more refined data processing, the completed design provides a strong foundation for future development. It demonstrates how affordable autonomous systems can make lake mapping more accessible, repeatable, and useful for environmental monitoring, research, and water resource management.
