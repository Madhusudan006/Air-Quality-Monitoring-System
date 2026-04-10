# Real-Time Air Quality Monitoring System (AQMS)

[cite_start]An end-to-end Internet of Things (IoT) solution designed to bridge the gap between raw environmental data and actionable health advisories[cite: 11, 114]. [cite_start]This system leverages a high-performance edge core and a deterministic safety logic engine to provide sub-second latency visualization of indoor air quality[cite: 13, 28].

## 🚀 Key Features
* [cite_start]**Edge-Native Architecture**: Powered by **Zephyr RTOS** on a **Nordic nRF52840** (ARM Cortex-M4F), providing deterministic multi-sensor polling without cloud dependency[cite: 12, 28].
* [cite_start]**Multi-Modal Sensor Fusion**: Aggregates high-fidelity data from three specialized sensors to monitor $CO_{2}$, TVOC, PM2.5, PM10, and Humidity[cite: 13, 29].
* [cite_start]**Robust ETL Pipeline**: Features a sub-10ms **Regex-based ingestion engine** and an advanced **brace-counting algorithm** for reconstructing fragmented JSON packets[cite: 38, 247].
* [cite_start]**ASHRAE-Compliant Safety Logic**: A deterministic engine that classifies environmental severity into four tiers: OK, WARN, ALERT, and CRITICAL[cite: 31, 39].
* [cite_start]**Real-Time Dashboard**: A **Streamlit-based** analytics interface delivering visual updates every 1,000ms[cite: 30, 98].

---

## 🛠 Hardware Architecture

[cite_start]The system utilizes a **split-rail power topology** to ensure signal integrity and manage divergent voltage requirements across the sensor array[cite: 29, 189].

### Components
* [cite_start]**MCU**: Nordic nRF52840 (64 MHz, 1MB Flash, 256KB RAM)[cite: 68, 125].
* [cite_start]**$CO_{2}$ Sensor**: Sensirion **SCD41** (Photoacoustic principle, $\pm30$ ppm accuracy)[cite: 83, 158].
* **PM Sensor**: Sensirion **SPS30** (Laser scattering with internal fan for constant airflow)[cite: 163, 164].
* [cite_start]**TVOC Sensor**: AMS **CCS811** (MOX-based sensing with $eCO_{2}$ cross-validation)[cite: 171, 175].
* **Power**: **MB102 module** providing isolated 3.3V (Logic) and 5V (SPS30 Fan) rails[cite: 189, 190].

### Protocol Partitioning
| Sensor | Interface | Protocol Rationale |
| :--- | :--- | :--- |
| **SCD41** | I2C (400 kHz) | Native compatibility and simple bus configuration[cite: 153, 198]. |
| **CCS811** | I2C (400 kHz) | Shared 3.3V logic rail; unique 7-bit addressing[cite: 184, 199]. |
| **SPS30** | UART (115200) | Isolated from I2C to avoid fan-induced electrical noise[cite: 170, 201]. |

---

## 💻 Software Methodology

### ETL Pipeline & Analysis
The software infrastructure follows a six-stage sequential flow to ensure data integrity[cite: 208]:
1.  **Data Ingestion**: Continuous ingestion via Serial and CoAP interfaces[cite: 239].
2.  **Regex Sanitization**: Strips ANSI escape codes and line noise in $<10$ms[cite: 246, 247].
3.  **Brace-Counting Recovery**: Reconstructs fragmented JSON payloads with 99.8% success[cite: 268, 374].
4.  **Time-Series Analysis**: Uses **Pandas** for rolling averages, delta calculations, and staleness checks[cite: 273, 274].
5.  **Safety Logic Mapping**: Evaluates fused sensor data against **ASHRAE Standard 62.1** and WHO guidelines[cite: 101, 285].
6.  **Visualization**: Streamlit reactive rendering for real-time frontend updates[cite: 98, 278].

---

## 📊 Performance Evaluation
* [cite_start]**Latency**: Total mean pipeline execution time of **29.7 ms**, well within the 1000ms polling interval[cite: 367, 368].
* [cite_start]**Stability**: Successfully completed a **48-hour continuous stress test** with zero hard resets and bounded memory utilization[cite: 355, 356].
* **Accuracy**: $CO_{2}$ readings cross-validated against a Testo 435-2 reference instrument[cite: 349].

---

## 📝 Authors
* [cite_start]**Ambulkar Madhusudan** - Dept. of Computer Science, Frankfurt UAS [cite: 2, 3]
* [cite_start]**Khan Sahil** - Dept. of Computer Science, Frankfurt UAS [cite: 5, 6]
* **Ahmed Wahaj** - Dept. of Computer Science, Frankfurt UAS [cite: 8, 9]

---

## 📜 References
* [cite_start][1] ASHRAE Standard 62.1-2022: Ventilation and Acceptable Indoor Air Quality[cite: 480].
* [cite_start][2] World Health Organization (WHO) Global Air Quality Guidelines[cite: 482].
* [cite_start][3] Sensirion SCD41 & SPS30 Datasheets[cite: 471, 486].
