# RX 9070 XT branch: bounded Linux/Proton game smoke test

Source tested: `Belobug/VIctory-Belobug-AMD`, branch `rx9070xt`, commit `81ebb7bdc9b7ca6ec2f3169973f50be77c7c803c`.

- Built `victory` with `cargo build -p victory --target x86_64-pc-windows-gnu --locked` using MinGW in a temporary Fedora container; no host packages were installed.
- Launched the build under GE-Proton 11-12 on Linux with the authorized local game installation.
- Runtime log showed game imports bound with no missing modules, shader translations/pipeline creation, and frame telemetry around 25.6 FPS with six draws per frame.
- Stopped after about 28 seconds on new correctable PCIe AER messages. No NVIDIA Xid was reported. Peak telemetry was about 52.4 °C and 4.2 GiB VRAM; after stopping, about 43.8 °C and 1.5 GiB.
- This confirms a short runtime/rendering smoke test only. The run ended before a main-menu result could be confirmed; it is not a full gameplay test or benchmark. Raw local logs and personal paths were not published.
