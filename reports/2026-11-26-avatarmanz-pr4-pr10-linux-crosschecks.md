# Cross-fork Linux shader checks: AvatarManz PRs #4 and #10 — 26 November 2026

KernelSmoker ran the portable `victory-shader` library tests from the AvatarManz fork to provide limited cross-fork validation relevant to 32-lane NVIDIA hosts. These runs do not validate the Belobug source tree or either PR's GPU behavior.

| External PR | Tested head | Result |
| --- | --- | --- |
| [AvatarManz #4: wave32 LDS flag regions](https://github.com/avatarmanz/VIctory-7900XTX/pull/4) | `e29bfe273470e1c5067d5b2c5c998b84ff3cb04d` | Linux `victory-shader` tests: 202 passed, 0 failed, 2 ignored; 2.92 s |
| [AvatarManz #10: wide indirect dispatch split](https://github.com/avatarmanz/VIctory-7900XTX/pull/10) | `b390945556ab74a933afaf625e9f35700b7d9eb7` | Linux `victory-shader` tests: 204 passed, 0 failed, 2 ignored; 2.92 s |

Commands used:

```text
timeout --signal=TERM --kill-after=15s 900s cargo test -p victory-shader --lib --locked -- --test-threads=1
timeout --signal=TERM --kill-after=10s 300s cargo test -p victory-shader --lib --locked --offline -- --test-threads=1
```

The first offline dependency resolution for PR #4 could not find `zlib-rs` in the Cargo cache. The network-enabled command above fetched `zlib-rs 0.6.8` and passed. Both successful test runs emitted six Rust warnings and no test failures.

These are CPU-only Linux unit-test results. They do not confirm D3D12 hardware dispatch, WARP, Windows DXIL validation, NVIDIA rendering, in-game behavior, or FPS. No GPU workload was run. Repeated correctable PCIe AER events were present during the session, so hardware and in-game tests were withheld. No game files were read or modified.

**Next validation:** repeat PR #4's 14-dispatch check on a 32-lane D3D12 adapter, then compare PR #10 with `VICTORY_DISPATCH_SPLIT=0` and `=1` from the same checkpoint and separate shader caches after confirming host stability.

## PR #18: wave32 compute thread IDs

Tested head `7eda5e3637825d375507f108f9424c7cfdf252e9` in a separate worktree. The focused CPU-side selector

```text
timeout --signal=TERM --kill-after=10s 300s cargo test -p victory-shader --lib wave_ticket_translation_respects_the_group_shape_and_lds_budget --locked --offline -- --test-threads=1 --nocapture
```

matched **0 tests** (204 filtered). `lane_order_tests` is guarded by `#[cfg(all(test, windows))]`, so Linux cannot execute the PR's test module. The complete portable suite

```text
timeout --signal=TERM --kill-after=10s 300s cargo test -p victory-shader --lib --locked --offline -- --test-threads=1
```

passed **202 tests, 0 failed, 2 ignored** in 2.63 s, with six Rust warnings. This does not confirm the 11-shape dispatch on 32-lane hardware, D3D12/WARP behavior, gameplay, or frame-time impact. Repeated correctable PCIe AER events were present, so no GPU or in-game test was run.
