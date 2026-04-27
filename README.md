# BESS Coordination for TSO-DSO Operations

**Service:** BESS Coordination for TSO-DSO Operations  
**Document Type:** Technical Manual & Service Specification  
**Author:** Slovenian DSO Elektro Gorenjska d.d. (ELGO), in collaboration with COMS and JSI  
**Version:** 1.0  
**Last Updated:** 18 Feb 2026  

---

# 1. Business Context & Definitions

As Europe accelerates its clean energy transition under the European Green Deal and EU Battery Regulation (2023/1542), Battery Energy Storage Systems (BESS) are becoming essential for grid stability, renewable integration, and decarbonization, with the EU targeting 200 GW of storage by 2030. Slovenia aligns with these goals through national green energy plans promoting large-scale BESS deployment to support variable renewables and enhance TSO-DSO coordination.

This service addresses emerging challenges in the Gorenjska region, where Elektro Gorenjska (ELGO, DSO) interfaces with ELES (TSO). Planned BESS installations, primarily 5–20 MW with flexibility for larger or smaller sizes, near DSO-TSO transformers or Razdelilna Transformatorska Postaja (RTP) sites risk unintended impacts on the TSO network through simultaneous or misaligned activations, exacerbating classic TSO-DSO issues such as over/under voltages, especially when combined with industrial demand fluctuations.

The service creates a unified TSO-DSO network model by interconnecting existing Common Information Model (CIM) files from ELES (TSO) and ELGO (DSO) at shared nodes, enabling accurate simulation of BESS effects. In Phase 1, it analyzes disturbance propagation from DSO flexibility activations (DER/BESS) to the TSO grid. In Phase 2, it develops coordinated control strategies to harness BESS for stabilization, dampening disruptive effects, balancing demand/response, and preventing issues such as blackouts, transforming potential problems into grid assets.

This two-phase approach enhances operational efficiency, supports EU market participation rules for storage such as the Clean Energy Package, and improves reliability metrics while complying with grid codes. The service targets deployment around key Gorenjska TSO connections, with modular scalability to other regions.

## Key Terms

| Term | Definition |
|---|---|
| Battery Energy Storage System (BESS) | Grid-connected battery systems (5–20 MW typical) for storing/releasing energy, enabling flexibility services such as frequency regulation and peak shaving |
| Common Information Model (CIM) | IEC 61970/61968 standard for exchanging power system data, used here to integrate TSO/DSO network models |
| Distribution Transformer Substation (RTP) | Primary HV/MV substation (typically 110/20 kV) at TSO-DSO interconnection points |
| Transformer Substation (TP) | Secondary MV/LV substation (typically 20/0.4 kV) supplying end-users |
| Transmission System Operator (TSO) | ELES, managing Slovenia's high-voltage transmission network |
| Distribution System Operator (DSO) | ELGO, operating the medium/low-voltage distribution grid |
| TSO-DSO Coordination | Collaborative management of flexibility resources across boundaries |
| Flexibility Management | Real-time control of DER/BESS to provide grid services |
| SAIFI | Average number of interruptions experienced per customer |
| SAIDI | Average outage duration per customer |
| High Voltage (HV) | Voltage for transmission |
| Medium Voltage (MV) | Voltage for distribution |
| Low Voltage (LV) | Voltage for end-users |

---

# 2. ELES – ELGO Coordination Context

This service is intended for coordination between ELES (TSO) and ELGO (DSO) in the Gorenjska region, focusing on RTP sites with TSO interconnections where BESS deployments are planned.

In current operations, ELGO manages MV distribution from RTP substations (110/20 kV), while ELES oversees HV transmission stability. Large-scale BESS installations near RTP transformers or HV/MV boundaries could trigger network stability issues such as over/under voltages and reactive power imbalances during simultaneous or poorly coordinated activations, propagating disturbances from DSO feeders into the transmission grid.

ELGO currently detects local disturbances via SCADA/DMS but lacks visibility of TSO impacts, while ELES monitors system-wide conditions via EMS with limited DSO granularity. Without integrated simulation tools, operators rely on conservative limits.

This service introduces a unified CIM-based model to support impact assessment, preventive alerting, and coordinated control strategies. It aims to identify mutual benefits such as congestion relief on the DSO side and stability support on the TSO side.

---

# 3. Problem Statement

The objective is to investigate the feasibility of a unified TSO-DSO network model for BESS impact analysis and coordinated control strategies in the Gorenjska region.

The service combines CIM XML files from ELES and ELGO at RTP interconnection points with hypothetical BESS parameters (5–20 MW charge/discharge profiles) and actual network device data.

## Research Focus

- Phase 1: Impact Assessment of misaligned BESS activations causing voltage and reactive power issues  
- Phase 2: Control Research for optimizing BESS dispatch for grid stabilization  

## Methodology

Offline simulations using Python and Pandapower on integrated CIM models with historical data for validation.

## Key Challenges

- Validation reliability due to limited real BESS deployments  
- Model fidelity dependent on accurate network parameters  

## Scalability

Initial focus is Gorenjska region; future expansion depends on research validation.

---

# 4. Data Description

The service relies on CIM network models, historical data, and hypothetical BESS scenarios.

## Table 1: Data Sources Summary

| Data Type | Source | Format | Size Estimate | Resolution |
|---|---|---|---|---|
| TSO Network Model | ELES CIM XML | XML/RDF | in review | Static topology |
| DSO Network Model | ELGO CIM XML | XML/RDF | in review | Static topology |
| Historical Loads/Events | ELGO SCADA/DMS | CSV/SQL | ~2–5 GB (2 years) | 1–15 min |
| BESS Hypotheticals | Research scenarios | JSON/CSV | / | Event-based |
| Device Parameters | CIM extensions | XML attributes | Included | Static |

## Table 2: CIM XML Network Model Classes

| CIM Class | Description | Key Attributes | Example |
|---|---|---|---|
| Substation | Physical grouping | name, VoltageLevels | RTP_Gorenjska_110kV |
| VoltageLevel | Voltage configuration | nominalVoltage | 110kV_HV |
| PowerTransformer | HV/MV transformer | ratedS, vectorGroup | 100 MVA |
| ACLineSegment | Transmission line | r, x, b | 10 km feeder |
| EnergyConsumer | Load | p, q | 5 MW load |
| BatteryEnergyStorage | BESS | chargeRate | 10 MW |

---

# 5. Analytics, Scope & Update Frequency

## Temporal Scope

Uses ~2 years of data starting from 2024.

## Update Frequency

- Full model refresh weekly  
- Partial updates on topology changes  

## Analytics Pipeline

1. Merge CIM models  
2. Apply load/BESS scenarios  
3. Evaluate voltages and flows  
4. Identify worst-case scenarios  

## Outputs

- Technical reports  
- Voltage/loading profiles  
- Risk/stability assessments  

All analytics are performed offline.

---

# 6. Evaluation Protocols & Metrics

Evaluation focuses on simulation accuracy and usefulness.

## 6.1 Data Usage Protocol

- Uses 2024–2025 data  
- Deterministic simulations  
- Parameter sweeps for BESS scenarios  

## 6.2 Data Gaps

- 70/30 calibration-validation split  
- Incomplete data excluded  

## 6.3 Evaluation Metrics

| Metric | Description | Target |
|---|---|---|
| Voltage RMSE | √(mean squared voltage error) | <5% |
| Reactive Power Error | Mean absolute % error | <10% |
| Propagation Threshold | BESS power causing >2% deviation | Scenario-based |
| Worst-case Coverage | Match with expert risks | >80% |
| Scenario Utility | Expert rating (1–5) | ≥4.0 |

---

# 7. Deliverables & Submissions

Internal research service, no external deliverables.

## 7.1 Deliverable Report

Single report including:

1. Methodology  
2. Research findings  
3. Evaluation results  
4. Recommendations  
5. Reproducibility package  

## 7.2 Technical Documentation

Includes:

- Jupyter notebooks  
- Python modules  
- Data specifications  

## Deployment Considerations

No production deployment planned.  
Future extensions depend on validation and data integration readiness.  
Final delivery aligned with EnerTEF project timeline (end 2027).
