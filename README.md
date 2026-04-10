# Real-Time Air Quality Monitoring System (AQMS)

An end-to-end Internet of Things (IoT) solution designed to bridge the gap between raw environmental data and actionable health advisories. This system leverages a high-performance edge core and a deterministic safety logic engine to provide sub-second latency visualization of indoor air quality.

## 🚀 Key Features
* **Edge-Native Architecture**: Powered by **Zephyr RTOS** on a **Nordic nRF52840** (ARM Cortex-M4F), providing deterministic multi-sensor polling without cloud dependency.
* **Multi-Modal Sensor Fusion**: Aggregates high-fidelity data from three specialized sensors to monitor CO2, TVOC, PM2.5, PM10, and Humidity.
* **Robust ETL Pipeline**: Features a sub-10ms **Regex-based ingestion engine** and an advanced **brace-counting algorithm** for reconstructing fragmented JSON packets.
* **ASHRAE-Compliant Safety Logic**: A deterministic engine that classifies environmental severity into four tiers: OK, WARN, ALERT, and CRITICAL.
* **Real-Time Dashboard**: A **Streamlit-based** analytics interface delivering visual updates every 1,000ms.

---

## 🛠 Hardware Architecture

The system utilizes a **split-rail power topology** to ensure signal integrity and manage divergent voltage requirements across the sensor array.

### Components
* **MCU**: Nordic nRF52840 (64 MHz, 1MB Flash, 256KB RAM).
* **CO2 Sensor**: Sensirion **SCD41** (Photoacoustic principle, ±30 ppm accuracy).
* **PM Sensor**: Sensirion **SPS30** (Laser scattering with internal fan for constant airflow).
* **TVOC Sensor**: AMS **CCS811** (MOX-based sensing with eCO2 cross-validation).
* **Power**: **MB102 module** providing isolated 3.3V (Logic) and 5V (SPS30 Fan) rails.

### Protocol Partitioning
| Sensor | Interface | Protocol Rationale |
| :--- | :--- | :--- |
| **SCD41** | I2C (400 kHz) | Native compatibility and simple bus configuration. |
| **CCS811** | I2C (400 kHz) | Shared 3.3V logic rail; unique 7-bit addressing. |
| **SPS30** | UART (115200) | Isolated from I2C to avoid fan-induced electrical noise. |

---

## 💻 Software Methodology

### ETL Pipeline & Analysis
The software infrastructure follows a six-stage sequential flow to ensure data integrity:
1.  **Data Ingestion**: Continuous ingestion via Serial and CoAP interfaces.
2.  **Regex Sanitization**: Strips ANSI escape codes and line noise in <10ms.
3.  **Brace-Counting Recovery**: Reconstructs fragmented JSON payloads with 99.8% success.
4.  **Time-Series Analysis**: Uses **Pandas** for rolling averages, delta calculations, and staleness checks.
5.  **Safety Logic Mapping**: Evaluates fused sensor data against **ASHRAE Standard 62.1** and WHO guidelines.
6.  **Visualization**: Streamlit reactive rendering for real-time frontend updates.

---

## 📊 Performance Evaluation
* **Latency**: Total mean pipeline execution time of **29.7 ms**, well within the 1000ms polling interval.
* **Stability**: Successfully completed a **48-hour continuous stress test** with zero hard resets and bounded memory utilization.
* **Accuracy**: CO2 readings cross-validated against a Testo 435-2 reference instrument.

---

## 📝 Authors
* **Ambulkar Madhusudan** - Dept. of Computer Science, Frankfurt UAS
* **Khan Sahil** - Dept. of Computer Science, Frankfurt UAS
* **Ahmed Wahaj** - Dept. of Computer Science, Frankfurt UAS

---

## 📜 References
* [1] ASHRAE Standard 62.1-2022: Ventilation and Acceptable Indoor Air Quality.
* [2] World Health Organization (WHO) Global Air Quality Guidelines.
* [3] Sensirion SCD41 & SPS30 Datasheets.
