# Hypha Energy: v2 Infrastructure Flow & Responsibility Matrix

---

## 1. Project Context & Reference
For the core logic regarding energy accounting and the smart contract architecture this infrastructure supports, please refer to the technical documentation here:
* **[Energy System Readme (GitHub)](https://github.com/hypha-dao/hypha-web/blob/energy-accounting/packages/storage-evm/contracts/ENERGY_SYSTEM_README.md)**

---

## 2. The Execution Split
* **Zek:** Managing the **Infrastructure Foundation** (Steps 0-3). Coordinating with **R&C and Grexx** at the physical edge and handling the data flow (Telemetry, Price Oracles, and Ingestion) using **C# Worker Services**.
* **Vlad:** Managing the **Value & Control Engine** (Steps 4-7). Picking up the data from the DB to drive the **VPP Fair-Split** and the **EMS Control Logic**, leading to the final blockchain settlement.

---

## 3. Responsibility Matrix: v2 POC vs. Production

| Step | Function | Owner | POC Tech Stack (Azure) | Production (AWS Pivot) |
| :--- | :--- | :--- | :--- | :--- |
| **0** | **Physical Edge** | **Zek / R&C / Grexx** | **zigbee2mqtt** (on Edge) | Managed IoT Gateway |
| **0b** | **Data Generator** | **Zek** | **C# WebJob** | Real Community Meters |
| **A** | **Price Oracle** | **Zek** | **C# WebJob** (Scheduled) | AWS Equivalent |
| **1** | **MQTT Broker** | **Zek** | **HiveMQ Cloud** + **IoT Hub** | HiveMQ Cloud + IoT Core |
| **2** | **Ingestion Service** | **Zek** | **C# WebJob** | AWS Equivalent |
| **3** | **Integration Point** | **Zek (Owner) / Vlad (Consumer)** | **Postgres / TimescaleDB** | **AWS RDS (Managed)** |
| **4** | **VPP Logic (Split)** | Vlad | Node.js (AWS Lambda) | AWS Lambda |
| **5** | **EMS Logic (Control)** | Vlad | Node.js (AWS Lambda) | AWS Lambda  |
| **6** | **Blockchain** | Vlad | Base Chain (Solidity) | Base Mainnet (KMS) |
| **7** | **Debt Settlement** | Vlad | Base / Smart Contracts | Embedded Wallets |
| **M** | **Monitoring** | **Zek / Vlad** | **Grafana Cloud** | Grafana Enterprise |

---

## 4. Integration Point & Data Flow
The database serves as the decoupled handover point. The following data flow and retention policies are established:

* **Ingestion (10s Frequency):** MQTT messages are captured by the C# Ingestion Service and written to the `raw_readings` table.
* **Summarization (15m Intervals):** A scheduled process summarizes the raw data into the `interval_readings` table.
* **Handover:** Vlad's logic triggers from the `interval_readings` and `price_points` tables.

### Database Schema Specs
* **raw_readings (TimescaleDB):** 10-second high-fidelity data. *Retention: 90 days.*
* **interval_readings (TimescaleDB):** 15-minute summarized data. *Retention: Permanent.*
* **price_points (Postgres):** Market price data fed by the C# Price Oracle. *Retention: Permanent.*
* **settlement_batches (Postgres):** Calculated results from the VPP fair-split. *Retention: Permanent.*

---

## 5. Project Scope & Setting
For this phase, we are building for a **Simulated Community**. The **C# Data Generator** will be configured for **X members** and **Y shared assets** (including **solar panels and batteries**) to provide a scalable baseline for verifying split-math and control loops.
