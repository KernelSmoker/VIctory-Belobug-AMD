# PR #172 Linux validation — 26 November 2026

Candidate: [`dc66cba6b1762023ccb1ee13489a7bc9e05d2c9c`](https://github.com/Belobug/VIctory-Belobug-AMD/commit/dc66cba6b1762023ccb1ee13489a7bc9e05d2c9c) (`Prune tests: compare the lowering with the clean-up passes off`).

On Bazzite Linux, this command completed successfully:

```text
cargo test -p victory-shader --lib --locked --offline -- --test-threads=1
```

Result: **203 passed, 0 failed, 6 ignored**, in 2.74 seconds. The portable `structure::tests::a_branch_no_lane_takes_is_cut` test passed.

This is Linux unit-test evidence only. The PR's test module documents that its D3D12 execution path is unavailable off Windows: shader validation is a no-op and the pruned/unpruned GPU result comparison is skipped. It does not establish Windows DXIL validator behavior, NVIDIA rendering correctness, or game runtime behavior.

A Windows GNU diagnostic build was also attempted:

```text
cargo build --locked --offline --profile diag --target x86_64-pc-windows-gnu -p victory --features diagnostics
```

It stopped before compiling the project because `x86_64-w64-mingw32-dlltool` is absent. No executable was produced, and no game was launched. No GPU test or benchmark was run.
