# My Thoughts on v2 Infrastructure

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

| Step | Function | Owner | POC Tech Stack | Production (AWS Pivot) |
| :--- | :--- | :--- | :--- | :--- |
| **0** | **Physical Edge** | **Zek / R&C / Grexx** | **zigbee2mqtt** (on Edge) | Managed IoT Gateway |
| **0b** | **Data Generator** | **Zek** | **C# On Azure** | Real Community Meters |
| **A** | **Price Oracle** | **Zek** | **C# WebJob Azure/AWS** (Scheduled) | AWS Equivalent |
| **1** | **MQTT Broker** | **Zek** | **HiveMQ Azure/AWS Cloud** + **IoT Hub** | HiveMQ Cloud + IoT Core |
| **2** | **Ingestion Service** | **Zek** | **C# WebJob Azure/AWS** | AWS Equivalent |
| **3** | **Integration Point** | **Zek (Owner) / Vlad (Consumer)** | **Postgres / TimescaleDB** | **AWS RDS (Managed)** |
| **4** | **VPP Logic (Split)** | Vlad | Node.js (AWS Lambda) |
