# Minidrone Autonomous Landing

Autonomous flight system for the Parrot Mambo minidrone that navigates a competition course and lands precisely on a moving colored target. Fully designed, tuned, and simulated in MATLAB/Simulink, with code generation for real hardware deployment.

---

## Demo

https://github.com/Hp092/minidrone-autonomous-landing/blob/main/demo.mp4

---

## Overview

The drone takes off, follows a pre-defined waypoint trajectory, and autonomously lands on a moving platform identified by color using the onboard downward-facing camera. The controller is built on a cascaded PID architecture using a linearized state-space airframe model, with an IMU Kalman filter and complementary filter for attitude estimation.

| Capability | Details |
|---|---|
| Takeoff & altitude hold | Barometric + sonar altitude KF |
| Attitude control | Cascaded PID on linearized state-space model |
| Waypoint tracking | Pre-defined competition trajectory |
| Target detection | Color sensing via downward camera |
| Autonomous landing | Vision-guided descent onto moving colored target |

---

## Hardware

| Component | Details |
|---|---|
| Platform | Parrot Mambo minidrone |
| Sensors | IMU (accel + gyro), barometer, sonar, downward camera |
| Interface | MATLAB Aerospace Toolbox — Parrot support package |

---

## Software

| Layer | Technology |
|---|---|
| Modeling & simulation | MATLAB / Simulink |
| Control design | Linearized state-space, cascaded PID |
| State estimation | IMU Kalman filter, complementary filter, altitude KF |
| Airframe models | Linear and nonlinear Simulink models |
| Trajectory planning | Competition Track Builder MATLAB app (`drone_track_builder.mlapp`) |
| Flight code generation | Simulink Coder → Parrot target |

---

## Control Architecture

The flight controller uses a cascaded structure:

1. **Outer loop** — position and altitude reference tracking (waypoints → attitude setpoints)
2. **Inner loop** — attitude and rate control (PID on roll, pitch, yaw)
3. **Control mixer** — maps thrust/torque demands to per-motor commands via `Ts2Q` matrix
4. **State estimator** — Kalman filter on IMU data for angle and rate estimates; separate KF for altitude fusing barometer and sonar

Motor thrust is clipped to 92% of maximum to retain attitude control headroom at full throttle.

---

## Repository Structure

```
minidrone-autonomous-landing/
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
│   ├── controllerVars.m             # Controller tuning parameters
│   ├── estimatorVars.m              # State estimator parameters
│   ├── vehicleVars.m                # Vehicle physical parameters
│   └── sensorsVars.m                # Sensor model parameters
├── utilities/
│   ├── startVars.m                  # Project startup script
│   ├── generateFlightCode.m         # Code generation script
│   └── drone_track_builder.mlapp    # Competition track builder app
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

% 3. (Optional) Edit the competition trajectory
openTrackBuilder()

% 4. Open the main simulation model
open_system('mainModels/parrotMinidroneCompetition.slx')

% 5. Run the simulation
sim('mainModels/parrotMinidroneCompetition.slx')
```

To generate and deploy flight code to the Parrot hardware:

```matlab
run('utilities/generateFlightCode.m')
```

---

## Authors

**Harsh Padmalwar**  
**Atharv Kulkarni**
