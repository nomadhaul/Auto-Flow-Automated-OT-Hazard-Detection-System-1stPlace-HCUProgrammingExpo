# Auto Flow: Edge-to-Cloud Automated Fire Mitigation System
**1st Place Winner – HCU Programming Expo**

> An Industrial Control System (ICS) prototype integrating hardware-in-the-loop (Raspberry Pi, IR sensors, PWM servos) with a secure public AWS gateway via an encrypted reverse SSH tunnel.

---



https://github.com/user-attachments/assets/c37c1b32-3678-4583-9db2-c69440c5149b



---

## System Architecture

The system operates across two core engineering pillars:

### 1. Hardware & Edge Computing
* **Local Processing:** Raspberry Pi executing an object-oriented Python 3 daemon.
* **Interrupt-Driven Actuation:** Active-low hardware interrupts from an Infrared sensor array trigger immediate physical suppression sequences via PWM servos and 5V relays.
* **Concurrency:** Thread-safe resource locks isolate physical hardware actuation from concurrent web telemetry requests.

### 2. Secure Cloud Gateway
* **Encrypted Perimeter:** The local edge device establishes a persistent, outbound reverse SSH tunnel to an AWS EC2 instance (Ubuntu), eliminating the need for exposed inbound local ports.
* **Cloud Dashboard:** A lightweight Flask web dashboard provides remote telemetry and live video streaming (OpenCV / PiCamera2).
* **Access Control:** HTTP Basic Authentication mitigates automated bot discovery and restricts manual override controls to authenticated operators.

## Tech Stack
* **Language & Runtime:** Python 3 (OOP, Multithreading, Thread Locking)
* **Computer Vision:** OpenCV, PiCamera2
* **Web & Ingress:** Flask, Reverse SSH Tunneling
* **Edge Hardware:** Raspberry Pi, RPi.GPIO, `gpiozero`, Active-Low IR Sensors, PWM Servos, 5V Relay
* **Cloud Infrastructure:** AWS EC2 (Ubuntu Linux)

## Security Controls
* **Zero-Trust Default State:** The system boots into a restricted monitoring state; manual hardware overrides are disabled until authenticated.
* **Inbound Hardening:** The local network requires zero open inbound firewall ports; all cloud communication is tunneled outbound via SSH keys.
* **Race-Condition Mitigation:** Mutex locks prevent race conditions between asynchronous sensor poll loops and external web socket requests.

## Deployment & Setup

> **Note:** Requires physical GPIO configuration. Verify pin mappings against your schematic before running.

1. Clone the repository:
   ```bash
   git clone [https://github.com/michaelmorelli/Auto-Flow.git](https://github.com/michaelmorelli/Auto-Flow.git)
   cd Auto-Flow
