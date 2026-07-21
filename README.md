# GO-ERCOT-2026

Updated GO ERCOT workflow for simulation and analysis of the Electric Reliability Council of Texas (ERCOT) grid operations.

## Overview

GO ERCOT is a grid operations model developed to simulate and analyze the operations of the Electric Reliability Council of Texas (ERCOT) interconnection. This Python-based workflow processes grid data, runs optimization-based simulations, and generates detailed outputs for analysis and reporting.

**GO ERCOT Model Characteristics:**
- Written using the **Pyomo** optimization package in Python
- Models the ERCOT interconnection with a detailed nodal topology
- Allows flexible configuration of network resolution and mathematical formulations
- Supports transmission line capacity and inter-BA hurdle rate scaling

### Relationship to GO WEST

GO ERCOT shares similar functionality with **GO WEST** (Grid Operations model of the U.S. Western Interconnection), but with a focus on the ERCOT market. Both models:
- Use Pyomo for optimization modeling
- Support configurable node reduction and network simplification
- Allow selection of mathematical formulations (LP or MILP)
- Generate detailed grid operation simulations
- Provide hourly operational analysis

The key difference is the **geographic focus**: GO WEST models the Western Interconnection (WECC), while GO ERCOT focuses on the Texas-based ERCOT grid.

## Project Structure

```
GO-ERCOT-2026/
├── inputs/              # Input data and configuration files
├── outputs/             # Generated output files and results
├── simulation_folders/  # Simulation scripts and model files
└── README.md           # This file
```

### Directories

- **inputs/**: Contains input data and configuration files needed for simulations
  - Network topology data
  - Generator parameters and characteristics
  - Load profiles
  - Renewable energy availability data (solar, wind, hydro)
  - Transmission line parameters
  - Fuel prices and other economic inputs
  
- **outputs/**: Stores all generated output files and results from simulations
  - Optimization results
  - Operational metrics
  - Analysis reports
  
- **simulation_folders/**: Contains simulation-related Python scripts and supporting files
  - Data preprocessing scripts
  - Pyomo model formulations (LP and MILP variants)
  - Wrapper scripts for running simulations
  - Data aggregation and formatting scripts

## Running GO ERCOT Model

Running the GO ERCOT model includes two main phases:

### Phase 1: Preprocessing of Data

1. **Data Preparation**: Use preprocessing scripts to select and organize the ERCOT network topology and generator data from source datasets

2. **Network Reduction** (if applicable): Apply network reduction algorithms to simplify the model by:
   - Reducing the number of nodes/buses
   - Aggregating transmission lines
   - Consolidating generators
   
3. **Data Allocation**: Process the reduced network data to create model input files including:
   - Generator parameters and fuel type classifications
   - Hourly solar and wind power availability by node
   - Hydroelectric generation constraints
   - Transmission line thermal limits and reactances
   - BA-to-BA transmission hurdle rates
   - Nodal load profiles

### Phase 2: Starting the Simulation

1. **Data Setup**: The `ERCOTDataSetup.py` script merges all relevant data into a single data file (e.g., `ERCOT_data.dat`) formatted for Pyomo interpretation

2. **Model Execution**: Run the wrapper script to:
   - Call the optimization solver
   - Execute the selected model formulation
   - Generate simulation results

## Key Configuration Options

The GO ERCOT model allows users to choose:
- **Number of nodes** to retain in the system
- **Mathematical formulation** (LP for economic dispatch or MILP for unit commitment)
- **Transmission line capacity scaling factors**
- **BA-to-BA hurdle rate scaling factors**
- **Generator commitment scope** (coal only, coal + gas, or economic dispatch only)

## Model Files (Simulation_folders)

Typical files generated and used in simulations include:

| File Name | Description |
|-----------|-------------|
| `ERCOT_data.dat` | Consolidated data file in Pyomo format containing all model inputs |
| `data_genparams.csv` | Names and parameters of all generators |
| `nodal_load.csv` | Hourly electricity demand at each node |
| `nodal_solar.csv` | Hourly available solar power generation at each node |
| `nodal_wind.csv` | Hourly available wind power generation at each node |
| `Hydro_max.csv` | Maximum hourly hydropower availability at each node |
| `Hydro_min.csv` | Minimum hourly hydropower availability at each node |
| `Hydro_total.csv` | Daily total hydropower availability at each node |
| `line_params.csv` | Thermal limits and reactances of each transmission line |
| `BA_to_BA_transmission_matrix.csv` | Binary matrix showing inter-BA connections |
| `BA_to_BA_hurdle_scaled.csv` | Scaled hurdle rates between BAs |
| `Fuel_prices.csv` | Daily coal and natural gas prices for each generator |
| `must_run.csv` | Available nodal must-run generation capacity |
| `gen_mat.csv` | Binary matrix showing generator-to-bus connections |
| `line_to_bus.csv` | Binary matrix showing transmission line-to-bus connections |
| `ERCOTDataSetup.py` | Python script that creates the `ERCOT_data.dat` file |
| `ERCOT_LP_coal.py` | LP problem formulation (coal only UC) |
| `ERCOT_LP_coal_gas.py` | LP problem formulation (coal + gas UC) |
| `ERCOT_MILP_coal.py` | MILP problem formulation (coal only UC) |
| `ERCOT_MILP_coal_gas.py` | MILP problem formulation (coal + gas UC) |
| `ERCOT_simple.py` | LP formulation for economic dispatch (no UC) |
| `wrapper_coal.py` | Execution script for coal-only unit commitment |
| `wrapper_coal_gas.py` | Execution script for coal + gas unit commitment |
| `wrapper_simple.py` | Execution script for economic dispatch |

## Getting Started

### Prerequisites

- Python 3.x
- Pyomo optimization package
- Optimization solver (e.g., CPLEX, Gurobi, or open-source alternatives like CBC or GLPK)
- Required Python packages (see requirements.txt)

### Installation

1. Clone the repository:
```bash
git clone https://github.com/sembahen/GO-ERCOT-2026.git
cd GO-ERCOT-2026
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Install an optimization solver (if not already available):
```bash
# Example: Install CBC solver (open-source)
conda install -c conda-forge coinbonmin
```

### Usage

1. **Prepare Input Data**: Ensure all required input files are in the `inputs/` directory

2. **Run Data Setup**: Execute the data setup script to prepare data for the model:
```bash
python simulation_folders/ERCOTDataSetup.py
```

3. **Run Simulation**: Execute the appropriate wrapper script based on your configuration:
```bash
# For coal and gas unit commitment
python simulation_folders/wrapper_coal_gas.py

# Or for economic dispatch only
python simulation_folders/wrapper_simple.py
```

4. **Review Results**: Check the `outputs/` directory for simulation results and analysis

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request or open an issue for bugs, feature requests, or improvements.

## License

[Add license information here]

## References

- GO WEST Repository: [https://github.com/IMMM-SFA/IM3_WECC](https://github.com/IMMM-SFA/IM3_WECC)
- Pyomo Documentation: [http://www.pyomo.org/](http://www.pyomo.org/)

## Contact

For questions or issues, please open an issue on the [GitHub repository](https://github.com/sembahen/GO-ERCOT-2026).
