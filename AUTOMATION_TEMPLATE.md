# [PROJECT_NAME] — Automated Embedded Development Guide

## How to Use This Template
Replace all `[PLACEHOLDERS]` with your project-specific values. Each section maps to a phase of the automated development workflow. Permission markers show where Claude Code will request approval — decide in advance whether to bypass or approve manually.

---

## Project Idea
_Describe the goal of the project in 1-2 sentences. What does it do? What hardware does it target?_

> Example: Create a [FEATURE] program for a [BOARD_NAME] using AI assistance to generate code, manage GitHub, and handle compile/upload via terminal commands.

---

## Plan
1. Create a new PlatformIO project targeting `[BOARD_ID]`
2. Write the firmware for `[FEATURE]`
3. Initialize a git repository and push to GitHub
4. Compile the firmware using PlatformIO CLI
5. Upload to the board over USB
6. Verify on hardware via serial monitor
7. _Add any additional steps specific to your project_

---

## Phase 1 — Project Setup

> [!WARNING]
> **PERMISSION REQUESTED** — Tool: `PowerShell` — Create project directory

```powershell
New-Item -ItemType Directory -Path "[PROJECT_PATH]\src" -Force
```

**Files to create:**
- `platformio.ini` — board and framework configuration
- `src/main.cpp` — main firmware source

**platformio.ini template:**
```ini
[env:[BOARD_ID]]
platform = [PLATFORM]
board = [BOARD_ID]
framework = arduino
monitor_speed = [BAUD_RATE]
lib_deps =
    [LIBRARY_1]
    [LIBRARY_2]
```

_Common platforms: `espressif32`, `atmelavr`, `ststm32`_
_Common boards: `esp32dev`, `uno`, `megaatmega2560`, `nucleo_f401re`_

---

## Phase 2 — Git Initialization

> [!WARNING]
> **PERMISSION REQUESTED** — Tool: `Bash` — Initialize git repository

```powershell
cd "[PROJECT_PATH]"
git init
git add .
git commit -m "Initial commit - [PROJECT_NAME]"
```

**Before committing, ensure `.gitignore` contains:**
```
.pio/
.vscode/
*.pyc
__pycache__/
*.o
*.a
*.d
```

---

## Phase 3 — GitHub Remote & Push

> [!WARNING]
> **PERMISSION REQUESTED** — Tool: `Bash` — Push to GitHub remote

```powershell
git remote add origin https://github.com/[GITHUB_USERNAME]/[REPO_NAME].git
git branch -M main
git push -u origin main
```

**Prerequisites:**
- GitHub account signed in via VS Code
- Empty repo created at `github.com/[GITHUB_USERNAME]/[REPO_NAME]`
- Personal Access Token (PAT) generated with `repo` scope
- Git Credential Manager installed (`git config --global credential.helper manager`)

**Subsequent pushes:**
```powershell
git add .
git commit -m "[COMMIT_MESSAGE]"
git push
```

---

## Phase 4 — Compile

> [!WARNING]
> **PERMISSION REQUESTED** — Tool: `PowerShell` — Run PlatformIO build

```powershell
& "C:\Users\[USERNAME]\.platformio\penv\Scripts\pio.exe" run
```

**Expected output:**
```
[SUCCESS] Took [N] seconds
RAM:   [X%] (used [N] bytes from [TOTAL] bytes)
Flash: [X%] (used [N] bytes from [TOTAL] bytes)
```

**If build fails:**
- Check `lib_deps` in `platformio.ini` for missing libraries
- Run `pio lib install [LIBRARY_NAME]` to add dependencies
- Check for compiler errors in `src/main.cpp`

---

## Phase 5 — Upload to Board

> [!WARNING]
> **PERMISSION REQUESTED** — Tool: `PowerShell` — Flash firmware to board

```powershell
& "C:\Users\[USERNAME]\.platformio\penv\Scripts\pio.exe" run --target upload
```

**Expected output:**
```
Auto-detected: [COM_PORT]
Uploading...
Hash of data verified.
[SUCCESS] Took [N] seconds
```

**If upload fails:**
- Check board is connected via USB
- Verify correct COM port: `pio device list`
- Close serial monitor — it locks the COM port
- Hold BOOT button on board during upload if needed

---

## Phase 6 — Serial Monitor

> [!WARNING]
> **PERMISSION REQUESTED** — Tool: `PowerShell` — Open serial monitor

```powershell
& "C:\Users\[USERNAME]\.platformio\penv\Scripts\pio.exe" device monitor --port [COM_PORT] --baud [BAUD_RATE]
```

**To stop:** Press `Ctrl+C` — this releases the COM port for future uploads.

**Common baud rates:** `9600`, `115200`, `230400`

---

## Phase 7 — Verify & Iterate

Confirm the firmware behaves as expected on hardware. If changes are needed:

> [!WARNING]
> **PERMISSION REQUESTED** — Tool: `PowerShell` — Re-compile and re-flash

```powershell
# Edit src/main.cpp, then:
& "C:\Users\[USERNAME]\.platformio\penv\Scripts\pio.exe" run --target upload
```

Push changes to GitHub after verification:
```powershell
git add .
git commit -m "[DESCRIBE_CHANGE]"
git push
```

---

## Permissions Reference

### When to Bypass Permissions

| Scenario | Safe to Bypass? | Reason |
|---|---|---|
| Compiling firmware | Yes | Code already reviewed, reversible |
| Uploading to your own board | Yes | Known code, personal hardware |
| Routine git commit & push (personal repo) | Yes | Low risk, you control the repo |
| Creating project structure | Yes | Fully reversible |
| Running serial monitor | Yes | Read-only |
| Deleting files or folders | **No** | Irreversible |
| Pushing to shared/production repo | **No** | Affects others |
| Running unfamiliar scripts | **No** | Unknown behavior |
| Installing packages | **No** | Supply chain risk |
| Flashing unknown firmware | **No** | Can brick hardware |

### Enabling Bypass Mode

```json
{
  "permissions": {
    "defaultMode": "bypassPermissions"
  }
}
```
Save to `C:\Users\[USERNAME]\.claude\settings.json` for global effect, or `.claude\settings.json` inside the project folder for project-only scope.

> **Rule of thumb:** Bypass when you know exactly what Claude is about to do. Keep prompts on when exploring or working near anything shared or irreversible.

---

## Terminal Color & Symbol Reference

| Color | ANSI Code | Meaning |
|---|---|---|
| Bright Yellow | `\e[93m` | Permission requested / caution |
| Green | `\e[32m` | Success |
| Red | `\e[31m` | Error / failure |
| Cyan | `\e[36m` | Information / status |
| Reset | `\e[0m` | Return to default color |

| Symbol | Meaning |
|---|---|
| `[SUCCESS]` | Operation completed without errors |
| `[FAILED]` | Operation encountered an error |
| `***` | High priority alert |
| `=` (repeated) | Visual section divider |

---

## Key Facts

| Item | Value |
|---|---|
| Board | `[BOARD_NAME]` |
| Upload port | `[COM_PORT]` |
| Baud rate | `[BAUD_RATE]` |
| Key pin(s) | `[PIN_ASSIGNMENTS]` |
| GitHub repo | `https://github.com/[GITHUB_USERNAME]/[REPO_NAME]` |
| PlatformIO CLI | `C:\Users\[USERNAME]\.platformio\penv\Scripts\pio.exe` |

---

## Checklist

- [ ] Project folder and `platformio.ini` created
- [ ] `src/main.cpp` written and reviewed
- [ ] `.gitignore` in place
- [ ] Git repo initialized and first commit made
- [ ] GitHub repo created (empty)
- [ ] Code pushed to GitHub
- [ ] Firmware compiles without errors
- [ ] Firmware uploaded to board
- [ ] Behavior verified on hardware via serial monitor
- [ ] Final changes committed and pushed
