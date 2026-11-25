Release XMA IMDCT comparison at PR #35 (8851e3d), Intel i9-11900K, Bazzite 45, Rust 1.98.1. The existing bit-equivalence test passed for all 150 synthetic inputs across the three sizes. A local timing harness called the PR's uncached reference and cached implementation in alternating order. Both used the same synthetic input and preallocated output. Median ns per call across eight batches of 2,000 calls each, with the cached table warm:

| n | Uncached | Cached |
|---:|---:|---:|
|128|1936.5|752|
|256|3803|1573.5|
|512|7950.5|3518.5|

Apply timing.patch to the indicated revision and run cargo test --release --locked -p victory-formats compare_imdct_timings -- --ignored --nocapture. These timings cover the transform only.
