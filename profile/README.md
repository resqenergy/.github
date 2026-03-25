# ResQEnergy Documentation

Overview of repositories in the **ResQEnergy** organization and their roles in preprocessing data for Energy System (ES) simulations.

<p align="center">
  <img src="../assets/logo.jpg" title="ResQEnergy Logo" width="512px" height="340px"/>
</p>

## Table of Contents
- [General Architecture](#general-architecture)
- [Repositories](#repositories)
    - [npro](#npro)
    - [es_adlershof](#es_adlershof)
    - [resqdb](#resqdb)

## General Architecture
The main workflow involves exporting result data from one repository into **S3 storage** and importing it as input for subsequent tools.

---

## Repositories

### [npro](https://github.com/resqenergy/npro)
**Role:** Weather data application and demand generation.

- **Output for:** [`es_adlershof`](#es_adlershof)
- **Description:** 
    - Applies different weather data years to the NPRO tool.
    - Stores resulting building demand data to CSV files or S3.
    - These demands are used to build the datapackage in [`es_adlershof`](#es_adlershof).

### [es_adlershof](https://github.com/resqenergy/es_adlershof)
**Role:** Energy system structure and datapackage assembly.

- **Input from:** [`npro`](#npro) (via S3)
- **Output for:** [`resqdb`](#resqdb)
- **Description:** 
    - Sets up the energy system structure.
    - Loads data from multiple sources (including S3) into an `oemof.tabular` datapackage.

### [resqdb](https://github.com/resqenergy/resqdb)
**Role:** Scenario management and simulation results.

- **Input from:** [`es_adlershof`](#es_adlershof)
- **Description:** 
    - Can hold multiple `oemof.tabular` datapackages (scenarios).
    - Runs simulations and stores result data in an external database.
    - Generates visualizations for **Apache Superset**.
 
### [resq_demands](https://github.com/resqenergy/resq_demands)
**Role:** Calculation of demands

- **Input from:** [`Sharepoint`](https://rlinstitutde.sharepoint.com/:f:/s/427_ResQEnergy-427_internal_Team/IgBvQkHRbzj5QKYOSYSKpBqHASKIRa8Dw6rd0rz9ZfQEkVU?e=p1cire) and [`npro`](#npro)
- **Output for:** [`Sharepoint`](https://rlinstitutde.sharepoint.com/:f:/s/427_ResQEnergy-427_internal_Team/IgDkGXbWJShKSbR1Qf9c9xKmAd02Hn3EjOBNuNNcSgRISIY?e=aUOa5x)
- **Description:** 
    - Calculates total area and units per Cluster and per bus for status quo and projections for 2035 and 2050
    - Generates energy demands and their time series

### [resq_process_company_data](https://github.com/resqenergy/resq_process_company_data)
**Role:** Calculation of area and units per Cluster of non-residential buildings via processing company data and Gebäudegrunddatensatz

- **Input from:** [`Sharepoint`](https://rlinstitutde.sharepoint.com/:f:/s/427_ResQEnergy-427_internal_Team/IgBrRnJIDRASTrrFOauHZw_pATnH9rJFoh6hthIq46IUDBI?e=mQt8Cl)
- **Output for:** [`Sharepoint`](https://rlinstitutde.sharepoint.com/:f:/s/427_ResQEnergy-427_internal_Team/IgCTT3B0WA2pRIoc4zZY0s-SASHBtR4BINgFFCASqVY0xcE?e=Rf9VdT)
- **Description:** 
    - Calculates total area and units per Cluster and per bus for status quo of non-residential buildings
 
### [resq_precalc](https://github.com/resqenergy/resq_precalc)
**Role:** Various precalculations in the project

- **Input from:** [`Sharepoint`](https://rlinstitutde.sharepoint.com/:f:/s/427_ResQEnergy-427_internal_Team/IgBmfAcZ58xLTIGwckVM2mVvAa0gHGJZmahbooWqKqTfpRI?e=d32ruc)
- **Description:** 
    - COP of the industrial heat waste (under construction)
    - Calculation of the mean temperature of the BTB-dh network
    - Calculation of the total heat quantity of the Teltow Canal at the measuring point
    - Calculation of a mean electricity price from a time series
    - Generation of a plot showing the shares of supply in future district heating systems according to Langfristszenarien
    - A helping script to modify oemof-B3 weatherdata with Testreferenzjahre-ts
    - Calculation of the installed capacity of solarthermal (st) plants in Adlershof without Wohnen am Campus for results on installed capacities in Adlershof
