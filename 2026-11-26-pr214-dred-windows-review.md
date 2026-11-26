# PR #214: DRED diagnostics under async submission

PR: https://github.com/Belobug/VIctory-Belobug-AMD/pull/214  
PR head reviewed: `9d1f4c44c94027e7683c74b3d42088f1c6bdab43`

The change gathers the retained command lists from in-flight submissions and passes them to the existing DRED page-fault and breadcrumb formatter. It uses `try_lock` so a device-loss report does not block on the in-flight queue; if the queue is busy, list breadcrumbs may be omitted while the rest of the DRED data is still requested. I found no actionable source defect in that path.

Validation:

- `cargo check -p victory-gpu --release --features diagnostics --target x86_64-pc-windows-gnu --locked --offline` passed.
- This compile check includes the Windows release code that Linux-target checks omit.
- No device-loss runtime test or NVIDIA GPU run was performed. Correctable PCIe AER activity continues on the host, and the PR author also reports no induced DRED runtime capture.
