# Formula Student EV Traction Control (Model-Based Design)

This repository contains a Model-Based Design (MBD) workflow for a longitudinal Traction Control System (TCS) tailored for an independent-drive Formula Student electric vehicle. 

The project demonstrates the complete engineering loop used in modern motorsport: modeling physical vehicle dynamics in continuous time, synthesizing a discrete digital controller, auto-generating C++ code for an embedded Vehicle Control Unit (VCU).

## 🏎️ System Architecture

The system is divided into three core domains:

1. **Continuous-Time Plant (Simulink):** A non-linear quarter-car physics model simulating an Emrax electric motor, 1:5 planetary gear reduction, tire rotational inertia, aerodynamic drag ($C_d A = 1.3$), and a linear tire friction model with peak force saturation. 
2. **Discrete-Time Controller (Simulink $\rightarrow$ C++):** A 100 Hz digital PID controller targeting an optimal 10% longitudinal slip ratio ($\lambda = 0.10$). Features anti-windup clamping to respect the physical saturation limits of the motor inverter (0 to 150 Nm).
3. **Telemetry Validation (Python):** Post-simulation data processing using `pandas` and `matplotlib` to analyze controller rise time, overshoot, and steady-state slip tracking.

## 📂 Repository Structure

```text
FS-Traction-Control-Sim/
├── Simulink/
│   ├── E_Team_TCS.slx             # Main model (Physics Plant + Atomic Controller Subsystem)
│   └── tcs_telemetry_out.csv      # Exported workspace data from simulation runs
├── Embedded_VCU_Code/
│   ├── Traction_Controller.cpp    # Auto-generated C++ 100Hz step function
│   ├── Traction_Controller.h      # Header file for VCU integration
│   └── ert_main.cpp               # Example hardware timer execution wrapper
├── Telemetry_Analysis/
│   └── plot_tcs_data.py           # Python script to generate slip/torque telemetry graphs
└── README.md
