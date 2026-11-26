# PR #221: multi-wave Wave64 test review

PR: https://github.com/Belobug/VIctory-Belobug-AMD/pull/221  
PR head tested: `b81546bbf59b19a9b44cefb9247b27f7400cec0b`  
KernelSmoker follow-up branch: `codex/fix-pr221-wave64-test`, commit `e7b437e75930e61cc562948e26761b5e2bd5163d`

The PR adds console-flow translation for multi-wave Wave64 compute programs on 32-lane hosts. The existing `console_lanes_translate_only_for_hosts_with_that_wave_size` CPU test still required 128-thread groups to be rejected on an NVIDIA-style 32-lane target, contradicting the new support and the added multi-wave exchange test. Updated that assertion and comment to require translation.

Validation:

- Before the correction, `cargo test -p victory-shader --locked` reported 195 passed, 1 failed, 6 ignored; the failure was the stale NVIDIA two-wave expectation.
- After the correction, the full suite reported 196 passed, 0 failed, 6 ignored; an additional integration target reported 3 passed, 1 ignored.
- `cargo check -p victory-shader --tests --target x86_64-pc-windows-gnu --locked --offline` passed, compiling the Windows-only adapter-dispatch tests.
- No GPU dispatch was run. Correctable PCIe AER messages continue on the host, so the RTX 3090 runtime check remains deferred.
