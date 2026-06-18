# Pharmaceutical-Grade Purified Water Treatment System
### Electrical & Automation Documentation | MEPL, Kathmandu

> **Status:** Active — currently in commissioning phase (Butwal, Nepal)  
> **Role:** Junior Automation Engineer — electrical documentation, instrumentation mapping, control panel design, and upcoming PLC programming  
> **Firm:** Mechatronics Engineering Pvt. Ltd. (MEPL)

---

## Project Overview

This project is a **control system upgrade** for an existing pharmaceutical purified water (PW) production facility. The system takes municipal or bore-well raw water through a multi-stage treatment chain — pre-treatment, dual-pass reverse osmosis, electro-deionization, and UV sterilization — to produce GMP/GEP-compliant purified water for internal pharmaceutical use.

The upgrade introduced a new automation layer: a Mitsubishi PLC, VFD-controlled pump zones, pneumatic valve actuation, and a distributed sensor network for real-time water quality monitoring and automatic rejection logic.

**Key system figures:**
- RO-1 capacity: 2,000 LPH | RO-2 capacity: 4,000 LPH
- RO storage: 3 × 5,000L tanks (15,000L total)
- 6 motors across 4 VFD-controlled zones
- 23 instruments mapped (sensors, actuators, controllers)
- 53 instrument nodes in full I/O mapping
- GMP/GEP compliant design with full Design Qualification (DQ) documentation

---

## Process Flow

```
Raw Water Tank
      │
  Sand Filter → Carbon Filter → Softener
                                    │
                              [TDS / pH / ORP check]
                                    │
                                  RO-1 (2000 LPH)
                              ┌─────┴──────┐
                           Reject        Permeate
                        (→ Raw Tank)   [EC check]
                                    │
                              RO Storage Tanks
                              (3 × 5000L)
                                    │
                    ┌───────────────┘
              Hot Water Tank ──→ RO-2 (4000 LPH)
              (Sanitization Loop)     │
                              ┌───────┴──────┐
                           Reject          Permeate
                        (→ Raw Tank)    [pH check]
                                    │
                                   EDI
                              ┌─────┴──────┐
                           Reject        Product
                        (→ Drain)      [EC check]
                                    │
                                  UV System
                                    │
                                 PW Tank
```

---

## Treatment Stages

**Pre-Treatment:** Sand filter (suspended solids), activated carbon filter (chlorine/organics), water softener (Ca²⁺/Mg²⁺ removal via automatic brine regeneration).

**RO-1 (First Pass):** 2,000 LPH system with pre-filter. pH and ORP monitored before entry — automatic rejection if out of spec. EC checked on permeate before entering RO storage tanks.

**RO Storage:** Three SS304 tanks, 5,000L each, with high/low level sensors. Feeds both RO-2 and the hot water sanitization circuit.

**RO-2 (Second Pass):** 4,000 LPH system. Further reduces EC and conditions pH for EDI inlet. Concentrate returns to raw water tank.

**EDI (Electro-Deionization):** Continuous CEDI unit (IONPURE LXM30HI-3), hot-water sanitizable, chemical-free operation. Reject discharged to drain; product passes UV before PW tank storage.

**Hot Water Sanitization Loop:** Shared circuit between RO-2 and EDI for periodic thermal sanitization.

---

## My Contribution

This was an active engineering internship task, not a standalone project. My responsibilities on this system specifically:

**Process Review & Logic Audit**
- Traced the full system from raw water intake to PW tank node by node
- Identified instrumentation logic issues in the original proposal
- Produced an independent process flow diagram for internal verification
- Optimised sensor placement and discussed instrument selection with the senior engineer

**Electrical Documentation (AutoCAD Electrical)**
- Documented all 6 motor circuits with VFD connections, MPCB ratings, and wire sizing
- Rated all instrumentation wiring — signal types, connection ports, and electrical matching
- Produced a 10-sheet schematic set covering: motor power, HPP circuits, power distribution, AC/DC control supply, PLC I/O, button mapping, VFD relay logic, and pneumatic outputs

**Control Panel Design**
- Sized the SS304 panel enclosure around HMI unit dimensions and DIN rail component layout
- Planned internal component arrangement prioritising heat flow and cooling clearance
- Specified component ratings: MCBs, MPCBs, 24V DC supply

**Instrumentation & Sensor Mapping**
- Mapped all 53 instrument nodes across the full process chain
- Defined signal types, connection ports, control zones, and actuation modes for each node
- Aligned sensor placement to PLC I/O architecture

**Upcoming (Commissioning Phase)**
- Site visit to Butwal to oversee panel wiring and connection verification
- Voltage drop calculations and wire diameter confirmation
- VFD cable routing to minimise EMF broadcasting across the plant floor
- Mitsubishi FX3SA-60MR-CM PLC programming (assigned independently)

---

## Motor & Power System

### Power Devices

| S.N | Motor ID | Description | Rating (kW) | Control | Connection |
|-----|----------|-------------|-------------|---------|------------|
| 1 | M001 | Initial Pump P1 | 1.0 | VFD | U, V, W |
| 2 | M002 | Initial Pump P2 (backup) | 1.0 | VFD | U, V, W |
| 3 | M003 | HPP 1 | 2.2 | VFD | U, V, W |
| 4 | M004 | HPP 2 (backup) | 2.2 | VFD | U, V, W |
| 5 | M005 | HPP 3 (RO-2) | 4.0 | VFD | U, V, W |
| 6 | M006 | Distribution Pump | 1.5 | VFD | U, V, W |

> **Note:** M001/M002 and M003/M004 are redundant pairs. Only one runs at a time; the other is on standby. VFD connection in Zones 1 and 2 is currently manual — operators physically reconnect the VFD to the selected motor before startup. This is a known commissioning-phase limitation.

### Wire Sizing

| # | Run | Load (A) | Required (mm²) | Installed (mm²) |
|---|-----|----------|----------------|-----------------|
| 1 | M001/M002 → VFD | 2.5 | 0.75 | 1.5 |
| 2 | M003/M004 → VFD | 5.0 | 1.0 | 1.5 |
| 3 | M005 → VFD | 9.0 | 1.0 | 1.5 |
| 4 | M006 → VFD | 3.5 | 0.75 | 1.5 |
| 5 | VFDs → MCCB | 22.0 | 4.0 | 6.0 |

### Protection Components

| Component | Rating |
|-----------|--------|
| MCB | 24A |
| MPCB 1 | 2.2A |
| MPCB 2 | 4.8A |
| MPCB 3 | 8.5A |
| MPCB 4 | 3.4A |
| 24V DC Supply | 10A |

---

## Instrumentation

### Connection Mapping (23 nodes)

| S.N | Tag | Part | Signal | Ports | Connections | Description |
|-----|-----|------|--------|-------|-------------|-------------|
| 1 | TDS 1 | SUP-TDS210-B | Analog Out | 4-Wire | V+, V−, P+, P− | 24V DC with independent meter display |
| 2 | TDSC 1 | — | Controller | — | — | — |
| 3 | pH 1 | SUP-PH6.5 | Analog Out | 4-Wire | V+, V−, P+, P− | 24V DC with independent meter display |
| 4 | pHC 1 | — | Controller | — | — | — |
| 5 | ORP 1 | SUP-pH5013A | Analog Out | 4-Wire | V+, V−, P+, P− | 24V DC with independent meter display |
| 6 | ORPC 1 | — | Controller | — | — | — |
| 7 | PBV 2 | Solenoid Valve | Digital Out | 2-Wire | 24V PLC, 0V return | Pneumatic valve via 24V solenoid coil |
| 8 | PBV 1 | Solenoid Valve | Digital Out | 2-Wire | 24V PLC, 0V return | Pneumatic valve via 24V solenoid coil |
| 9 | LPS 1 | SUP-Y210 | Digital In | 2-Wire | Electrical Meter | Low pressure switch |
| 10 | HPS 1 | SUP-Y210 | Digital In | 2-Wire | Electrical Meter | High pressure switch |
| 11 | TDS 2 | SUP-TDS210-B | Analog Out | 4-Wire | V+, V−, P+, P− | 24V DC with independent meter display |
| 12 | TDSC 2 | — | Controller | — | — | — |
| 13 | PBV 3 | Solenoid Valve | Digital Out | 2-Wire | 24V PLC, 0V return | Pneumatic valve via 24V solenoid coil |
| 14 | PBV 4 | Solenoid Valve | Digital Out | 2-Wire | 24V PLC, 0V return | Pneumatic valve via 24V solenoid coil |
| 15 | LPS 2 | SUP-Y210 | Digital In | 2-Wire | Electrical Meter | Low pressure switch |
| 16 | HPS 2 | SUP-Y210 | Digital In | 2-Wire | Electrical Meter | High pressure switch |
| 17 | TDS 3 | SUP-TDS210-B | Analog Out | 4-Wire | V+, V−, P+, P− | 24V DC with independent meter display |
| 18 | TDSC 3 | — | Controller | — | — | — |
| 19 | PBV 5 | Solenoid Valve | Digital Out | 2-Wire | 24V PLC, 0V return | Pneumatic valve via 24V solenoid coil |
| 20 | PBV 6 | Solenoid Valve | Digital Out | 2-Wire | 24V PLC, 0V return | Pneumatic valve via 24V solenoid coil |
| 21 | PS 1 | SUP-Y210 | Digital In | 2-Wire | — | Standard pressure switch contact |
| 22 | FLS 1 | SUP-ZPM | Digital In | 2-Wire | — | Float/flow switch level contact |
| 23 | EC | SUP-TDS210-B | Analog Out | 4-Wire | V+, V−, P+, P− | Conductivity controller, 24V auxiliary |

### Full I/O Map (53 nodes) — Selected entries

| S.N | Instrument | Input | Output | Type | Display | Control Zone |
|-----|------------|-------|--------|------|---------|--------------|
| 1 | TDS 1 | Raw water tank | First Filter | Sensor | Control | TDS I |
| 2 | pH 1 | Raw water tank | First Filter | Sensor | Control | TDS I |
| 3 | ORP 1 | Raw water tank | First Filter | Sensor | Control | TDS I |
| 8 | PBV 1 | Pump 1/2 | Drain | Actuator | Control | TDS I |
| 9 | PBV 2 | Pump 1/2 | First Filter | Actuator | Control | TDS I |
| 10 | LPS 1 | First Filter | M003/M004 | Sensor | HP Drive | HP Control I |
| 11 | HPS 1 | M003/M004 | RO-I | Sensor | HP Drive | HP Control I |
| 15 | TDS 2 | RO-I | RO Tank | Sensor | Control | TDS II |
| 19 | PBV 3 | RO-I | Drain | Actuator | Control | TDS II |
| 20 | PBV 4 | RO-I | RO Tank | Actuator | Control | TDS II |
| 31 | TDS 3 | RO-II | EDI | Sensor | Control | TDS III |
| 35 | PBV 5 | RO-II | Drain | Actuator | Control | TDS III |
| 46 | EC 1 | EDI | UV | Sensor | EDI Power | EDI Power |
| 48 | DVTV | UV | PW Tank | Actuator | EDI Power | EDI Power |

> Full 53-node I/O map available in `/docs/NPL_Instruments.pdf`

---

## Documentation Package

| Document | Format | Description |
|----------|--------|-------------|
| Electrical Schematic (10 sheets) | AutoCAD Electrical / PDF | Motor circuits, power distribution, PLC I/O, VFD logic, pneumatic outputs |
| Pneumatic & Logic Schematic | QElectroTech | Full pneumatic circuit with ladder logic overlay |
| Connection Mapping | PDF | 23-node sensor/actuator wiring table |
| Instrument I/O Map | PDF | 53-node full process instrument map |
| Power Devices & Wire Sizing | PDF | Motor ratings, wire sizing calculations, protection component ratings |
| Control Panel Render | Image | DIN rail layout, HMI placement, thermal planning |

---

## Tools & Technologies

| Domain | Tools / Components |
|--------|-------------------|
| Electrical CAD | AutoCAD Electrical, QElectroTech |
| PLC | Mitsubishi FX3SA-60MR-CM |
| HMI | Mitsubishi GS2107-N |
| Drives | 4 × VFDs (Mitsubishi) |
| Sensors | Supmea SUP-TDS210-B, SUP-PH6.5, SUP-pH5013A, SUP-Y210, SUP-ZPM |
| EDI | IONPURE LXM30HI-3 (hot-water sanitizable) |
| RO Membranes | DOW/DuPont 8040 (hot-sanitizable, 1000 LPH each) |
| Panel Material | SS304, Jindal Steel 2mm |
| Documentation | GMP Design Qualification (DQ) package produced |

---

## Current Status

The system is currently in the **commissioning phase**. Electrical schematics and documentation are complete. Panel fabrication is underway. Site visit for wiring verification, voltage drop confirmation, and VFD cable routing is scheduled. PLC programming (Mitsubishi ladder logic) is assigned and pending commissioning readiness.

---

*Authored by Sambridda Ranabhat | Mechatronics Engineering Pvt. Ltd. (MEPL) | 2026*
