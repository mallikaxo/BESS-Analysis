***

# ⚡ Solar Energy and Battery Energy Storage System (BESS) Analysis

## 🧩 Project Overview  
This project focuses on the **performance analysis of a Battery Energy Storage System (BESS)** — a critical component in modern smart grids and renewable energy integration.  

The notebook explores how energy is:  
- **Generated** from renewable or distributed sources  
- **Consumed** by connected loads  
- **Stored** in the battery for later use  
- **Exported** back to the grid when production exceeds demand  

By leveraging **data analytics and visualization**, this project provides insights into the **energy flow dynamics**, system efficiency, and the **optimal utilization** of storage capacity.  
---

## 🛠️ Project Structure & Workflow

The project is structured into two main, sequential components:

| Notebook | Role | Key Functionality |
| :--- | :--- | :--- |
| **`solar_data_definition.ipynb`** | **Data Preparation** | Cleans, interpolates, and standardizes raw solar and consumption data into a clean 1-minute time series. |
| **`bess_analysis.ipynb`** | **BESS Simulation & Analysis** | Contains the core BESS model, simulates energy flow, calculates financial metrics, and generates visualizations. |

---

## 🎯 Objectives  
- Evaluate the **charging and discharging behavior** of the battery system  
- Quantify the **energy and power exchanged** between the grid, storage, and local consumption  
- Identify **patterns or inefficiencies** in system performance  
- Support **decision-making** for improving renewable energy integration and grid stability  

---

## 📂 1. Data Preparation (`solar_data_definition.ipynb`)

This notebook addresses common issues in raw energy data, such as irregular measurement intervals and missing values, to create a uniform 1-minute time-step dataset.

### Key Data Processing Steps:

* **Time Series Normalization**: A uniform, per-minute **timestamp** index (1440 data points for 24 hours) is established.
* **Cubic Interpolation**: The `scipy.interpolate.interp1d` function with **cubic** interpolation is used to smoothly estimate missing values:
    * **Generation**: Missing solar data is safely filled with **0**.
    * **Consumption**: Missing load data is filled using **extrapolation** to maintain a continuous load profile.
* **Output**: The clean dataset is saved as `dataset.csv` for use in the BESS simulation.

---

## 🔋 2. BESS Simulation and Financial Analysis (`bess_analysis.ipynb`)

This is the core analysis notebook, which uses an OOP approach to model the BESS interaction with the solar generator and the grid.

### Technical Implementation (OOP Modeling)

The central **`BatterySystem`** class manages all energy transactions:

* **`__init__`**: Initializes the battery with a capacity (e.g., $13.5$ kWh) and an initial State of Charge (SOC) (e.g., $27\%$).
* **`step(generation, consumption)`**: The simulation core. It calculates the energy balance for each 1-minute time step, determining:
    * If the system needs to **charge** the battery with surplus solar power.
    * If the battery needs to **discharge** to meet the load deficit.
    * If power needs to be **imported** from or **exported** to the grid.
* **`energy(df)`**: Quantifies total energy (in kWh) exchanged between all components using **trapezoidal numerical integration** (`numpy.trapezoid`).
* **`billing(df)`**: Provides a financial summary based on assumed import (₹10/kWh) and export (₹6/kWh) tariffs.

### Simulation Summary

The model calculates the total energy (kWh) exchanged and the resulting financial impact over the simulation period.

| Energy Metric | Value (kWh) |
| :--- | :--- |
| Total Generation | $73.53$ |
| Total Consumption | $68.27$ |
| Grid Import | $4.12$ |
| Grid Export | $11.87$ |
| Battery Charged | $15.37$ |
| Battery Discharged | $-17.87$ |

| Financial Analysis | Amount (₹) |
| :--- | :--- |
| **Bill Without Battery** | ₹682.72 |
| **Bill With Battery (Import Cost)** | ₹41.15 |
| **Profit Due to Export** | ₹71.23 |
| **Net Balance (Profit)** | **₹30.07** |

### Key Visualizations

The analysis is supported by robust time-series plots and distribution charts:

* **Energy Source Contribution (Pie Chart)**: Clearly shows how much of the $68.27$ kWh total consumption was supplied by **Solar** ($67.8\%$), the **Battery** ($26.2\%$), and the **Grid** ($6.0\%$).
* **System Metrics (Bar Plots)**: Detailed time-series view of:
    * Solar Generation (kW)
    * House Load (kW)
    * Grid Power (kW)
    * Battery Power (kW)
    * Battery State of Charge (%)
    * Battery Stored Energy (kWh)


## 📌 Future Scope  
- Integration with **real-time monitoring systems**  
- Implementation of **machine learning models** for predictive energy management  
- Expansion to include **multiple storage and generation sources**  
