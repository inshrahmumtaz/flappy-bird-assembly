# 🐦 Flappy Bird — x86 MASM Assembly

> A fully playable Flappy Bird clone written entirely in x86 Assembly Language  
> using MASM, Irvine32, and the Windows Console API.

![Platform](https://img.shields.io/badge/Platform-Windows-blue)
![Language](https://img.shields.io/badge/Language-x86%20MASM%20Assembly-red)
![Library](https://img.shields.io/badge/Library-Irvine32-green)
![Build](https://img.shields.io/badge/Build-Visual%20Studio%202019%2F2022-purple)
![Type](https://img.shields.io/badge/Project-Group%20Project-orange)

---

## 👥 Group Members

| Name | Roll Number |
|------|-------------|
| Fiza Bint-e-Ejaz | 241940 |
| Inshrah Mumtaz | 241862 |
| Abdul Hannan Qureshi | 241859 |

**Course:** Computer Organization and Assembly Language  
**Instructor:** Lect. Romana Maroof  
**Institute:** Air University Islamabad — CS & IT Department

---

## 🎮 About The Project

This is a group project developed as part of our Computer Organization
and Assembly Language (COAL) course. The entire game — physics, rendering,
input handling, collision detection, and timing — is implemented at the
register and instruction level in pure x86 assembly.

Instead of a graphics engine, the game uses an **80×25 Windows console**
as its display surface, with ASCII/block characters representing the bird,
pipes, and ground.

---

## ✨ Features

- 🐦 Smooth bird physics using the **x87 FPU** (gravity + jump impulse)
- 🟩 Scrolling pipes with **randomized gap positions**
- 💥 **AABB collision detection** using floating-point comparisons
- 🖥️ **Double-buffered rendering** — 2000-byte char + color arrays
- ⌨️ **Non-blocking keyboard input** — game loop never stalls
- 🏆 **Live score + best score** tracking
- 🎨 Colored console output — yellow bird, green pipes, cyan sky
- 📺 Title screen, active gameplay, and game over overlays

---

## 🕹️ Controls

| Key | Action |
|-----|--------|
| `SPACE` | Start game |
| `SPACE` | Flap (jump upward) |
| `SPACE` | Restart after game over |

---

## 🛠️ Built With

| Tool | Purpose |
|------|---------|
| MASM (ML.EXE) | Microsoft Macro Assembler |
| Visual Studio 2019/2022 | IDE and build system |
| Irvine32.lib | Console I/O library |
| kernel32.lib | Windows API |
| x87 FPU | All physics and collision math |
| Windows Console | 80×25 display surface |

---

## 🚀 How To Build & Run

### Requirements
- Windows 10 / 11
- Visual Studio 2019 or 2022
  with **"Desktop development with C++"** workload
- Irvine32 library (included in repo)
- Windows 10 SDK

### Steps

**1. Clone the repository**
```bash
git clone https://github.com/YOUR_USERNAME/flappy-bird-assembly.git
cd flappy-bird-assembly
```

**2. Open Visual Studio**
```
File → New → Project → Empty Project (C++)
Name: FlappyBird
```

**3. Add main.asm**
```
Right-click Source Files → Add → Existing Item → main.asm
```

**4. Enable MASM**
```
Project → Build Dependencies → Build Customizations
☑ masm (.targets, .props) → OK
```

**5. Set file type**
```
Right-click main.asm → Properties
Item Type → Microsoft Macro Assembler → OK
```

**6. Set platform to 32-bit**
```
Build → Configuration Manager → Platform → x86 → Close
```

**7. Set include path**
```
Project Properties
→ Microsoft Macro Assembler → General
→ Include Paths → [path to project folder]
```

**8. Set library paths**
```
Project Properties → Linker → General
→ Additional Library Directories → add:
   [path to project folder]
   C:\Program Files (x86)\Windows Kits\10\Lib\10.0.XXXXX.0\um\x86
```

**9. Add libraries**
```
Linker → Input → Additional Dependencies → add:
   Irvine32.lib
   kernel32.lib
   user32.lib
```

**10. Disable SAFESEH**
```
Linker → Advanced
→ Image Has Safe Exception Handlers → No (/SAFESEH:NO)
```

**11. Build and Run**
```
Build → Rebuild Solution    (Ctrl+Alt+F7)
Debug → Start Without Debugging    (Ctrl+F5)
```

---

## 🏗️ Project Architecture

```
GAME LOOP (main)
├── ProcessInput        ← non-blocking SPACE key check
├── UpdatePhysicsAsm    ← FPU gravity + jump every frame
├── UpdatePipes         ← move, score, remove, spawn pipes
│   └── CheckCollision  ← AABB per pipe
└── RenderFrame
    ├── ClearScreenBuffer   ← REP STOSB fills 2000 cells
    ├── DrawPipesToBuffer   ← nested loop: pipe→col→row
    ├── DrawBirdToBuffer    ← '>' rising, 'v' falling
    ├── DrawScoreToBuffer   ← decimal conversion
    ├── DrawOverlays        ← title / game over screens
    └── FlushScreenBuffer   ← Gotoxy+SetTextColor+WriteChar
```

---

## ⚙️ Physics Constants

| Variable | Value | Description |
|----------|-------|-------------|
| gravity | 0.18 | Added to velocity every frame |
| jumpStrength | -1.05 | Velocity on SPACE press |
| pipeSpeed | 0.8 | Columns scrolled per frame |
| frameDelay | 33ms | ~30 FPS timing |
| gapSize | 7.0 rows | Pipe opening height |

---

## 📁 File Structure

```
flappy-bird-assembly/
│
├── main.asm          ← entire game source code
├── Irvine32.inc      ← Irvine32 include file
├── Irvine32.lib      ← Irvine32 static library
├── kernel32.lib      ← Windows kernel library
├── user32.lib        ← Windows user library
└── README.md         ← this file
```

---

## 📚 Assembly Concepts Used

- `PROC` / `ENDP` — 20 modular procedures
- `PUSHAD` / `POPAD` — full register preservation
- x87 FPU — `FLD`, `FADD`, `FSTP`, `FCOMP`, `FSTSW`, `SAHF`
- `REP STOSB` — fast 2000-cell buffer clear
- `REP MOVSB` — in-place pipe array shift
- Conditional jumps — `JZ`, `JNZ`, `JE`, `JNE`, `JA`, `JB`
- `STRUCT` — 16-byte aligned Pipe structure
- Win32 API — `GetConsoleCursorInfo`, `SetConsoleCursorInfo`
- Irvine32 — `ReadKey`, `Gotoxy`, `SetTextColor`, `GetMseconds`

---

## 📄 License

This project was developed for academic purposes at  
**Air University Islamabad** as part of the COAL course.
