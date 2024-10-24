# Decoupled Power Flow Algorithm

This repository contains implementations of the **Decoupled Power Flow** algorithm for performing power flow analysis on distribution or transmission systems in both MATLAB and Python. The Decoupled Power Flow (DPF) method is a simplified version of the Newton-Raphson method, suitable for large power systems. The code can be used for any system model, provided that the input data adheres to the specified format.

## Overview

The primary function computes the voltage profile of buses in a radial or mesh distribution system. It requires two key inputs: load data and line data. The load data specifies the bus index along with the real and reactive power for each bus, while the line data includes information on the resistance and reactance of the lines connecting the buses.

### Functionality

The function includes the following features:
- **Data Input Requirements**:
  - The load data must be structured with three columns: bus index, real power (P), and reactive power (Q).
  - The line data must have four columns: sending bus index, receiving bus index, resistance (R), and reactance (X).
  
- **Customizable Parameters**: Users can specify a constant slack bus voltage, a convergence tolerance for iteration, and a maximum number of iterations allowed for the calculations. Default values are provided but can be overridden when calling the function.

- **Decoupled Power Flow Approach**: The method leverages the decoupling between active power (P) and voltage angles (θ), and reactive power (Q) and voltage magnitudes (|V|). The decoupled Jacobian matrix allows for faster computation.

### General Assumptions for Decoupled Power Flow

- **Decoupled Power Flow Equations**: The power flow equations are simplified by assuming that the variations in real power (P) primarily affect the voltage angles (θ), and the variations in reactive power (Q) primarily affect the voltage magnitudes (|V|).

- **Assumptions in the Jacobian Matrix**: The method assumes that:
  - \( J_2 = 0 \): Real power \( P \) has negligible dependence on voltage magnitudes \( |V| \).
  - \( J_3 = 0 \): Reactive power \( Q \) has negligible dependence on voltage angles \( \theta \).

This assumption leads to the following simplified Jacobian matrix:


This greatly simplifies the computational process, speeding up the solution.

- **Flat Start Initialization**: Typically, the initial guess for voltage magnitudes is set to 1.0 p.u. for non-slack buses, and the voltage angle at the slack bus is set to 0 radians.

- **Lossless Network**: The method assumes that the transmission lines are primarily inductive, so the resistance (R) is much smaller than the reactance (X). This assumption simplifies the decoupled equations.

- **Iterations**: The method iteratively solves for voltage angles and magnitudes separately until convergence is reached, which is determined by the maximum error between iterations being below a specified tolerance.

### Guidance for Use

1. **Input Format**: Ensure that the input data (i.e., load and line data) are formatted correctly:
 - **Load Data**: 
   - Column 1: Bus index
   - Column 2: Real power (P)
   - Column 3: Reactive power (Q)
 - **Line Data**: 
   - Column 1: Sending bus index
   - Column 2: Receiving bus index
   - Column 3: Line resistance (R)
   - Column 4: Line reactance (X)

2. **Function Call**: To execute the function, you can provide the load and line data as arguments, along with optional parameters for slack bus voltage, tolerance, and maximum iterations. If you skip any of the optional parameters, the function will use the predefined default values.

3. **Example Usage**:
 - **Python**:
   ```python
   v, iteration = dpf_method(load_data, line_data, slack_bus_voltage=1.05, tolerance=1e-4)
   ```
 - **MATLAB**:
   ```matlab
   [v, iteration] = dpf_method(load_data, line_data, 1.05, 1e-4);
   ```

4. **Exiting the Function**: Users are prompted to press Enter to continue or type 'quit' to exit the function. This allows for flexibility in how users interact with the function.

### Expected Outputs

Upon successful execution, the function returns the following outputs:
- Voltage after each iteration
- Number of iterations
- Time taken for each iteration
- Average time taken for all runs
- Total active and reactive power loss
- Substation active and reactive power
- Graph of voltage magnitude and angle
- Single line diagram of the system model
- Graph of the relationship between maximum error and computational time
- .csv and .txt file of the voltage profile

This provides a comprehensive tool for conducting power flow analysis in distribution networks, supporting both MATLAB and Python users.

## Files

- `dpf_method.py`: Python code for the Decoupled Power Flow method for power flow analysis.
- `dpf_method.m`: MATLAB code for the Decoupled Power Flow method for power flow analysis.
