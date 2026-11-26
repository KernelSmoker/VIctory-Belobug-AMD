# PR #198: Linux test-scope cross-check

- PR head checked: `c289582ba6a2c8e3bd7a2a320fc4281fe24a1247`
- Command: `timeout --signal=TERM --kill-after=10s 300s cargo test --release --lib -p victory-shader --locked --offline f64_arithmetic_keeps_overlapping_source_pairs_distinct -- --test-threads=1 --show-output`
- Build: succeeded in 37.32 s. Existing non-fatal Rust warnings were emitted.
- Result: **0 tests ran; 202 were filtered out.** `valu_tests` is included only under `#[cfg(all(test, windows))]` in `crates/victory-shader/src/emit/mod.rs`.
- Scope: this Linux command does not exercise the f64 regression. It is not GPU, WARP, Windows, or game validation. The PR description's RX 9060 XT/WARP results remain the available runtime evidence.
