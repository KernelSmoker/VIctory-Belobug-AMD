# PR #200: Linux cross-check of normalized quotient lowering

- PR head tested: `c7f41cf00f169c868d7f24713b2d617f549ed7a6`
- Environment: Bazzite Linux, pinned Rust toolchain; offline Cargo dependencies.
- Command: `timeout --signal=TERM --kill-after=10s 300s cargo test -p victory-shader --lib normalized_quotients_as_emitted_are_the_nearest_floats --locked --offline -- --test-threads=1 --nocapture`
- Result: passed; 1 test passed, 0 failed, 191 filtered out. The focused test ran in 0.36 s after a 9.16 s test-profile build.
- Scope: CPU-side synthetic evaluation of emitted shader arithmetic. This confirms the focused Linux test on the PR head; it is not a GPU test, Windows validation, or game benchmark.
- No source changes were made to PR #200.
