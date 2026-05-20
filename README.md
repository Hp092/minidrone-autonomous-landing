# Parrot Minidrone Landing Controller

Autonomous flight control system for the Parrot Mambo minidrone, designed and simulated in MATLAB/Simulink. The controller handles takeoff, waypoint tracking, and landing using a cascaded control architecture built on linearized airframe dynamics.

---

## Demo

https://github.com/Hp092/parrot-minidrone-landing-controller/blob/main/demo.mp4

---

## Hardware

| Component | Details |
|---|---|
| Platform | Parrot Mambo minidrone |
| Sensors | IMU, barometer, downward camera |
| Interface | MATLAB Aerospace Toolbox — Parrot support package |

---

## Software

| Layer | Technology |
|---|---|
| Modeling & simulation | MATLAB / Simulink |
| Control design | Linearized state-space, cascaded PID |
| Airframe models | Linear and nonlinear Simulink models |
| Flight code generation | Simulink Coder → Parrot target |

---

## Repository Structure

```
parrot-minidrone-landing-controller/
├── controller/
│   └── flightControlSystem.slx      # Main flight control system
├── mainModels/
│   ├── parrotMinidroneCompetition.slx  # Top-level simulation model
│   ├── cmdData.mat / cmdData.xlsx      # Command trajectory data
│   └── sensorCalibration.mat
├── linearAirframe/
│   ├── linearAirframe.slx           # Linearized airframe model
│   ├── trimLinearizeOpPoint.m        # Trim & linearize script
│   └── trimNonlinearAirframe.slx
├── nonlinearAirframe/
│   └── nonlinearAirframe.slx        # Full nonlinear airframe model
├── libraries/
│   ├── dynamicsLibrary.slx          # Dynamics block library
│   └── environmentLibrary.slx       # Environment block library
├── tasks/
│   ├── asbBusDefinitionCommand.m    # Bus signal definitions
│   ├── controllerVars.m             # Controller tuning parameters
│   ├── estimatorVars.m              # State estimator parameters
│   └── vehicleVars.m                # Vehicle physical parameters
├── utilities/
│   ├── startVars.m                  # Project startup script
│   └── generateFlightCode.m        # Code generation script
├── support/                         # 3D visualization assets (VRML)
├── demo.mp4                         # Flight demonstration
└── MinidroneCompetition.prj         # MATLAB project file
```

---

## Getting Started

**Requirements:** MATLAB R2023b or later, Simulink, Aerospace Toolbox, Parrot support package.

```matlab
% 1. Open the project
open('MinidroneCompetition.prj')

% 2. Initialize workspace variables
run('utilities/startVars.m')

% 3. Open the main simulation model
open_system('mainModels/parrotMinidroneCompetition.slx')

% 4. Run the simulation
sim('mainModels/parrotMinidroneCompetition.slx')
```

To generate flight code for the Parrot hardware:

```matlab
run('utilities/generateFlightCode.m')
```

---

## Authors

**Harsh Padmalwar**  
**Atharv Kulkarni**
