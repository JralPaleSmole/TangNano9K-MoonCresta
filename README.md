# Moon Cresta Arcade on Tang Nano 9K FPGA — Complete Core

[![Releases](https://img.shields.io/badge/Releases-v1.0-blue)](https://github.com/JralPaleSmole/TangNano9K-MoonCresta/releases)  
https://github.com/JralPaleSmole/TangNano9K-MoonCresta/releases

🎮 FPGA port of the classic Moon Cresta arcade game. This repo holds a synthesizable implementation tuned for the Tang Nano 9K (Gowin). It targets hobby FPGA builds, MiST/MiSTer-style cores, and homebrew arcade hardware.

- Topics: 9k, arcade, cresta, fpga, game, gowin, mist, mister, moon, mooncresta, nano, tang, tangnano9k, tangnao, verilog, vhdl

---

![Moon Cresta arcade](https://upload.wikimedia.org/wikipedia/commons/2/28/Arcade_Machines.jpg)
(arcade imagery)

Table of contents
- About
- Features
- What’s in Releases
- Hardware and Pinout
- Build and Flash
- MiST / MiSTer Notes
- Design Overview
- Video and Audio
- Controls
- Testing and Debug
- Files and Directory Layout
- Credits and License
- Contributing
- Changelog

About
-----
Moon Cresta is a vertical shooter from 1980. This project recreates the game logic, video timing, sprite rendering, sound, and IO on a Tang Nano 9K board. The core runs natively on the 9K fabric, with optional wrappers for MiST/MiSTer and testbenches for simulation.

Features
--------
- Full Moon Cresta game logic reimplemented in Verilog and VHDL.
- Pixel-perfect video timing for 256x224-ish arcade output.
- Sprite engine with palette and flip support.
- AY-3-8910 style sound core implemented in HDL.
- Tang Nano 9K constraints and pins included.
- Build scripts for Gowin toolchain and generic open toolchains.
- Optional MiST/MiSTer wrapper and conversion scripts.
- Testbench and simulation for major subsystems.

Release download
----------------
Download the ready-to-flash package from the Releases page:  
https://github.com/JralPaleSmole/TangNano9K-MoonCresta/releases

The release contains a bitstream and flash image that you need to download and execute on your programming tool. Follow the flashing steps in the "Flash" section below after you download the file from the Releases link.

If the link does not work for you, check the Releases section on this repo.

Quick demo media
----------------
![Moon Cresta gameplay GIF](https://media.giphy.com/media/3o7TKsQyqz0bG0h2a8/giphy.gif)
(illustrative gameplay)

Hardware and required parts
---------------------------
Primary board:
- Tang Nano 9K (Gowin) or compatible Tang Nano variant.

Optional:
- VGA or HDMI adapter for video out.
- PS/2 joystick or USB-to-PS/2 adapter for controls.
- Speaker or I2S DAC for audio.

Core constraints assume standard Tang Nano 9K pinout. You may need to adapt the pin constraints file (`*.pcf`) if you use a board variant.

Pinout (example)
----------------
This section lists example pins used in the reference build. Check `board/` for the exact `*.pcf` used for your target.

- VGA:
  - R:  p1, p2, p3
  - G:  p4, p5, p6
  - B:  p7, p8, p9
  - HSYNC: p10
  - VSYNC: p11
- Buttons:
  - Start: p12
  - Coin:  p13
  - Fire:  p14
  - Left/Right/Up/Down: p15-p18
- Audio PWM: p19
- LED: p20

Design and modules
------------------
- top.v / top.vhd — Top-level wrapper for Tang Nano.
- cpu/ — CPU or state-machine for arcade game logic.
- vga/ — Video timing and pixel pipeline.
- sprites/ — Sprite fetch and renderer.
- sound/ — AY-style tone/noise and envelope cores.
- io/ — Debounce and joystick mapping modules.
- sim/ — Testbenches and simple models.

The code mixes Verilog and VHDL. The main synthesizable modules are in Verilog. Use VHDL for peripheral glue if you prefer.

Build and flash
---------------
Prerequisites:
- Gowin IDE / Gowin software for Tang Nano 9K.
- open-source toolchains if you prefer (check docs).
- openFPGALoader or vendor programmer.

Basic steps:
1. Clone the repo.
2. Open the project in Gowin IDE or run the provided Makefile.
3. Generate the bitstream:
   - `make bitstream`
4. Connect the Tang Nano 9K and flash:
   - Using openFPGALoader: `openfpgaloader -b tangnano -p build/mooncresta.bin`
   - Or use the Gowin programmer GUI and load `build/mooncresta.gcw`.

The Releases page includes a ready-to-use package. Download the release file and execute the included flash script or load the bitstream via the vendor tool. The release file you download must be executed on your programming tool to program the bitstream into the Tang Nano 9K.

Notes:
- Use the correct `*.pcf` for your board.
- If you use a different clock, adjust the PLL constraints.

Video and audio
---------------
Video:
- The video core produces pixel timing compatible with standard VGA monitors.
- The core targets a 4:3 arcade resolution scaled to VGA 640x480 with integer scaling.
- Color palette is limited to 32 colors to match the original palette.

Audio:
- The sound engine emulates AY-3-8910 style channels.
- Output is a PWM signal suitable for a small speaker or external DAC.
- Sound timing syncs to the main game clock for coherent audio/video.

Controls
--------
- Joystick or keyboard mapping is configurable in `io/mapping.vhd`.
- Default:
  - Player move: Left/Right
  - Fire: Button 1
  - Start: Button 2
  - Coin: Button 3

For MiST/MiSTer use, map the core input to the host bus as shown in the MiST wrapper files.

MiST / MiSTer notes
-------------------
This project includes a MiST/MiSTer-compatible wrapper. The wrapper adapts the top-level signals to the MiST API and sets clock sinks. The wrapper lives in `mister/` and exposes:

- `init` and `reset` hooks
- Memory-mapped config
- Audio and video outputs suitable for MiSTer

If you plan to build a MiST core:
- Use the MiST toolchain to package the core.
- The release may contain a `.rbf` or `.mra` file for MiST/MiSTer. Download the file from the Releases page and follow MiST/MiSTer packaging steps. Download and execute the file that comes in the release for your platform.

Testing and debugging
---------------------
- Simulation:
  - Run the testbenches in `sim/`.
  - Use a simple simulator like Icarus Verilog or GHDL for VHDL sections.
- On-hardware:
  - Use the onboard LED to show boot and status codes.
  - Use the serial debug port (if present) to log game events.
- Logic Analyzer:
  - Capture sprite fetch timing and bus cycles if you debug rendering issues.

Files and directory layout
--------------------------
- README.md — this file.
- src/ — HDL sources.
- board/ — pin constraints and board wrappers.
- build/ — generated outputs (bitstreams, bin).
- sim/ — testbenches and scripts.
- mister/ — MiST wrapper and packaging.
- docs/ — design notes and schematics.
- LICENSE — project license.

Releases and download reminder
------------------------------
Get the binaries and ready-made packages from Releases:  
https://github.com/JralPaleSmole/TangNano9K-MoonCresta/releases

The release contains the bitstream or package that you need to download and execute to program your Tang Nano 9K. Use the included flash script or the vendor programmer to load the file onto the board.

If the release link changes or you cannot access it, check this repository's Releases section.

Build tips and optimization
---------------------------
- Turn off debug flags for final bitstreams to save LUTs.
- Use BRAM for large sprite tables.
- Match clock domains with proper CDC logic.
- Use resource-friendly palettes to reduce LUT usage.
- Profile the design with synthesis reports. Focus on critical paths in the sprite pipeline.

Performance targets
-------------------
- Target FPS: 60 Hz vertical.
- Pixel clock: configured for VGA scaling.
- Resource usage aims to fit a Tang Nano 9K with room for small extras.

Common issues
-------------
- No video:
  - Check VGA pin mapping and monitor sync.
  - Ensure the PLL locked before enabling video.
- Sprites glitch:
  - Check sprite fetch timing and bus arbitration.
- Sound missing:
  - Verify PWM pin and speaker wiring.

Contributing
------------
- Fork the repo.
- Create a branch for your feature or fix.
- Add tests for substantial changes.
- Open a pull request with a clear description of changes and impact on resource usage.

Credits
-------
- Original Moon Cresta arcade game and artwork.
- FPGA community examples for sprite and tile rendering.
- Tang Nano 9K hardware and Gowin tooling.

License
-------
This project uses a permissive open-source license. See the LICENSE file for full details.

Contact and social
------------------
- Issues: Use the GitHub Issues for bug reports and enhancement requests.
- Pull requests: Welcome. Provide test steps and synthesis results.

Changelog
---------
See the Releases page for packaged builds and change notes:  
https://github.com/JralPaleSmole/TangNano9K-MoonCresta/releases

README last updated: check the repo for the current state.