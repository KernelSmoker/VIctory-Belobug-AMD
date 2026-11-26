# Issue #174: isolate the guarded-view test's temporary pin

Issue: https://github.com/Belobug/VIctory-Belobug-AMD/issues/174  
KernelSmoker branch commit: `23981c4858920743552d6364a7d4598f5e4d9f54`

The `kernel_reads_into_guarded_blocks_after_opening` test now removes pin entries wholly inside its temporary mapped view after unmapping it. This prevents that test's two-second write pin from contaminating another parallel test if Windows reuses the same virtual address. Production remap behavior is unchanged.

Validation:

- `cargo check -p victory-gpu --tests --target x86_64-pc-windows-gnu --locked --offline` passed.
- Built the Windows GNU test binary using MinGW tools in a temporary Fedora container; no host packages were installed.
- Ran the `watch::tests` subset five times under GE-Proton 11-12 on Linux with eight test threads: 7 passed per run, 0 failed (35 total test executions).
- No D3D12/GPU test was run for this change. Correctable PCIe AER messages continue to appear on the host, so GPU work remained stopped after the earlier bounded PR #220 smoke test.
