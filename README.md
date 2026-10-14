# Auto Flow ICS - Turret Command Center

## Overview
The Auto Flow Command Center is a lightweight, web-based telemetry and control interface designed for a prototype Industrial Control System (ICS). It provides remote administration for a physical fire-suppression turret, allowing operators to monitor live video feeds, toggle infrared (IR) sensor logic, and manually actuate physical water pumps.



https://github.com/user-attachments/assets/49b7c399-6c1e-4c03-948c-6cb130cc031f



## System Architecture
* **Backend:** Python (Flask API routing, threading, and hardware GPIO/PWM control)
* **Computer Vision:** OpenCV (CV2) thermal blob detection via HSV color filtering
* **Hardware Integration:** Raspberry Pi, Picamera2, Pan/Tilt servos, 3-zone IR sensor array, water pump relay
* **Frontend:** HTML5, CSS3, Vanilla JavaScript

## Core Capabilities
* **Dual-Mode Operations:** Seamless toggling between 'Autonomous' threat-scanning and 'Manual' operator override.
* **Kinetic Trigger Discipline:** Pump actuation is bounded by a strict error-margin deadband to prevent accidental discharge from servo jitter.
* **Concurrency Management:** Implementation of `threading.Lock()` to prevent race conditions when the API and the autonomous scanner attempt to access the hardware simultaneously.
* **Asynchronous Telemetry:** Fetch-based API calls prevent frontend blocking during heavy hardware actuation.

## ⚠️ Security Posture & Vulnerability Roadmap
*This project was initially developed as a functional hardware prototype. It is currently undergoing a security refactor to harden the architecture against adversarial exploitation.*

### Known Vulnerabilities & Planned Mitigations
1. **Cleartext Telemetry & Hardcoded Secrets:**
   * **Threat:** The web control interface operates over HTTP, and the admin credentials (`USER_CREDENTIALS = {"admin": "cyber2026"}`) are hardcoded directly into the Python script. An attacker on the network can easily sniff the password or reverse-engineer the firmware to steal the credential.
   * **Mitigation:** Wrap the Python web server in a reverse proxy (e.g., Nginx) configured with TLS/HTTPS. Migrate all hardcoded secrets to an encrypted environment variable vault.
2. **Denial of Service (DoS) via API Flooding:**
   * **Threat:** An attacker could rapidly loop unauthenticated POST requests to `/api/camera/move` or `/api/pump/on`, causing servo burnout, flooding the physical environment, or crashing the Python backend.
   * **Mitigation:** Implement strict rate-limiting on the backend logic, and integrate a JWT (JSON Web Token) authentication middleware for all state-changing endpoints.
3. **Cross-Site Request Forgery (CSRF):**
   * **Threat:** State-changing actions rely on basic POST requests without anti-CSRF tokens.
   * **Mitigation:** Integrate CSRF token validation on the backend for all API calls.
