# PR #220: bindless write-range validation on Linux/Proton

PR: https://github.com/Belobug/VIctory-Belobug-AMD/pull/220  
PR head tested: `078c6b16923cf3a65fcaf5e9feb69ecfabbb7617`

## Build and focused test

- `cargo check --locked --offline --target x86_64-pc-windows-gnu -p victory-gpu --lib` passed.
- Built the focused Windows GNU test binary in a temporary Fedora container with MinGW tools; no host packages were installed.
- Ran `descriptor::host::tests::bindless_plan_is_remade_only_when_slots_change` with GE-Proton 11-12 on Linux. VKD3D-Proton selected the NVIDIA GeForce RTX 3090 (driver 620.73.4); result: 1 passed, 0 failed, 116 filtered out (0.68 s).

The focused unit test creates a D3D12 host device and exercises bindless plan rebuilding on a descriptor-slot change. It does not directly assert that changing a UAV write range changes the plan's write range, so a dedicated assertion for that exact regression would strengthen the PR's coverage.

## Bounded game smoke test

Built the PR revision's `victory` debug executable and launched it with an authorized local title installation through GE-Proton 11-12. The log shows shader translations, pipeline creation, title asset reads, and frame reports around 29.8–38.2 FPS with six draws per frame and zero host-pipeline drops. This was a short smoke test, not a full gameplay test or performance benchmark.

Stopped the run after new correctable PCIe AER messages appeared. No NVIDIA Xid was reported. Peak observed GPU temperature was about 48 °C and VRAM use about 6.6 GiB; after stopping, usage returned to about 1.4 GiB. The AER messages are recorded as a stop condition, not attributed to this code change. Raw local logs and personal paths were not published.

## Result

The Windows GNU library build and focused Proton/VKD3D GPU test passed. The game smoke test reached rendering but was deliberately stopped on PCIe AER activity; it does not establish stable extended gameplay. The GPU run exercised the PR build, but does not isolate the changed UAV write-range behavior.
