# PR #213: Windows diagnostics compile

- PR: [#213](https://github.com/Belobug/VIctory-Belobug-AMD/pull/213), `ccbf8886e491e9ae40bc89bd9f02c510ea5392ff`
- Environment: Linux cross-compilation to `x86_64-pc-windows-gnu`; no game runtime or GPU execution.
- Command: `cargo check --target x86_64-pc-windows-gnu -p victory-gpu --features diagnostics`
- Result: passed, exit code 0. Existing dead-code warnings appeared for `victory-shader::Module::int_value` and `victory-gpu::profile::RSP`.
- Review: GPU timing and copy-stat counters accumulate since the preceding five-second log. The updated interval frame counter is used to normalize those counters, while the one-second `frames` sample remains in the main FPS line. The compilation supports type/configuration compatibility only; it does not measure live diagnostics or validate Windows runtime behavior.
- Hardware: no GPU test or game launch was performed.

Assisted-by: Codex (GPT-6 Luna)
