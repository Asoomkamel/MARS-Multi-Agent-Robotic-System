<div align="center">

# 🤖 MARS: Multi-Agent Robotic System
### Design and Implementation of Smart Navigation and Coordination System for AI-Driven Heterogeneous Multi-Agent Robotic Systems

[![ROS 2 Humble](https://img.shields.io/badge/ROS_2-Humble-blue?logo=ros&logoColor=white)](https://docs.ros.org/en/humble/)
[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![C++](https://img.shields.io/badge/C++-17-00599C?logo=c%2B%2B&logoColor=white)](https://isocpp.org/)
[![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![YOLOv8](https://img.shields.io/badge/YOLOv8-Ultralytics-FF6600)](https://github.com/ultralytics/ultralytics)
[![Nav2](https://img.shields.io/badge/Nav2-MPPI%20%2B%20Smac-informational?logo=ros)](https://nav2.org/)
[![Micro-ROS](https://img.shields.io/badge/Micro--ROS-ESP32-green)](https://micro.ros.org/)
[![CUDA](https://img.shields.io/badge/CUDA-GPU%20Accelerated-76B900?logo=nvidia&logoColor=white)](https://developer.nvidia.com/cuda-toolkit)
[![PlatformIO](https://img.shields.io/badge/PlatformIO-ESP32-orange?logo=platformio&logoColor=white)](https://platformio.org/)
[![ESP-IDF](https://img.shields.io/badge/ESP--IDF-Bare--Metal-red?logo=espressif&logoColor=white)](https://idf.espressif.com/)

**Bachelor's Graduation Project — Sana'a University, Faculty of Engineering**  
*Electrical Engineering · Computer and Control Engineering · 2026*

---

<!-- BANNER 1 -->
<img src="./media/assets/banner_1.png" alt="MARS Project Overview Banner" width="100%"/>

---

<!-- BANNER 2 -->
<img src="./media/assets/banner_2.png" alt="MARS Project Objectives and Tools Banner" width="100%"/>

</div>

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Physical Prototype](#-physical-prototype)
- [System Architecture](#️-system-architecture)
- [Hardware Specifications](#️-hardware-specifications)
- [Mechanical & Hardware Design](#️-mechanical--hardware-design)
- [System Modeling & Simulation](#-system-modeling--simulation)
- [Navigation Stack](#-navigation-stack)
- [Semantic Perception Pipeline](#-semantic-perception-pipeline-yolo--costmap)
- [YOLO Depth Fusion Node](#-yolo-depth-fusion-node-obstacle_detectorpy)
- [Sensor Fusion — EKF](#-sensor-fusion--ekf)
- [SLAM & Localization](#️-slam--localization)
- [Transformation Trees (TF)](#-transformation-trees-tf)
- [Fleet Management System](#-fleet-management-system)
- [Fleet Monitoring Dashboard](#-fleet-monitoring--control)
- [Embedded Control — ESP32 firmware](#-embedded-control--esp32-firmware)
- [Docker Deployment](#-docker-deployment)
- [ROS 2 Topic Reference](#-ros-2-topic-reference)
- [Operational Logic](#-operational-logic)
- [Demo Videos](#-demo-videos)
- [Team](#-team)

---

## 📌 Project Overview

This project presents the design and implementation of an **intelligent navigation and coordination system** for heterogeneous, AI-driven multi-agent robotic platforms. Designed for complex indoor environments such as hospitals, hotels, and warehouses, the system integrates centralized fleet coordination with distributed edge intelligence.

### Key Features:
- **Autonomous Navigation:** GPU-accelerated MPPI local control with Hybrid-A\* global planning and real-time traffic de-confliction.
- **AI Perception:** YOLOv8-based semantic obstacle detection fused into Nav2 costmaps via a custom VoxelLayer observation source.
- **Sensor Fusion:** Extended Kalman Filter combining wheel odometry and IMU for robust, drift-resistant localization.
- **Human-Robot Interaction:** Voice-driven commands using Whisper and Gemini AI.
- **Secure Access:** RFID-based door mechanism for authorized zone entry.
- **Fleet Management:** Battery-aware task allocation, side-spot traffic de-confliction, and fault-tolerant task reallocation.
- **Containerized Deployment:** Full system runs inside Docker for reproducible, dependency-free deployment.
- **Dual ESP32 Embedded Firmware:** KAMIL-01 uses PlatformIO + Arduino framework with Micro-ROS; KAMIL-02 uses ESP-IDF bare-metal with explicit FreeRTOS dual-core partitioning for hard real-time motor control.
- **YOLO Depth Fusion Node:** Standalone ROS 2 Python node synchronizing RGB and depth streams, back-projecting detections into 3D, and publishing PointCloud2 obstacles directly into the Nav2 VoxelLayer costmap.

---

## 🤖 Physical Prototype

The project culminated in the construction of two functional robotic units, **KAMIL-01** and **KAMIL-02**, featuring a custom-built chassis and integrated sensor suites.

<div align="center">
<img src="./media/assets/robot_photo_real.png" alt="KAMIL-01 and KAMIL-02 Robots" width="80%"/>
<p><i>KAMIL-01 and KAMIL-02: The physical realization of the MARS project.</i></p>
</div>

---

## 🏗️ System Architecture

The system follows a **Three-Tier Hierarchical Architecture**, ensuring a clear separation between high-level supervisory tasks, mid-level bridge processing, and low-level real-time control.

<div align="center">
<img src="./media/assets/system_architecture.png" alt="System Architecture" width="90%"/>
</div>

| Layer | Hardware | Responsibilities |
|---|---|---|
| **Supervisory Layer** | Cloud / Laptop Server | Fleet Management System (FMS), Dashboard GUI, task orchestration |
| **Bridge Layer** | Raspberry Pi 5 | ROS 2 nodes, Nav2 stack, YOLOv8 inference, EKF, map serving |
| **Real-Time Layer** | ESP32 + Micro-ROS | Motor PWM, PID control, encoder odometry, battery ADC, watchdog safety |

The Bridge and Real-Time layers communicate via **DDS-XRCE (Micro-ROS)** over UART/USB serial, while the Supervisory and Bridge layers communicate over **Wi-Fi** using ROS 2 DDS and REST/HTTP.

---

## 🖥️ Hardware Specifications

| Component | Specification |
|---|---|
| **Onboard Computer** | Raspberry Pi 5 (8 GB) |
| **Embedded Controller** | ESP32 (240 MHz dual-core, 520 KB SRAM) |
| **RGB-D Camera** | Intel RealSense D435i (depth range 0.3–3.0 m effective) |
| **IMU** | Integrated IMU via `/robot_name/imu/data` |
| **Drive System** | Differential drive — NEMA17 stepper motors + quadrature encoders |
| **Motor Driver** | DRV8833 (or equivalent) — PWM via ESP32 LEDC channels |
| **Power System** | 3S LiPo 11.1V with dedicated 5V regulated supply for compute |
| **Robot Footprint** | 44 cm × 38 cm rectangular chassis |
| **Max Linear Velocity** | 0.5 m/s |
| **Communication** | Wi-Fi 802.11n to fleet server; UART serial to ESP32 |
| **GPU (Simulation/Inference)** | NVIDIA RTX 4060 (used for CUDA-accelerated MPPI planning) |

---

## ⚙️ Mechanical & Hardware Design

### Mechanical Design Process
The robot chassis was designed with a modular approach, focusing on ease of maintenance and structural integrity. The design transitioned from 2D schematics to 3D SolidWorks models, followed by physical fabrication using 3D printing for structural components.

<div align="center">
<img src="./media/assets/mechanical_design.png" alt="Mechanical Design Process" width="90%"/>
</div>

### Differential Drive Assembly
The locomotion system utilizes a differential drive mobile base with NEMA17 stepper motors and closed-loop encoder feedback for precise odometry.

<div align="center">
<img src="./media/assets/motor_wheel_assembly.png" alt="Differential Drive Assembly" width="70%"/>
</div>

---

## 📊 System Modeling & Simulation

### Mathematical Modeling
Extensive system modeling was performed using MATLAB/Simulink to validate the kinematics and control logic before deployment.

<div align="center">
<img src="./media/assets/system_modeling.png" alt="System Modeling in Simulink" width="90%"/>
</div>

### Simulation Environment
The system was rigorously tested in a **Gazebo Hospital World**, allowing for the evaluation of multi-robot scenarios, navigation stacks, and perception pipelines in a safe, reproducible environment.

<div align="center">
<img src="./media/assets/simulation_environment.png" alt="Gazebo Simulation Environment" width="90%"/>
</div>

---

## 🧭 Navigation Stack

MARS uses the full **Nav2** navigation framework configured for multi-robot deployment, with every parameter file using a `robot_name` substitution token — meaning a **single shared YAML drives all robots** while maintaining independent namespaced topics and TF frames.

### Global Planner — Smac Hybrid-A\*

The global planner uses `nav2_smac_planner/SmacPlannerHybrid` with:
- **Reeds-Shepp motion model** — considers reverse arcs for kinematically feasible paths
- **72 angle quantization bins** for fine heading resolution
- **0.2 m minimum turning radius** enforcing smooth car-like curves
- Integrated **path smoother** (1000 iterations, w_smooth=0.3, w_data=0.2)
- `reverse_penalty: 2.0` to prefer forward driving while still allowing reverse when necessary

### Local Planner — MPPI Controller (GPU-Accelerated)

The local planner uses `nav2_mppi_controller::MPPIController` with **CUDA acceleration** targeting an NVIDIA RTX 4060:

| Parameter | Value | Purpose |
|---|---|---|
| `batch_size` | 600 | Trajectory samples per planning cycle |
| `time_steps` | 32 | Planning horizon steps |
| `model_dt` | 0.1 s | Time per step (3.2 s lookahead) |
| `device` | `cuda` | GPU acceleration enabled |
| `motion_model` | `DiffDrive` | Differential drive kinematics |
| `vx_max` | 0.5 m/s | Maximum forward velocity |
| `wz_max` | 1.9 rad/s | Maximum angular velocity |

#### MPPI Critic Stack (8 Active Critics)

The MPPI controller evaluates each sampled trajectory against 8 simultaneous cost critics:

| Critic | Weight | Role |
|---|---|---|
| `ConstraintCritic` | 4.0 | Kinematic feasibility enforcement |
| `CostCritic` | 3.81 | Costmap obstacle avoidance (collision cost: 1,000,000) |
| `GoalCritic` | 5.0 | Progress toward goal position |
| `GoalAngleCritic` | 3.0 | Final heading alignment |
| `PathAlignCritic` | 6.0 | Global path alignment (highest weight) |
| `PathFollowCritic` | 5.0 | Staying close to the reference path |
| `PathAngleCritic` | 2.0 | Heading consistency along path |
| `PreferForwardCritic` | 5.0 | Penalizes unnecessary backward motion |

> The `CostCritic` includes footprint-aware collision checking and short-circuits trajectory evaluation early when a collision is detected, saving GPU compute.

### Costmap Design — Asymmetric Inflation Strategy

A deliberate asymmetry is applied between the two costmap layers:

| | Local Costmap | Global Costmap |
|---|---|---|
| **Inflation Radius** | 0.55 m | 0.9 m |
| **Cost Scaling Factor** | 5.0 | 10.0 |
| **Obstacle Layer** | VoxelLayer (3D) | ObstacleLayer (2D) |
| **Update Rate** | 4.0 Hz | 5.0 Hz |
| **Purpose** | Responsive local avoidance | Conservative long-range planning |

The local costmap uses a **VoxelLayer** (16 voxels tall, 0.2 m z-resolution, max 2 m height) for true 3D obstacle representation, while the global costmap uses a flat 2D ObstacleLayer for efficient long-range path computation.

### Keepout Filter — Restricted Zones

Both costmaps apply a **KeepoutFilter** plugin loading a `keepout_mask.yaml` file, enforcing hard no-go zones (e.g., restricted wards, operating rooms, hazardous areas). This is configured per-robot via namespaced `/robot_name/costmap_filter_info` and `/robot_name/keepout_filter_mask` topics.

### Recovery Behaviors

The **Behavior Server** provides three recovery plugins executed by the Nav2 Behavior Tree when navigation fails:

- **Spin** — rotates in place to clear local costmap
- **BackUp** — reverses to escape tight situations
- **Wait** — pauses for dynamic obstacles to clear

The Behavior Tree (`navigate_to_pose_w_replanning_and_recovery.xml`) orchestrates replanning and recovery automatically, with 30+ BT node plugins loaded including `nav2_is_battery_low_condition_bt_node` for battery-triggered behaviors.

---

## 👁️ Semantic Perception Pipeline (YOLO → Costmap)

One of MARS's core contributions is the direct integration of AI object detection into the Nav2 navigation planner. The pipeline works as follows:

```
RGB-D Camera
     │
     ├──► Color Image ──► YOLOv8 Detection ──► Bounding Boxes + Class Labels
     │                                                      │
     └──► Depth Image ──► Depth Association ──► 3D Point Projection
                                                            │
                                              Camera Frame → Base Frame → Map Frame (via TF)
                                                            │
                                              Published as PointCloud2
                                              on /robot_name/detected_object_pose
                                                            │
                                              VoxelLayer: yolo_marking observation source
                                                            │
                                              Nav2 Local Costmap (2s persistence)
                                                            │
                                              MPPI Controller applies semantic costs
```

### Key Design Details

- **Bounding box → depth association:** Median depth is sampled from the central 50% of depth pixels within each bounding box, rejecting boundary artifacts and occluded background pixels.
- **3D back-projection** uses camera intrinsics (focal length, principal point) from the `CameraInfo` topic to convert `(u, v, depth)` → `(X, Y, Z)` in camera frame.
- **TF transformation** converts the 3D point from camera frame → `base_link` → global `map` frame using AMCL pose.
- **Class-specific inflation radii** are applied: humans receive 0.8 m clearance (proxemic safety), furniture 0.3 m, unknown objects 0.5 m.
- **2-second obstacle persistence** (`observation_persistence: 2.0`) keeps detected humans visible in the costmap even if briefly missed by the detector.
- **Complementary source roles:** YOLO **marks** obstacles, laser scan **clears** them — preventing ghost obstacles from lingering.

---

## 🔬 YOLO Depth Fusion Node (`obstacle_detector.py`)

The semantic perception layer is implemented as a standalone **ROS 2 Python node** (`YoloDepthFusion`) that bridges the AI detection pipeline directly into the Nav2 costmap system. This node is the concrete implementation of the pipeline described above.

### Development Stack
- **Language:** Python 3.10+
- **Framework:** ROS 2 Humble (`rclpy`)
- **AI Model:** Ultralytics YOLOv8 (`yolov8n.pt` by default, configurable)
- **Computer Vision:** OpenCV (`cv2`) + NumPy
- **ROS ↔ OpenCV bridge:** `cv_bridge`
- **Point cloud generation:** `sensor_msgs_py.point_cloud2`
- **Sensor synchronization:** `message_filters.ApproximateTimeSynchronizer`

### Node Architecture

```
                ┌──────────────────────────────────────────────┐
                │            YoloDepthFusion Node               │
                │                                              │
  kinect/rgb  ──┤─► ApproximateTimeSynchronizer                │
  kinect/depth ─┤        (slop=0.6s, queue=10)                 │
                │              │                               │
                │              ▼                               │
                │       image_callback()                       │
                │              │                               │
                │    ┌─────────┴──────────┐                   │
                │    ▼                    ▼                    │
                │  cv_bridge            cv_bridge              │
                │  BGR8 image        depth passthrough         │
                │    │                    │                    │
                │    ▼                    │                    │
                │  YOLO inference         │                    │
                │  (filtered by          │                    │
                │   target_objects)       │                    │
                │    │                    │                    │
                │    ▼                    ▼                    │
                │  Bounding box ──► Depth pixel sample         │
                │  centroid (u,v)    at (cx, cy)               │
                │                        │                    │
                │              3D back-projection              │
                │         (Kinect intrinsics fx=fy=525)        │
                │                        │                    │
                │         ┌──────────────┼─────────────┐      │
                │         ▼              ▼              ▼      │
                │    PointCloud2      Marker        Debug img  │
                └─────────┬──────────────┬──────────────┬─────┘
                          │              │              │
                          ▼              ▼              ▼
               detected_object_pose  detected_     yolo_debug_
               (→ VoxelLayer costmap) object_marker   image
```

### Key Technical Features

**Approximate Time Synchronization**
- Uses `message_filters.ApproximateTimeSynchronizer` with a 0.6-second slop tolerance to align RGB and depth frames that may arrive at slightly different timestamps — essential for real sensor hardware where streams are not perfectly synchronized.

**Configurable ROS 2 Parameters**
```python
# All tunable at launch or via parameter server
self.declare_parameter('yolo_model', 'yolov8n.pt')          # Swap model freely
self.declare_parameter('target_objects', ['bottle', 'cup',
                        'chair', 'person', 'keyboard'])      # Filter detections
self.declare_parameter('confidence_threshold', 0.5)          # Min YOLO confidence
```

**Namespace-Aware Frame ID**
The node dynamically resolves its ROS 2 namespace at runtime and constructs the correct TF frame:
```python
ns = self.get_namespace().strip('/')
fixed_frame_id = f"{ns}/camera_depth_optical_frame" if ns else "camera_depth_optical_frame"
```
This ensures the published PointCloud2 messages carry the correct namespaced frame ID (e.g., `robot1/camera_depth_optical_frame`), making them valid for TF lookup in a multi-robot environment.

**Depth Validity Filtering**
Before projecting a detection into 3D, the node enforces strict depth validity gates:
```python
if not np.isnan(dist) and dist > 0.4 and dist < 8.0:
```
- Rejects NaN values (common near depth camera edges)
- Enforces minimum 0.4 m (below the camera's reliable range)
- Enforces maximum 8.0 m (beyond sensor accuracy)
- Handles both `16UC1` (millimeter integer) and float depth encodings automatically

**3D Back-Projection (Kinect Intrinsics)**
```python
fx, fy = 525.0, 525.0      # Kinect focal lengths (pixels)
cx_opt, cy_opt = 319.5, 239.5   # Principal point (image center)

x = (u - cx_opt) * z / fx   # X in camera frame (meters)
y = (v - cy_opt) * z / fy   # Y in camera frame (meters)
```
The bounding box centroid `(u, v)` and sampled depth `z` are back-projected to `(x, y, z)` in the camera optical frame using the pinhole camera model.

**Dual Output — Costmap + Visualization**

| Publisher | Topic | Type | Consumer |
|---|---|---|---|
| `obj_pub` | `detected_object_pose` | `PointCloud2` | Nav2 VoxelLayer (`yolo_marking` source) |
| `marker_pub` | `detected_object_marker` | `Marker` (SPHERE, r=0.2m) | RViz2 visualization |
| `debug_img_pub` | `yolo_debug_image` | `Image` | Live bounding box preview |

The PointCloud2 output feeds directly into the Nav2 local costmap's `yolo_marking` observation source (configured in `nav2_params_multi.yaml`), where it marks obstacle cells with 2-second persistence.

**BEST_EFFORT QoS** is used for sensor topic subscriptions, matching the typical QoS profile of RGB-D camera drivers to prevent connection mismatches.

---

## 📡 Sensor Fusion — EKF

The `robot_localization` Extended Kalman Filter (EKF) runs at **20 Hz** in 2D mode, fusing two sensor streams for robust, drift-resistant localization:

### Odometry Source (`odom0`)
- Topic: `/robot_name/wheel/odometry`
- Contributes: **position (x, y, z)** and **linear/angular velocities**
- Differential mode: `false` (absolute odometry used)

### IMU Source (`imu0`)
- Topic: `/robot_name/imu/data`
- Contributes: **roll, pitch, yaw**, all **angular velocities**, all **linear accelerations**
- `imu0_relative: true` — orientation treated as relative change (no magnetometer dependency)
- Gravity removal enabled (`imu0_remove_gravitational_acceleration: true`)

### EKF Configuration

| Parameter | Value |
|---|---|
| Filter frequency | 20 Hz |
| Mode | 2D (planar) |
| World frame | `robot_name/odom` |
| State vector size | 15 states (x, y, z, roll, pitch, yaw + derivatives + accelerations) |
| Acceleration limits | Linear: 1.3 m/s², Angular: 3.4–4.5 rad/s² |

The fused odometry output feeds directly into AMCL for particle filter pose estimation, and is consumed by the Nav2 controller server for velocity command feedback.

---

## 🗺️ SLAM & Localization

The robots utilize **SLAM Toolbox** for real-time mapping and **AMCL** for precise localization. A key contribution is the **Multi-Robot Map Merging** capability, allowing multiple robots to build and share a unified map of the environment.

<div align="center">
<img src="./media/assets/slam_localization.png" alt="SLAM and Localization Details" width="90%"/>
</div>

### SLAM Toolbox Configuration
- **Solver:** Ceres Solver with `SPARSE_NORMAL_CHOLESKY` + Levenberg-Marquardt trust region
- **Loop closure:** Enabled — minimum chain size of 10 nodes, fine response threshold 0.45
- **Map resolution:** 0.05 m/cell
- **Max laser range:** 12.0 m
- **Mode:** Online asynchronous mapping

### AMCL Localization
- **Motion model:** `nav2_amcl::DifferentialMotionModel`
- **Laser model:** `likelihood_field`
- **Particle count:** 500–2000 (adaptive)
- **Transform tolerance:** 3.0 s (accommodates multi-robot TF latency)
- **Update thresholds:** 0.25 m linear / 0.2 rad angular (prevents unnecessary particle updates)

---

## 🌳 Transformation Trees (TF)

The MARS project utilizes a structured coordinate frame system to manage spatial relationships between the world, robots, and their individual sensors.

### 1. Multi-Agent Configuration (With Namespacing)
In a multi-robot setup, namespacing is critical to avoid frame ID collisions. Each robot operates within its own namespace (e.g., `robot1`, `robot2`), allowing the Fleet Management System to track multiple agents simultaneously in a global `world` frame.

<div align="center">
<img src="./media/screenshots/tf_tree_namespaced.png" alt="Namespaced TF Tree" width="100%"/>
<p><i>Figure: TF Tree for a 3-robot heterogeneous fleet with namespacing.</i></p>
</div>

**Key Characteristics:**
- **Global Root:** The `world` frame serves as the common origin for all agents.
- **Namespaced Frames:** Each robot has its own `map`, `odom`, and `base_link` prefixed with its unique ID (e.g., `robot1/base_link`).
- **Independent Odometry:** Each agent maintains its own localized odometry chain within its namespace.
- **Single shared config:** All robots use one `nav2_params_multi.yaml` file — the `robot_name` token is substituted at launch time per robot.

### 2. Single-Agent Configuration (Without Namespacing)
For individual robot testing or single-agent deployments, a standard non-namespaced TF tree is used. This simplifies the configuration for standalone tasks.

<div align="center">
<img src="./media/screenshots/tf_tree_no_namespacing.png" alt="Non-Namespaced TF Tree" width="80%"/>
<p><i>Figure: Standard TF Tree for a single MARS agent.</i></p>
</div>

**Key Characteristics:**
- **Standard ROS Convention:** Follows the `map` → `odom` → `base_link` hierarchy.
- **Direct Sensor Links:** Sensors like `lidar_link` and `imu_link` are attached directly to the `base_link`.
- **Simplified Logic:** Ideal for initial calibration and single-robot SLAM validation.

---

## 🧠 Fleet Management System

The Fleet Manager runs as a native ROS 2 node, orchestrating the entire multi-robot fleet through several coordinated subsystems.

### Battery-Aware Task Allocation

Robot selection uses a composite cost function that balances proximity and battery health:

```
combined_score = euclidean_distance(robot → pickup) − (battery_level / 10.0)
```

Robots are excluded from selection if:
- Battery ≤ 15% (critical threshold — must charge)
- Status is `error` or `recovering`
- Currently occupying a side spot
- Estimated task energy exceeds available battery minus safety margin

### Battery Management

| Threshold | Level | Behavior |
|---|---|---|
| Normal | > 30% | Standard operation |
| Warning | 15–30% | Orange indicator, tasks still accepted |
| Critical | ≤ 15% | Task blocked, auto-return to charging station triggered |
| Charging | At parking spot | 1%/second charge rate, only starts at critical threshold |
| Consumption | During movement | 0.05% per meter traveled (tracked from AMCL pose deltas) |

### Traffic De-confliction — Side Spot System

When a robot's target location is occupied (within 2.0 m of another robot), the Fleet Manager automatically de-conflicts:

1. Identifies the blocking robot
2. Cancels the blocking robot's current goal
3. Redirects it to a pre-mapped **side spot** adjacent to the contested location
4. The requesting robot proceeds to the now-clear destination
5. A **30-second timeout** returns the displaced robot to its original task

Five side spots are mapped to their corresponding main nodes (e.g., Lobby → Lobby Side, Ward A → Ward A Out).

### Collision Recovery

- Consecutive navigation failures are counted per robot
- At **5 failures**, a collision is declared
- Goal is cancelled and a recovery timer retries the last navigation goal
- Up to **3 retry attempts** before the task is dropped and requeued
- Status transitions: `busy` → `recovering` → `idle` (on success) or `error` (on max retries)

### Task Lifecycle

```
PENDING → ASSIGNED → IN_PROGRESS → COMPLETED
                  ↘              ↗
                    FAILED → reallocated to next available robot
```

- **Opportunistic dispatch:** If a robot is returning to parking and a new task arrives (and battery > 15%), the parking route is cancelled immediately and the task is dispatched.
- **Smart Skip:** If a chained task's pickup location matches the robot's current position, the pickup navigation phase is skipped entirely.
- **Dropoff-Only mode:** Tasks with no pickup navigate directly to the dropoff, supported in both CSV and JSON task formats.

### Task Input Formats

The fleet manager accepts two task submission formats simultaneously:

```
# CSV format (via /task_commands)
assign,robot1,Ward A,Room 1,T123456-1

# JSON format (via /fleet/submit_task)
{"pickup": "Ward A", "dropoff": "Room 1", "task_id": "T123456-1"}
```

---

## 🎮 Fleet Monitoring & Control

The **RoboFleet Command Center** is a desktop GUI providing centralized monitoring and control, built with **CustomTkinter** (falling back to standard Tkinter if unavailable).

<div align="center">
<img src="./media/assets/fleet_dashboard.png" alt="Fleet Monitoring Dashboard" width="90%"/>
</div>

### Dashboard Features
- **Dual-mode operation:** Runs standalone in **Mock Mode** (no ROS 2 required) or connects live in **ROS 2 Mode** (`--ros2` flag)
- **Per-robot cards:** Real-time status, battery progress bar with color-coded thresholds (green >30%, orange 15–30%, red <15%), and current location
- **Task submission form:** Robot selector (manual or auto-assign), pickup/dropoff dropdowns across 26 hospital waypoints, Dropoff-Only checkbox
- **Task queue panel:** Live view of pending, assigned, and in-progress tasks
- **Sidebar stats:** Total robots, active task count, fleet average battery

### Launch

```bash
# Mock mode (no ROS 2 needed)
python3 fleet_dashboard.py

# ROS 2 live mode
python3 fleet_dashboard.py --ros2
```

---

## 🔌 Embedded Control — ESP32 Firmware

The MARS project uses **two ESP32 microcontrollers**, each programmed using a different toolchain and methodology, reflecting two distinct embedded development approaches.

---

### Robot 1 — PlatformIO + Arduino Framework (`main.cpp`)

**KAMIL-01**'s embedded firmware is developed using **PlatformIO** inside VS Code with the **Arduino framework**, providing a high-level hardware abstraction layer combined with production-grade stepper and encoder libraries.

#### Toolchain & Development Environment

| Tool | Role |
|---|---|
| **VS Code + PlatformIO extension** | IDE and build system |
| **Arduino framework** | Hardware abstraction (GPIO, Serial, timers) |
| **micro_ros_platformio** | Micro-ROS client library for Arduino/ESP32 |
| **FastAccelStepper** | High-performance stepper control with hardware timer ISRs |
| **ESP32Encoder** | Quadrature encoder reading via hardware pulse counter (PCNT) |

#### Pin Mapping

```
Right Motor:  STEP → GPIO26  |  DIR → GPIO27
Left Motor:   STEP → GPIO13  |  DIR → GPIO4
Enable:       GPIO14  (shared, TB6560 driver)

Left Encoder:  A → GPIO34  |  B → GPIO39  (input-only GPIOs)
Right Encoder: A → GPIO35  |  B → GPIO36  (input-only GPIOs)
```

> GPIO 34–39 are input-only on the ESP32 and have no internal pull-up resistors — the encoder board must supply external pull-ups.

#### Robot Physical Parameters

```cpp
WHEEL_DIAMETER_M = 0.066f    // 66 mm wheels
WHEEL_BASE_M     = 0.160f    // 160 mm track width
TICKS_PER_REV    = 1600.0f   // encoder ticks per full wheel revolution
METERS_PER_TICK  = π × 0.066 / 1600 ≈ 0.0001297 m/tick
```

#### Micro-ROS Integration

The firmware participates in the ROS 2 graph as a native node under the `robot1` namespace:

```
Node:      /robot1/diff_drive
Publishes: /robot1/odom        (nav_msgs/Odometry)  @ 20 Hz
Subscribes: /robot1/cmd_vel   (geometry_msgs/Twist)
```

The node waits for the Micro-ROS agent to be available before initializing (`rmw_uros_ping_agent` loop), ensuring clean startup without partial initialization. Frame IDs are assigned using `rosidl_runtime_c__String__assign()` for safe C-string memory management.

#### Control Loop Architecture

The firmware runs **two concurrent callbacks** managed by the `rclc_executor`:

```
┌─────────────────────────────────────────────────────┐
│                  rclc_executor                      │
│                                                     │
│  cmd_vel subscriber ──► cmd_vel_cb()                │
│       (ON_NEW_DATA)      │                          │
│                          ▼                          │
│                  Differential drive IK:             │
│                  vL = v − ω·L/2                     │
│                  vR = v + ω·L/2                     │
│                  vel_to_hz() → FastAccelStepper     │
│                                                     │
│  odom_timer (20 Hz) ──► odom_timer_cb()             │
│                          │                          │
│                          ├─ Watchdog: stop if no    │
│                          │  cmd_vel for > 500 ms    │
│                          │                          │
│                          ├─ Read encoder deltas      │
│                          │  (ESP32Encoder PCNT)     │
│                          │                          │
│                          ├─ Dead-reckoning odometry │
│                          │  (midpoint integration)  │
│                          │                          │
│                          └─ Publish odom msg        │
└─────────────────────────────────────────────────────┘
```

#### Velocity → Stepper Frequency Conversion

```cpp
// Convert m/s → stepper Hz using physical calibration
float ticks_per_s = v_mps / METERS_PER_TICK;
int32_t hz = clamp(|ticks_per_s|, 0, 8000);  // MAX_HZ_LIMIT = 8000 Hz safety cap

// Deadband: below 5 Hz treated as zero to avoid motor stutter
if (hz < MIN_HZ_DEADBAND) stepperX->stopMove();
```

#### Odometry — Midpoint Integration

The odometry computation uses the **midpoint Runge-Kutta integration** method for improved accuracy over simple Euler integration:

```cpp
float dC = 0.5f * (dL + dR);        // linear displacement (m)
float dT = (dR - dL) / WHEEL_BASE;  // heading change (rad)
float th_mid = theta + 0.5f * dT;   // midpoint heading

x_pos += dC * cos(th_mid);
y_pos += dC * sin(th_mid);
theta += dT;

// Quaternion from yaw (2D robot)
odom.orientation.z = sin(theta / 2);
odom.orientation.w = cos(theta / 2);
```

#### Safety Features
- **500 ms watchdog timer** — motors halt automatically if no `cmd_vel` is received within the timeout (communication loss protection)
- **8000 Hz stepper cap** (`MAX_HZ_LIMIT`) — hardware safety ceiling for the TB6560 driver
- **5 Hz deadband** (`MIN_HZ_DEADBAND`) — prevents motor chatter at near-zero commands
- **AutoEnable** on FastAccelStepper — motor driver enables automatically on movement and disables on stop, reducing heat and power draw

---

### Robot 2 — ESP-IDF + Bare-Metal Dual-Core Programming

**KAMIL-02**'s embedded firmware is developed using the **ESP-IDF framework** directly in VS Code (via the Espressif IDF extension), bypassing the Arduino abstraction layer entirely. This approach gives full control over FreeRTOS task scheduling, hardware peripheral registers, and core affinity.

#### Toolchain & Development Environment

| Tool | Role |
|---|---|
| **VS Code + ESP-IDF extension** | IDE, build system (CMake), flash & monitor |
| **ESP-IDF (Espressif IoT Development Framework)** | Native RTOS + hardware driver layer |
| **FreeRTOS** | Real-time task scheduling with explicit core pinning |
| **Micro-ROS ESP-IDF component** | Micro-ROS client for ESP-IDF projects |
| **ESP-IDF peripheral drivers** | LEDC (PWM), PCNT (encoder), UART, ADC — all accessed directly |

#### Dual-Core Task Partitioning

The ESP32's two Xtensa LX6 cores are explicitly partitioned to isolate real-time hardware tasks from ROS 2 communication:

```
┌─────────────────────────────┐    ┌─────────────────────────────┐
│         Core 0              │    │         Core 1              │
│   (Real-Time Control)       │    │   (ROS 2 Communication)     │
│                             │    │                             │
│  ┌─────────────────────┐    │    │  ┌─────────────────────┐   │
│  │  Motor Control Task │    │    │  │  Micro-ROS Spin Task│   │
│  │  Priority: HIGH     │    │    │  │  Priority: NORMAL   │   │
│  │  Period: 10 ms      │    │    │  │  (event-driven)     │   │
│  │                     │    │    │  │                     │   │
│  │  - LEDC PWM update  │    │    │  │  - UART transport   │   │
│  │  - PID computation  │    │    │  │  - DDS-XRCE encode/ │   │
│  │  - Watchdog check   │    │    │  │    decode           │   │
│  └─────────────────────┘    │    │  │  - Publish /odom    │   │
│                             │    │  │  - Receive /cmd_vel │   │
│  ┌─────────────────────┐    │    │  └─────────────────────┘   │
│  │  Encoder ISR Task   │    │    │                             │
│  │  (PCNT interrupt)   │    │    │  ┌─────────────────────┐   │
│  │  - Quadrature count │    │    │  │  Battery Monitor    │   │
│  │  - Tick accumulation│    │    │  │  Priority: LOW      │   │
│  └─────────────────────┘    │    │  │  - ADC sampling     │   │
│                             │    │  │  - Voltage publish  │   │
│  ┌─────────────────────┐    │    │  └─────────────────────┘   │
│  │  Safety Monitor     │    │    │                             │
│  │  - Over-current ISR │    │    └─────────────────────────────┘
│  │  - Encoder fault    │    │
│  └─────────────────────┘    │
└─────────────────────────────┘
```

#### Bare-Metal Hardware Programming Features

**LEDC (PWM) for Motor Control**
Motor speed is controlled via direct ESP-IDF LEDC API calls, bypassing any Arduino layer:
```c
// Direct ESP-IDF LEDC configuration
ledc_channel_config_t motor_ch = {
    .gpio_num   = MOTOR_PIN,
    .speed_mode = LEDC_HIGH_SPEED_MODE,
    .channel    = LEDC_CHANNEL_0,
    .timer_sel  = LEDC_TIMER_0,
    .duty       = 0,
};
ledc_channel_config(&motor_ch);
ledc_set_duty(LEDC_HIGH_SPEED_MODE, LEDC_CHANNEL_0, duty_cycle);
ledc_update_duty(LEDC_HIGH_SPEED_MODE, LEDC_CHANNEL_0);
```

**PCNT (Pulse Counter) for Encoders**
Quadrature encoder decoding uses ESP-IDF's hardware PCNT peripheral directly — no software interrupt polling:
```c
pcnt_config_t enc_cfg = {
    .pulse_gpio_num = ENC_A_PIN,
    .ctrl_gpio_num  = ENC_B_PIN,
    .channel        = PCNT_CHANNEL_0,
    .unit           = PCNT_UNIT_0,
    .pos_mode       = PCNT_COUNT_INC,
    .neg_mode       = PCNT_COUNT_DEC,
    .lctrl_mode     = PCNT_MODE_REVERSE,
    .hctrl_mode     = PCNT_MODE_KEEP,
};
pcnt_unit_config(&enc_cfg);
```

**FreeRTOS Task Creation with Core Pinning**
```c
// Pin motor control to Core 0 (real-time priority)
xTaskCreatePinnedToCore(
    motor_control_task,   // task function
    "MotorCtrl",          // name
    4096,                 // stack size
    NULL,                 // parameters
    configMAX_PRIORITIES - 1,  // highest priority
    NULL,
    0                     // Core 0
);

// Pin Micro-ROS communication to Core 1
xTaskCreatePinnedToCore(
    microros_spin_task,
    "MicroROSSpin",
    8192,
    NULL,
    5,
    NULL,
    1                     // Core 1
);
```

**Inter-Core Communication via FreeRTOS Queues**
Velocity commands received on Core 1 (Micro-ROS) are passed to Core 0 (motor control) via a thread-safe FreeRTOS queue, preventing data races without mutexes:
```c
// Core 1: receive cmd_vel, enqueue
xQueueSend(cmd_vel_queue, &twist_msg, 0);

// Core 0: dequeue and apply
xQueueReceive(cmd_vel_queue, &local_twist, portMAX_DELAY);
apply_motor_commands(local_twist);
```

#### Comparison: PlatformIO/Arduino vs. ESP-IDF Bare-Metal

| Aspect | KAMIL-01 (PlatformIO/Arduino) | KAMIL-02 (ESP-IDF Bare-Metal) |
|---|---|---|
| **Abstraction level** | High (Arduino HAL) | Low (direct register/driver access) |
| **Stepper control** | FastAccelStepper library | Custom LEDC PWM + timer ISR |
| **Encoder reading** | ESP32Encoder library | PCNT hardware peripheral directly |
| **RTOS scheduling** | Arduino loop + rclc_executor | FreeRTOS tasks with explicit core affinity |
| **Core isolation** | Single execution context | Hard Core 0 / Core 1 partitioning |
| **Build system** | PlatformIO (ini config) | ESP-IDF (CMake) |
| **Development speed** | Faster iteration | More control, more setup |
| **Real-time guarantee** | Soft (shared loop) | Hard (Core 0 dedicated, highest priority) |



The entire MARS software stack runs inside a **Docker container**, ensuring a consistent, reproducible environment with no manual dependency management.

### Prerequisites
- Docker Engine 24.0+
- NVIDIA Container Toolkit (for GPU-accelerated MPPI planning)
- Host: Ubuntu 22.04 recommended

### Quick Start

```bash
# Clone the repository
git clone https://github.com/Asoomkamel/MARS-Multi-Agent-Robotic-System.git
cd MARS-Multi-Agent-Robotic-System

# Build the Docker image
docker build -t mars-ros2 .

# Run the container (with GPU and display support)
docker run -it --rm \
  --gpus all \
  --network host \
  --env DISPLAY=$DISPLAY \
  --volume /tmp/.X11-unix:/tmp/.X11-unix \
  --device /dev/ttyUSB0:/dev/ttyUSB0 \
  mars-ros2

# Inside the container — launch the full multi-robot stack
ros2 launch mars_bringup multi_robot.launch.py robot_names:="[robot1,robot2,robot3]"

# Launch the fleet dashboard (separate terminal)
python3 fleet_dashboard.py --ros2
```

> **Note:** Pass `--device /dev/ttyUSB0` (or the appropriate port) to expose the ESP32 serial connection to the container. Use `xhost +local:docker` on the host before launching if RViz2 or Gazebo windows do not appear.

### Container Contents
- ROS 2 Humble + Nav2 full stack
- SLAM Toolbox + robot_localization (EKF)
- YOLOv8 (Ultralytics) + Intel RealSense SDK
- Micro-ROS agent
- Fleet Manager + Dashboard GUI dependencies

---

## 📡 ROS 2 Topic Reference

| Topic | Message Type | Direction | Description |
|---|---|---|---|
| `/task_commands` | `std_msgs/String` (CSV) | Dashboard → Fleet Manager | Task assignment in CSV format |
| `/fleet/submit_task` | `std_msgs/String` (JSON) | Dashboard → Fleet Manager | Task assignment in JSON format |
| `/fleet_dashboard/status` | `std_msgs/String` (JSON) | Fleet Manager → Dashboard | Full robot state at 2 Hz |
| `/fleet_battery_status` | `std_msgs/String` (JSON) | Fleet Manager → Dashboard | Battery levels at 1 Hz |
| `/fleet_status` | `std_msgs/String` | Fleet Manager → Monitor | Human-readable fleet status |
| `/robot_name/detected_object_pose` | `sensor_msgs/PointCloud2` | YOLO node → Costmap | Semantic obstacle positions |
| `/robot_name/amcl_pose` | `geometry_msgs/PoseWithCovarianceStamped` | AMCL → Fleet Manager | Robot localization pose |
| `/robot_name/scan` | `sensor_msgs/LaserScan` | RGB-D → Nav2 | Depth-to-LaserScan converted data |
| `/robot_name/navigate_to_pose` | `nav2_msgs/action/NavigateToPose` | Fleet Manager → Nav2 | Navigation action goals |
| `/robot_name/cmd_vel_nav` | `geometry_msgs/Twist` | MPPI → Micro-ROS | Velocity commands to ESP32 |
| `/robot_name/wheel/odometry` | `nav_msgs/Odometry` | ESP32 → EKF | Wheel encoder odometry |
| `/robot_name/imu/data` | `sensor_msgs/Imu` | IMU → EKF | Raw IMU measurements |
| `/robot_name/map` | `nav_msgs/OccupancyGrid` | Map Server → Nav2 | Static map for navigation |
| `/robot_name/costmap_filter_info` | `nav2_msgs/CostmapFilterInfo` | Filter Server → Costmap | Keepout zone filter metadata |

---

## 🔄 Operational Logic

The system's operational flow is governed by an **Asynchronous State Machine (ASM)**, coordinating everything from user requests to low-level hardware triggers like RFID door opening.

<div align="center">
<img src="./media/assets/operational_logic.png" alt="System Operational Logic" width="70%"/>
</div>

---

## 🎬 Demo Videos

| Demo | Description | Link |
|------|-------------|------|
| 🗺️ **SLAM Mapping** | Two robots building a shared map in real-time | [Watch →](https://drive.google.com/drive/folders/1-9L6LYLV3B_W_U3SomK1VmOPT8XbXjZW) |
| 🚗 **Autonomous Navigation** | Multi-robot nav with MPPI controller | [Watch →](https://drive.google.com/drive/folders/1-9L6LYLV3B_W_U3SomK1VmOPT8XbXjZW) |
| 👁️ **YOLOv8 Detection** | Live bounding box detection with RGB-D fusion | [Watch →](https://drive.google.com/drive/folders/1-9L6LYLV3B_W_U3SomK1VmOPT8XbXjZW) |
| 🎙️ **Voice HRI** | Natural language command execution | [Watch →](https://drive.google.com/drive/folders/1-9L6LYLV3B_W_U3SomK1VmOPT8XbXjZW) |
| 🔑 **RFID Door Access** | Automated secure door entry | [Watch →](https://drive.google.com/drive/folders/1-9L6LYLV3B_W_U3SomK1VmOPT8XbXjZW) |

---

## 👥 Team

| Name | Student ID |
|------|-----------|
| Mutasim Makin Al-Kamil  " Team's Leader"  | 202270192 |
| Osama Muhammed Maaodhah | 202270222 |
| Mamdouh Abdul Fattah Al-Samei | 202270377 |
| Hussain Abdul Moeen Al-Samei | 202270380 |

**Supervisor:** Prof. Dr. Abdulraqib Abdo Asaad  
**Institution:** Sana'a University — Faculty of Engineering

---

<div align="center">

**⭐ If this project inspired you, please give it a star!**

*Built with passion by engineering students from Yemen 🇾🇪*

</div>
