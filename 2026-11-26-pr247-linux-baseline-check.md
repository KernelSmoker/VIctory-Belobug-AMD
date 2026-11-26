# PR #247 native Linux baseline comparison

- Date: 2026-11-26 (Europe/Amsterdam)
- PR: `cb7df6b8318c853063a746879c2acad929ec2d72`
- Test: `cargo test -p victory-gpu watch:: --no-run` on PR head and base `rx9070xt` commit `81ebb7bdc9b7ca6ec2f3169973f50be77c7c803c`.

## Results

Both builds fail before tests run with the same three Linux-target compile errors: Windows-only `crate::texture` and `crate::host` modules are cfg-disabled in `list.rs`, and `host_resource` is unavailable in `resource.rs`. This is a host/target limitation reproduced on the base branch, not evidence of a regression introduced by PR #247. No GPU or game was launched.

`cargo xtask policy`: passed (`246 files ok`). `git diff --check origin/rx9070xt...HEAD`: passed. The PR author reports Windows/Wine `watch::` tests; those could not be independently reproduced here because this environment lacks the Windows test runtime/toolchain. Full workspace `cargo fmt --check` reports formatting differences across existing files, so it does not isolate this PR's changed lines.

No source changes were made. The Linux comparison does not validate the new `VirtualQuery` fallback at runtime.
