<div align="center">

# 🤖 MARS: Multi-Agent Robotic System
### Design and Implementation of Smart Navigation and Coordination System for AI-Driven Heterogeneous Multi-Agent Robotic Systems

[![ROS 2 Humble](https://img.shields.io/badge/ROS_2-Humble-blue?logo=ros&logoColor=white)](https://docs.ros.org/en/humble/)
[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![C++](https://img.shields.io/badge/C++-17-00599C?logo=c%2B%2B&logoColor=white)](https://isocpp.org/)
[![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![YOLOv8](https://img.shields.io/badge/YOLOv8-Ultralytics-FF6600?logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0iI0ZGNjYwMCIgZD0iTTEyIDJMMiA3djEwbDEwIDUgMTAtNVY3bC0xMC01em0wIDQuNWw3IDMuNS03IDMuNS03LTMuNSA3LTMuNXptLTggNS41bDYgM3Y3bC02LTN2LTd6bTEwIDEwdj03bDYtM3Y3bC02IDN6Ii8+PC9zdmc+)](https://github.com/ultralytics/ultralytics)

**Bachelor's Graduation Project — Sana'a University, Faculty of Engineering**  
*Electrical Engineering · Computer and Control Engineering · 2026*

---

<!-- BANNER 1 -->
<img src="https://raw.githubusercontent.com/Asoomkamel/MARS-Multi-Agent-Robotic-System/main/media/assets/banner_1.png" alt="MARS Project Overview Banner" width="100%"/>

---

<!-- BANNER 2 -->
<img src="https://raw.githubusercontent.com/Asoomkamel/MARS-Multi-Agent-Robotic-System/main/media/assets/banner_2.png" alt="MARS Project Objectives and Tools Banner" width="100%"/>

</div>

---

## 📌 Project Overview

This project presents the design and implementation of an **intelligent navigation and coordination system** for heterogeneous, AI-driven multi-agent robotic platforms. Designed for complex indoor environments such as hospitals, hotels, and warehouses, the system integrates centralized fleet coordination with distributed edge intelligence.

### Key Features:
- **Autonomous Navigation:** Robust multi-robot coordination and traffic management.
- **AI Perception:** YOLOv8-based obstacle avoidance and semantic understanding.
- **Human-Robot Interaction:** Voice-driven commands using Whisper and Gemini AI.
- **Secure Access:** RFID-based door mechanism for authorized zone entry.
- **Fleet Management:** Smart task allocation and adaptive workload distribution.

---

## 🤖 Physical Prototype

The project culminated in the construction of two functional robotic units, **KAMIL-01** and **KAMIL-02**, featuring a custom-built chassis and integrated sensor suites.

<div align="center">
<img src="https://raw.githubusercontent.com/Asoomkamel/MARS-Multi-Agent-Robotic-System/main/media/assets/robot_photo_real.png" alt="KAMIL-01 and KAMIL-02 Robots" width="80%"/>
<p><i>KAMIL-01 and KAMIL-02: The physical realization of the MARS project.</i></p>
</div>

---

## 🏗️ System Architecture

The system follows a **Three-Tier Hierarchical Architecture**, ensuring a clear separation between high-level supervisory tasks, mid-level bridge processing, and low-level real-time control.

<div align="center">
<img src="https://raw.githubusercontent.com/Asoomkamel/MARS-Multi-Agent-Robotic-System/main/media/assets/system_architecture.png" alt="System Architecture" width="90%"/>
</div>

1.  **Supervisory Layer (Cloud/Server):** Handles the Fleet Management System (FMS), User Interface, and high-level task orchestration.
2.  **Bridge Layer (Raspberry Pi 5):** Manages ROS 2 nodes, global planning, AI perception (YOLOv8), and data compression.
3.  **Real-Time Layer (ESP32):** Executes low-level motor control, PID loops, and sensor feedback via Micro-ROS.

---

## ⚙️ Mechanical & Hardware Design

### Mechanical Design Process
The robot chassis was designed with a modular approach, focusing on ease of maintenance and structural integrity. The design transitioned from 2D schematics to 3D SolidWorks models, followed by physical fabrication using 3D printing for structural components.

<div align="center">
<img src="https://raw.githubusercontent.com/Asoomkamel/MARS-Multi-Agent-Robotic-System/main/media/assets/mechanical_design.png" alt="Mechanical Design Process" width="90%"/>
</div>

### Differential Drive Assembly
The locomotion system utilizes a differential drive mobile base with NEMA17 stepper motors and closed-loop encoder feedback for precise odometry.

<div align="center">
<img src="https://raw.githubusercontent.com/Asoomkamel/MARS-Multi-Agent-Robotic-System/main/media/assets/motor_wheel_assembly.png" alt="Differential Drive Assembly" width="70%"/>
</div>

---

## 📊 System Modeling & Simulation

### Mathematical Modeling
Extensive system modeling was performed using MATLAB/Simulink to validate the kinematics and control logic before deployment.

<div align="center">
<img src="https://raw.githubusercontent.com/Asoomkamel/MARS-Multi-Agent-Robotic-System/main/media/assets/system_modeling.png" alt="System Modeling in Simulink" width="90%"/>
</div>

### Simulation Environment
The system was rigorously tested in a **Gazebo Hospital World**, allowing for the evaluation of multi-robot scenarios, navigation stacks, and perception pipelines in a safe, reproducible environment.

<div align="center">
<img src="https://raw.githubusercontent.com/Asoomkamel/MARS-Multi-Agent-Robotic-System/main/media/assets/simulation_environment.png" alt="Gazebo Simulation Environment" width="90%"/>
</div>

---

## 🗺️ SLAM & Localization

The robots utilize **SLAM Toolbox** for real-time mapping and **AMCL** for precise localization. A key contribution is the **Multi-Robot Map Merging** capability, allowing multiple robots to build and share a unified map of the environment.

<div align="center">
<img src="https://raw.githubusercontent.com/Asoomkamel/MARS-Multi-Agent-Robotic-System/main/media/assets/slam_localization.png" alt="SLAM and Localization Details" width="90%"/>
</div>

---

## 🎮 Fleet Monitoring & Control

The **RoboFleet Command Center** provides a centralized interface for monitoring robot availability, battery levels, and mission states, while allowing for manual task submission and emergency overrides.

<div align="center">
<img src="https://raw.githubusercontent.com/Asoomkamel/MARS-Multi-Agent-Robotic-System/main/media/assets/fleet_dashboard.png" alt="Fleet Monitoring Dashboard" width="90%"/>
</div>

---

## 🔄 Operational Logic

The system's operational flow is governed by an **Asynchronous State Machine (ASM)**, coordinating everything from user requests to low-level hardware triggers like RFID door opening.

<div align="center">
<img src="https://raw.githubusercontent.com/Asoomkamel/MARS-Multi-Agent-Robotic-System/main/media/assets/operational_logic.png" alt="System Operational Logic" width="70%"/>
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
