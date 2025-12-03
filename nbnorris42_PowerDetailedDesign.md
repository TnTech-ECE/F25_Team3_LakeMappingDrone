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
- 5 V / 10 A buck converter  
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
| 5 V 10 A Buck Converter        | DROK         | 5V-10A            | Amazon     | Same as Mfr P/N     | 1   | $17.49 | [https://www.amazon.com/](https://www.amazon.com/DROK-Converter-5-3V-32V-Regulator-Transformer/dp/B078Q1624B/ref=sr_1_1_sspa?dib=eyJ2IjoiMSJ9.cKfwTrlAJh7wRVi5JMftbdF1j19UIxiKRCgAj6Dbb239w02Sz20OrDlTUBJRvMn8Ayv8srllGoDqDb56aj6QN4kxiMylhlQcrlHGD6Qbpzn-fmrnvPeHxqpHS16NOKZsLi2ymDf_w9sr7Omury6bclfZGdoHlz_iqf9C8JIwuj2YrbcoOtmQUxab9hD-8svzOM2Hb4ecMJuVzS7dpl2xbPTsEGVh6fmc_YD8lxdiOgk.O7brV1fXevM-OeOs6yuLmxxhrY3OoofALSrpcwFfVgE&dib_tag=se&keywords=drok%2Bbuck%2Bconverter&qid=1764783882&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1) |
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
