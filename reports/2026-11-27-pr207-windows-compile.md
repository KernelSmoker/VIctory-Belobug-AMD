# PR #207: DualSense HID path terminator review

- PR: [#207](https://github.com/Belobug/VIctory-Belobug-AMD/pull/207), `25643c8f228432cb472af5fe2c209b8a0b4d75f3`.
- Review: `device_paths` copies `len + 1` UTF-16 units from the live SetupAPI buffer, retaining its terminator before `open_pad` passes the pointer to `CreateFileW`. The added Windows test asserts every returned path has exactly one NUL, at its end. This addresses the unbounded string read described in the PR. The test enumerates interfaces on the host and requires Windows SetupAPI, so it cannot run on this Linux host.
- Command: `cargo check --target x86_64-pc-windows-gnu -p victory-hle --lib`
- Result: passed, exit code 0. This is a Windows-targeted compile check only; it does not execute SetupAPI, enumerate HID devices, or test runtime behavior. Existing dead-code/unused warnings were emitted.
- Test limitation: `cargo test -p victory-hle --lib modules::dualsense::tests::device_paths_keep_their_terminator -- --exact` on Linux failed during compilation in unrelated `victory-gpu` code (`windows` crate unresolved and `host_resource` unavailable under this target), before reaching the HID test. No test ran.
- Hardware: no game launch or GPU work; NVIDIA telemetry is unavailable.

Assisted-by: Codex (GPT-6 Luna)
