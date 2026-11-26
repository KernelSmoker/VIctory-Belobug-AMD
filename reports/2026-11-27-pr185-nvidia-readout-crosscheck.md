# PR #185: NVIDIA readout crash cross-check

- PR: [#185](https://github.com/Belobug/VIctory-Belobug-AMD/pull/185), `279f0f57c318373936e92c602d3656464eba5d0b`
- Issue: [#170](https://github.com/Belobug/VIctory-Belobug-AMD/issues/170)
- Scope: reviewed the `ClearRenderTargetView` rectangle chunking and the added host regression test. The normal readout path now submits at most 16 rectangles per call and makes no call for an empty `marks` slice.
- Commands: `cargo clippy --release -p victory-gpu --no-deps --target x86_64-pc-windows-gnu`; the same command with `--features diagnostics`; `git diff --check`.
- Result: both Windows GNU cross-target clippy checks passed (exit 0). Existing warnings remain across the crate. `git diff --check` passed.
- Platform limit: `cargo test -q -p victory-gpu --lib --locked readout::` was attempted on Linux but cannot compile this Windows-only crate: Windows modules and the `windows` crate are gated out. No GPU/device runtime test was performed in this environment. PR #185 reports a passing host D3D12 regression test on an RTX 4060; this cross-check does not independently reproduce that result.
- Review note: the host test returns early when `host()` is unavailable, so a successful test run on a machine without a host D3D12 device would not exercise the calls. Its documented live RTX run is the evidence for that path.

Assisted-by: Codex (GPT-6 Luna)
