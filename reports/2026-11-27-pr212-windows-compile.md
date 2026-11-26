# PR #212: clamp write tracking compile check

- PR: [#212](https://github.com/Belobug/VIctory-Belobug-AMD/pull/212), `888d58a7b4c4c92126d5eb112e6cc8a8a118818c`
- Environment: Linux cross-compilation to `x86_64-pc-windows-gnu`; no game runtime or GPU execution.
- Command: `cargo check --target x86_64-pc-windows-gnu -p victory-gpu --features diagnostics`
- Result: passed, exit code 0. Existing dead-code warnings appeared for `victory-shader::Module::int_value` and `victory-gpu::profile::RSP`.
- Scope: compile compatibility only. The new `storing_draw_counts_as_a_write` D3D12 host-device test was not run, so the indirect-clamp ordering behavior has not been exercised at runtime and this is not a GPU benchmark.
- Hardware: no game launch or GPU test was performed.

Assisted-by: Codex (GPT-6 Luna)
