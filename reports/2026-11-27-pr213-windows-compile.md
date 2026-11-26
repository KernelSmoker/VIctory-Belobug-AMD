# PR #213: copystat interval diagnostics check

- PR: [#213](https://github.com/Belobug/VIctory-Belobug-AMD/pull/213), `ccbf8886e491e9ae40bc89bd9f02c510ea5392ff`
- Environment: Linux cross-compilation to `x86_64-pc-windows-gnu`; no game runtime or GPU execution.
- Command: `cargo check --target x86_64-pc-windows-gnu -p victory-gpu --features diagnostics`
- Result: passed, exit code 0. Existing dead-code warnings appeared for `victory-shader::Module::int_value` and `victory-gpu::profile::RSP`.
- Additional check: `cargo clippy --release -p victory-gpu --features diagnostics --no-deps` on Linux failed because this Windows-specific crate configuration cannot resolve `windows::Win32::Foundation::RECT` and `host_resource`; this is a host-target mismatch, not a Windows-target validation.
- Review note: `TIMED_FRAMES.swap` runs before `log_gpu_times`. That function returns without using `frames` when `GPU_TIMES` is `None`, which is the ordinary case unless `VICTORY_GPU_TIMING` populated timing data. In that case the interval frame count is still consumed, and `copystat::log(interval_frames)` can divide by 1 and retain the reported overcount. Consider moving the swap out of `log_gpu_times` (or returning whether it consumed timing data) so copy statistics always receive the interval count. This is static code-path analysis; no runtime measurement was performed.
- Hardware: RTX 3090 was at 53.7 °C, 9.4 GiB allocated, 29% utilization after the compile check. No game launch or GPU test was performed.

Assisted-by: Codex (GPT-6 Luna)
