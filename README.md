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

> [!WARNING]
> **PERMISSION REQUESTED** — Tool: `PowerShell` — Create new directory

```powershell
New-Item -ItemType Directory -Path "c:\Users\johan\Documents\PlatformIO\Projects\ESP32_Blinky_AI\src" -Force
```
Created the PlatformIO project folder structure.

---

### 2. Git Initialization

> [!WARNING]
> **PERMISSION REQUESTED** — Tool: `Bash` — Run git init, add, commit

```powershell
cd "c:\Users\johan\Documents\PlatformIO\Projects\ESP32_Blinky_AI"
git init
git add .
git commit -m "Initial commit - ESP32 Blinky project"
```
Initialized local git repository and made the first commit.

---

### 3. GitHub Remote & Push

> [!WARNING]
> **PERMISSION REQUESTED** — Tool: `Bash` — Add remote origin and push to GitHub

```powershell
git remote add origin https://github.com/BillWyo/ESP32_Blinky_AI.git
git branch -M main
git push -u origin main
```
Linked to GitHub and pushed. Authenticated via Personal Access Token.

---

### 4. Compile

> [!WARNING]
> **PERMISSION REQUESTED** — Tool: `PowerShell` — Run PlatformIO build

```powershell
& "C:\Users\johan\.platformio\penv\Scripts\pio.exe" run
```
**SUCCESS** — 6.5% RAM, 20.3% Flash used.

---

### 5. Upload to ESP32

> [!WARNING]
> **PERMISSION REQUESTED** — Tool: `PowerShell` — Flash firmware to ESP32 on COM4

```powershell
& "C:\Users\johan\.platformio\penv\Scripts\pio.exe" run --target upload
```
Auto-detected **COM4**, flashed in ~12 seconds. **SUCCESS**

---

### 6. Serial Monitor

> [!WARNING]
> **PERMISSION REQUESTED** — Tool: `PowerShell` — Open serial monitor on COM4

```powershell
& "C:\Users\johan\.platformio\penv\Scripts\pio.exe" device monitor --port COM4 --baud 115200
```
Confirmed output: `LED ON` / `LED OFF` every 500ms.

---

### 7. Stop Serial Monitor & Re-upload (LED pin fix)

> [!WARNING]
> **PERMISSION REQUESTED** — Tool: `PowerShell` — Re-flash firmware after LED pin fix

```powershell
& "C:\Users\johan\.platformio\penv\Scripts\pio.exe" run --target upload
```
Released COM4, re-flashed after reverting to GPIO 2. **SUCCESS** — LED confirmed blinking.

---

## Permissions & Bypass Mode

Claude Code asks for permission before running each tool. This can be bypassed when you know exactly what Claude is about to do.

### Enabling Bypass Permissions

**Via settings.json** (`C:\Users\johan\.claude\settings.json`):
```json
{
  "permissions": {
    "defaultMode": "bypassPermissions"
  }
}
```

**At startup:**
```powershell
claude --dangerously-skip-permissions
```

**Via chat:**
Type `/config` and set `defaultMode` to `bypassPermissions`.

### Permission Modes

| Mode | Behavior |
|---|---|
| `default` | Prompts for each new tool or command |
| `acceptEdits` | Auto-allows file edits, prompts for Bash |
| `bypassPermissions` | No prompts — Claude runs everything automatically |

### When It Is Safe to Bypass

| Scenario | Safe? | Reason |
|---|---|---|
| Compiling firmware | Yes | Code already reviewed, fully reversible |
| Uploading firmware to your own board | Yes | Known code, personal hardware |
| Routine git commit & push (personal repo) | Yes | Low risk, you control the repo |
| Creating a new project structure | Yes | Fully reversible |
| Running serial monitor | Yes | Read-only, nothing is changed |
| Reformatting or renaming files | Yes | Cosmetic, reversible |
| Deleting files or folders | **No** | Irreversible — one wrong path loses work |
| Pushing to a shared or production repo | **No** | Affects other people |
| Running unfamiliar scripts | **No** | Unknown behavior |
| Modifying system files or OS config | **No** | Can destabilize your machine |
| Installing packages | **No** | Supply chain attack risk |
| Flashing unknown firmware | **No** | Can brick hardware |

### Rule of Thumb

> Bypass when you know exactly what Claude is about to do.
> Keep prompts on when exploring, experimenting, or working near anything shared or irreversible.

For this project — compile, upload, serial monitor, and git commits are all good candidates for bypass mode.

---

## Terminal Color & Symbol Reference

Claude Code uses ANSI color codes and symbols in the terminal to communicate status at a glance. The table below explains each color and symbol used throughout this project.

### Colors

| Color | ANSI Code | Meaning | Used For |
|---|---|---|---|
| Yellow (Bright) | `\e[93m` | Caution / Attention | Permission requests, warnings |
| Green | `\e[32m` | Success | Build passed, upload complete |
| Red | `\e[31m` | Failure / Error | Build errors, upload failures |
| Cyan | `\e[36m` | Information | Status messages, serial output |
| White (Default) | `\e[0m` | Reset | Returns terminal to normal color |

### Symbols

| Symbol | Meaning |
|---|---|
| `***` | High priority alert (e.g. permission requested) |
| `=` (repeated) | Section divider / visual separator |
| `[SUCCESS]` | Operation completed without errors |
| `[FAILED]` | Operation encountered an error |
| `>>>` | Progress indicator during flash/upload |
| `*` (percentage) | Memory usage bar during compile |

### Permission Hook Marker

Whenever Claude Code needs your approval to run a command, a bright yellow banner appears:

```
============================================================
  *** PERMISSION REQUESTED ***  Tool: Bash
============================================================
```

This is triggered by the `PermissionRequest` hook configured in `~/.claude/settings.json`. The tool name shown tells you exactly which tool is asking for permission before you approve or deny it.

### ANSI Color Code Quick Reference

| Code | Effect |
|---|---|
| `\e[0m` | Reset all formatting |
| `\e[1m` | Bold |
| `\e[31m` | Red text |
| `\e[32m` | Green text |
| `\e[33m` | Yellow text |
| `\e[36m` | Cyan text |
| `\e[91m` | Bright red |
| `\e[92m` | Bright green |
| `\e[93m` | Bright yellow |
| `\e[96m` | Bright cyan |

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
