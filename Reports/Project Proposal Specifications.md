Lake Mapping Drone

## Introduction

Understanding the depth and terrain of lakes and other bodies of water is essential for environmental monitoring, infrastructure planning, and recreational management. The conventional method of underwater mapping relies on specialized boats, lots of manpower, and expensive sonar equipment. This makes it unattainable for smaller organizations and research groups. As a result, there is a growing need for affordable, autonomous systems capable of collecting and transmitting in-depth data to produce detailed underwater maps. 

Our project proposes the design and development of an autonomous boat that will measure depth and send data to be able to generate a detailed map of a body of water. The system will incorporate low-cost sonar technology, autonomous navigation, and wireless data transmission. With this system, it will be user friendly and scalable. By taking away the need for constant human operation and cheaper equipment, the project’s focus is to make underwater mapping more practical and accessible to a vast variety of users.

This proposal will outline the background and motivation for the project, clearly define the problem, describe the system requirements, and present the proposed design approach. In addition, it will discuss anticipated challenges, testing methods, and the broader impact of the solution.

## Formulating the Problem

This problem affects environmental and local governments who need accurate lake and reservoir data. This data can then be used for environmental monitoring, flood management, and infrastructure planning. It also impacts fisheries, recreational and conservation groups. The current lake mapping methods tend to be expensive and require well-trained operators. So, a low cost, autonomous solution would allow for more frequent and efficient data collection without the high cost and specialized skills. This would allow lake mapping to become more expandable and accessible. While commercial sonar and autonomous boats do exist, simply the cost and specific skill set needed to use them limit the availability greatly. Consumer sonar systems may provide depth readings but lack autonomy, data mapping, and integration capabilities. So, a custom-built solution can guarantee affordability, versatility in differing environments, and control over the capacity of the solution’s growth.

### Background

Effective depth mapping relies on sonar, GPS, and GIS technologies working together to generate accurate underwater maps [1]. However, mapping conditions are rarely ideal: wind, waves, and surface turbulence can introduce noise, while GPS accuracy may degrade near shorelines or under canopy cover. Although sonar is less affected by water clarity than optical sensors, shallow waters present specific challenges, such as interference from surface echoes and reduced resolution due to limited range [2].

Current commercial solutions often face limitations, including reduced accuracy in shallow environments, high cost, and lack of modularity for adapting to different research needs. These factors make them less accessible to smaller research groups and less versatile for studying properties beyond depth, such as stream flow.

Our project addresses these challenges by developing a modular, low-cost, and user-friendly mapping platform. While its primary focus is lake bottom depth, the design emphasizes adaptability, enabling researchers to expand into diverse water applications without being restricted by cost or technical barriers. This project is specifically targeted at inland lakes/small rivers and small-scale research environments, rather than large-scale hydrographic surveys, thereby focusing its scope on cost-effective and adaptable solutions for academic and field research settings.

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

- 	**UL 2054** – Household and Commercial Batteries – This standard applies to lithium-ion and rechargeable batteries, ensuring safe design, assembly, and protection from fire, explosion, and leakage. This applies to the project’s multiple battery systems.
- 	**IEC 60529** – Ingress Protection (IP) Ratings – This standard specifies the protection level of electrical enclosures against dust and water, which applies to protecting boat electronics and sensors.
- 	**CC 47 CFR Part 15** – Radio Frequency Devices – This standard applies to wireless transmitters and receivers used for RC control and autonomous communication systems.
- 	 **SO 8383:1985** – Ships and Marine Technology: Small Craft Stability – This standard applies to design and testing considerations for small watercraft stability, relevant to the catamaran hull design.
- 	 **Tennessee Wildlife Resources Agency (TWRA) Boating Regulations** – Local boating regulations govern safe operation of RC/autonomous surface vessels in Tennessee public waters.
- 	 **TVA (Tennessee Valley Authority) Waterway Regulations** – If operated on TVA-managed lakes, additional navigation and use regulations apply.
- 	 **EPA Clean Water Act Compliance** – Ensures the vessel does not discharge pollutants, chemicals, or fuels into waterways.

## Survey of Existing Solutions

This section summarizes current approaches and products used to measure water depth and water flow. We group solutions by sensing method and platform, then note capabilities and trade-offs relevant to an autonomous, low-cost survey vessel.

**1. Acoustic Doppler Current Profilers (ADCPs) for Flow/Discharge**

ADCPs are the water-industry standard for measuring velocity profiles and computing discharge. U.S. Geological Survey (USGS) guidance details validated procedures for moving-boat ADCP measurements and associated QA/QC practices. U.S. Geological Survey Publications
**Representative systems**
- **Teledyne RD Instruments (RDI) StreamPro** – portable ADCP designed for small streams; provides real-time discharge with built-in QA/QC workflows. Typical operating depths are on the order of decimeters to a few meters, targeting small channels. teledynemarine.com+1
- **SonTek (Xylem) RiverSurveyor RS5** – compact ADCP intended for discharge in rivers, streams, and canals; often paired with a small tow board or micro-USV and optional RTK positioning. YSI+2YSI+2
**Relevance to the project:**
 ADCPs offer high-fidelity flow/velocity data and well-established methods but increase cost, power draw, and integration complexity compared with simpler single-beam depth sensors. U.S. Geological Survey Publications

**2. Echo Sounders for Depth/Bathymetry**

Single-beam echo sounders provide vertical depth at a point; multibeam systems map swaths for faster coverage but at higher cost and integration requirements.
- **Single-beam (e.g., Blue Robotics Ping/Ping2)** – low-cost echosounders (≈100 m range, ~25° beam) commonly used on small USVs for bathymetric point soundings and as altimeters; open-source interfaces ease integration. Blue Robotics+2Blue Robotics+2
- **Survey-grade single-beam (e.g., CEESCOPE with RTK GNSS)** – integrated echo sounder + GNSS/RTK receivers for centimeter-level bottom mapping in professional workflows. ceehydrosystems.com
- **Multibeam packages (case example)** – ASVs equipped with multibeam sonars and RTK-INS (e.g., SBG Ekinox-D) enable wide-swath mapping with precise positioning and motion compensation. SBG Systems
**Relevance to the project:**
 A single-beam transducer is typically sufficient for channel profiling and proof-of-concept bathymetry; multibeam improves coverage but requires higher budget and more advanced navigation/attitude sensing. Blue Robotics+1

**3. Unmanned Surface Vessels (USVs) / Autonomous Survey Platforms**

Commercial USVs integrate propulsion, navigation, and payload bays for sonar and GNSS/INS.
- **Seafloor Systems EchoBoat-160** – purpose-built hydrographic USV supporting single-/multibeam payloads; marketed for efficient, crew-reduced surveys. Seafloor Systems Inc
- **Tersus GNSS “TheDuck”** – compact USV with single-beam echo sounder aimed at bathymetric surveys. tersus-gnss.com
**Relevance to the project:**
 Commercial USVs offer turn-key reliability but at significant cost. A student-built platform can tailor sensors and autonomy to the specific use case at lower price, with added integration effort. Seafloor Systems Inc+1

**4. Industry Practices and Alternative Flow Instruments**

Beyond ADCPs, vendors such as OTT Hydromet offer mechanical and acoustic flow meters and fixed stations for long-term discharge monitoring; mobile Doppler systems like OTT Qliner2 have specified accuracy and profiling ranges for wading and boat deployments. OTT+1

**5. Open-Source / Academic Efforts**

Recent literature and maker ecosystems describe low-cost autonomous surface vehicles that integrate commodity echosounders, GNSS, and open autopilots for bathymetry/monitoring—highlighting feasibility for budget-constrained applications, albeit with varying levels of validation versus professional gear. ResearchGate

## Measures of Success

To evaluate the accuracy and consistency of the system’s data, we will compare our collected measurements with reference data previously gathered and validated by professionals in the field. An error threshold will be established at the start of the project and gradually narrowed as development progresses, ensuring continuous improvement toward the final product.
The vessel will use sonar to collect multiple depth measurements and sensors to monitor water flow. This data will be transmitted remotely to a processing program, which will perform real-time analysis by comparing the collected data against the reference dataset. The program will also generate charts and graphs to highlight deviations.

Based on these analyses, iterative adjustments will be made to both hardware and software designs to reduce deviations between the collected and reference data. Project success will be demonstrated by achieving accuracy levels within the defined error thresholds and maintaining consistent performance across varied testing conditions.

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

**Supervisor**
Christopher Johnson

### Timeline

[Timeline in Progress]


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

[3] DIY sonar transducer

[4] How to Connect, Read, and Process Sensor Data on Microcontrollers – A Beginner's Guide


## Statement of Contributions

Jackson Hamblin - Existing Solutions

Ian Hanna - Background, Specific/Broader Implications

Nathan Norris - Budget, Timeline 

Brady Nugent - Introduction, Formulating the Problem

Ryan Thomas - Specifications/Constraints, Measures of Success

