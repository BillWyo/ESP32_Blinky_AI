# Blinky using AI — ESP32 DevKitC V4

## Project Idea
Create a simple LED blink program for an ESP32 DevKitC V4 using AI assistance (Claude Code) to generate the code, manage the GitHub workflow, and handle all compile and upload operations entirely through terminal commands — with the goal of learning the full embedded development workflow from code to running hardware.

## Plan
1. Create a new PlatformIO project targeting the ESP32
2. Write a blink sketch using the onboard LED
3. Initialize a git repository and push to GitHub
4. Compile the firmware using PlatformIO CLI
5. Upload to the ESP32 over USB
6. Verify on hardware via serial monitor

---

## Terminal Operations

### 1. Project Setup
```powershell
New-Item -ItemType Directory -Path "c:\Users\johan\Documents\PlatformIO\Projects\ESP32_Blinky_AI\src" -Force
```
Created the PlatformIO project folder structure.

---

### 2. Git Initialization
```powershell
cd "c:\Users\johan\Documents\PlatformIO\Projects\ESP32_Blinky_AI"
git init
git add .
git commit -m "Initial commit - ESP32 Blinky project"
```
Initialized local git repository and made the first commit.

---

### 3. GitHub Remote & Push
```powershell
git remote add origin https://github.com/BillWyo/ESP32_Blinky_AI.git
git branch -M main
git push -u origin main
```
Linked to GitHub and pushed. Authenticated via Personal Access Token.

---

### 4. Compile
```powershell
& "C:\Users\johan\.platformio\penv\Scripts\pio.exe" run
```
**SUCCESS** — 6.5% RAM, 20.3% Flash used.

---

### 5. Upload to ESP32
```powershell
& "C:\Users\johan\.platformio\penv\Scripts\pio.exe" run --target upload
```
Auto-detected **COM4**, flashed in ~12 seconds. **SUCCESS**

---

### 6. Serial Monitor
```powershell
& "C:\Users\johan\.platformio\penv\Scripts\pio.exe" device monitor --port COM4 --baud 115200
```
Confirmed output: `LED ON` / `LED OFF` every 500ms.

---

### 7. Stop Serial Monitor & Re-upload (LED pin fix)
```powershell
& "C:\Users\johan\.platformio\penv\Scripts\pio.exe" run --target upload
```
Released COM4, re-flashed after reverting to GPIO 2. **SUCCESS** — LED confirmed blinking.

---

## Key Facts

| Item | Value |
|---|---|
| Board | ESP32-D0WD-V3 rev 3.0 |
| Upload port | COM4 |
| Baud rate | 115200 |
| LED pin | GPIO 2 |
| GitHub repo | https://github.com/BillWyo/ESP32_Blinky_AI |
| PlatformIO CLI | `C:\Users\johan\.platformio\penv\Scripts\pio.exe` |
