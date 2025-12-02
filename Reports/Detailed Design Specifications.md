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

<img width="557" height="332" alt="image" src="https://github.com/user-attachments/assets/12a88cd1-e19d-44ca-b8bf-9917f8913183" />

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


